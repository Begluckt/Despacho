# Documentación Técnica de la API de Despacho (Grupo 6) - v1.2

Este documento consolida la arquitectura lógica y los detalles de implementación de los endpoints, la máquina de estados y las normativas transversales aplicadas a la API del Grupo 6 (Logística y Despacho) en su versión 1.2.

---

## 1. Normativas Transversales (Alineación v1.2)

Con el fin de asegurar una integración fluida en el entorno de microservicios, el sistema implementa:

1. **CamelCase:** Todas las variables de request y response JSON están estrictamente en `camelCase` (ej. `orderId`, `customerName`, `weightKg`).
2. **Paginación Estándar:** Las respuestas que devuelven listas incluyen un objeto `pagination` unificado con `total`, `page`, `size` y `totalPages`.
3. **Manejo Unificado de Errores:** En caso de fallas, el servicio responde con una estructura unificada:
   ```json
   {
     "error": {
       "code": "VALIDATION_ERROR",
       "message": "Mensaje descriptivo del error"
     }
   }
   ```
4. **Trazabilidad (Headers Requeridos):** Todo endpoint transaccional valida la existencia de `X-Request-Id`, `X-Correlation-Id` y `X-Consumer` para asegurar la trazabilidad.

---

## 2. Endpoints Principales

### 2.1 Cotización (`POST /api/v1/shipments/quotes`)
Endpoint libre (sin requerir trazabilidad obligatoria) que permite al Checkout (Grupo 4) consultar el valor exacto de un envío antes de generar una orden.
- **Entrada:** La ciudad de destino (`city`) y una lista de paquetes (`packages`). Cada paquete define su `originCd` (Centro de Distribución: NORTE, CENTRO, SUR), peso real y dimensiones (Largo, Ancho, Alto).
- **Lógica:** Implementa el motor de *Pricing* dinámico. Aplica peso volumétrico si este es mayor al peso real, y cruza el origen con la "macro-zona" de destino para fijar tarifas locales o extremas.
- **Salida:** `totalShippingCost` en CLP.

### 2.2 Creación de Despacho (`POST /api/v1/shipments`)
Genera un despacho oficial en la base de datos tras recibir la solicitud del Sistema de Órdenes.
- **Validaciones:** Requiere los Headers de trazabilidad, e impide campos vacíos (como `orderId` en blanco).
- **Comportamiento:** Asigna automáticamente el estado `PENDING` e inserta el evento `ShipmentCreated` en la tabla `outbox_events` (Patrón Transactional Outbox) para notificar asíncronamente a los demás grupos.
- **Salida:** Retorna un arreglo con el `shipmentId` generado (ej. `["SHP-1234abcd"]`).

### 2.3 Listado y Búsqueda (`GET /api/v1/shipments`)
Devuelve todos los despachos bajo un formato paginado estándar.
- Permite filtrar envíos por `orderId` usando Query Params (ej. `?orderId=ORD-123`).

### 2.4 Actualización de Estados (`PATCH /api/v1/shipments/{shipmentId}`)
Permite mover un despacho a través de la máquina de estados.
- **Lógica:** Verifica que la transición sea válida. Si lo es, actualiza el estado en la tabla `shipments` y guarda la transición detallada (fecha, estado previo, estado nuevo) en `shipment_status_history`. Además, emite un evento Outbox (`ShipmentStatusUpdated`).

### 2.5 Historial de Envío (`GET /api/v1/shipments/{shipmentId}/history`)
Expone la tabla `shipment_status_history` vinculada a un ID específico, garantizando inmutabilidad y permitiendo auditoría temporal.

---

## 3. Máquina de Estados y Sub-Estados

El ciclo de vida logístico de un envío dentro del módulo del Grupo 6 está gobernado por una máquina de estados estricta. Ningún envío puede saltarse el flujo natural ni transicionar a un estado ilógico (ej. no se puede pasar de `DELIVERED` a `IN_TRANSIT`).

### Estados Base Autorizados:
- **`PENDING`**: Estado inicial y por defecto. Significa que el envío fue creado en la base de datos, las tarifas están calculadas, y el paquete está a la espera de ser procesado o recolectado en su respectivo Centro de Distribución.
- **`IN_TRANSIT`**: El paquete abandonó el almacén logístico de origen y se encuentra arriba de un camión de transporte rumbo a la dirección o ciudad del cliente.
- **`DELIVERED`**: Fin del flujo regular exitoso. El paquete fue entregado de manera efectiva en la dirección acordada.
- **`CANCELLED`**: Estado terminal abortado temprano. Solo es posible transicionar a este estado si el paquete aún estaba en `PENDING`. Si ya estaba en tránsito, no puede ser cancelado por el usuario, debe ser rechazado en puerta.
- **`FAILED`**: Estado de fallo de logística. Representa un problema durante el tránsito (ej. camión accidentado, dirección falsa, destinatario inubicable).
- **`RETURNED`**: Estado terminal logístico. Ocurre si un paquete que estaba marcado como entregado (o tras un fallo logístico prolongado) regresa al Centro de Distribución de origen (Logística Inversa).

### Transiciones Válidas (Grafo)
El servidor (`PATCH`) solo autorizará las siguientes rutas:
- `PENDING` ➔ `IN_TRANSIT` | `CANCELLED`
- `IN_TRANSIT` ➔ `DELIVERED` | `FAILED`
- `DELIVERED` ➔ `RETURNED`
- `CANCELLED`, `FAILED`, `RETURNED` ➔ *(Estados terminales, no permiten salidas)*.

---

## 4. Patrón Transactional Outbox (Eventos Asíncronos)

Dado que un sistema logístico no debe detener la compra de un cliente si hay latencia en el envío de mensajes, no utilizamos comunicación síncrona hacia otros microservicios tras crear un envío.
En su lugar, todas las mutaciones críticas (creación y actualización de estado) insertan un registro JSON estructurado en la tabla `outbox_events` bajo la misma transacción de PostgreSQL. Un *worker* en segundo plano será el encargado futuro de desencolar esos registros y despacharlos hacia Rabbit/Kafka/EventGrid de manera garantizada y resiliente.
