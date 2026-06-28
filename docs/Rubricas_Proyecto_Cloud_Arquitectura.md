# Rúbricas Fases Proyecto Cloud Arquitectura

# Resumen

Proyecto Integrado Cloud - Resumen


Plan de cinco evaluaciones progresivas para ecosistema cloud integrado.


| Evaluación | Fase | Tipo | Peso sugerido | Objetivo | Entregables principales | Criterio crítico | Fecha/Estado |
|---|---|---|---|---|---|---|---|
| E1 | Diseño del ecosistema y contratos | Grupal | 15% | Definir responsabilidad, contratos y dependencias. | Arquitectura, contratos, matriz dependencias. | Contrato aprobado antes de código. |  |
| E2 | Mock, modelo de datos y primera implementación | Grupal | 15% | Permitir integración temprana. | Mock, repo, README, pruebas. | Otros grupos avanzan sin bloqueo. |  |
| E3 | Desarrollo cloud funcional | Grupal | 20% | Desplegar servicio funcional en cloud free. | URL pública, BD, endpoints, deploy. | Servicio disponible y persistente. |  |
| E4 | Integración sistémica | Grupal | 25% | Demostrar integración real. | Flujos integrados, logs, errores, eventos/batch. | Al menos 2 integraciones reales. |  |
| E5 | Presentación final + defensa individual | Grupal + Individual | 25% | Demostrar sistema y comprensión individual. | Demo, arquitectura, 2 preguntas orales. | Cumplimiento objetivo + dominio individual. |  |


Fórmula recomendada: Nota final = E1*15% + E2*15% + E3*20% + E4*25% + E5*25%. E5 = Nota grupal final*60% + Nota individual oral*40%.


---

# Actividades por Grupo

Distribución sugerida para 10 grupos dentro del ecosistema Mini Marketplace Cloud.


| Grupo | Servicio | Responsabilidad | Integra con | Conceptos principales | Cloud sugerida | Entregables técnicos | Caso crítico |
|---|---|---|---|---|---|---|---|
| 1 | Frontend Marketplace / BFF | Interfaz principal: login, catálogo, carro, pedido y estado. | TODOS | REST, sesiones, pagination, manejo de errores | Vercel / Netlify / Cloudflare Pages | Frontend publicado, integración APIs, README | Flujo de compra visible para usuario |
| 2 | Identidad, usuarios y sesiones | Registro, login, sesión/token y roles. | TODOS | Sesiones, JWT/session token, SQL/NoSQL, seguridad básica | Supabase Auth / Firebase Auth / Render + Postgres | API/auth, usuarios, validación roles | Expiración y validación de sesión |
| 3 | Catálogo de productos | Productos, categorías, precios y stock visible. | TODOS | REST, pagination, SQL/NoSQL, versionado API | Supabase / Firebase / Render | Endpoints catálogo, filtros, paginación | Búsqueda y paginación estable |
| 4 | Carro y checkout | Carro, cálculo de totales e intención de pedido. | TODOS | Sesiones, REST, idempotencia, race condition simple | Render / Cloudflare Workers / Supabase | API cart/checkout, idempotency-key | Doble click no crea doble pedido |
|  | Inventario y concurrencia | Stock, reserva, liberación y confirmación. | TODOS | Concurrencia, race condition, locks, idempotencia, escalamiento horizontal | Supabase Postgres / Neon / Oracle DB | API inventario, prueba concurrente | Dos compras por último stock: solo una exitosa |
| 5 | Pedidos / Order Management | Ciclo de vida del pedido y publicación de eventos. | TODOS | REST, eventos, estados, idempotencia, eventual consistency, SQL | Render / Railway / Oracle Cloud / Supabase | API pedidos, eventos OrderCreated | Estados coherentes ante pago/stock/despacho |
| 6 | Despacho / logística | Solicitud de despacho y estados logísticos. | TODOS | REST, eventos, consistencia eventual, manejo de errores | Render / Supabase / Firebase | API shipments, eventos ShipmentCreated/Delivered | Pedido pasa a despacho y entrega |
| 8 | Pago simulado | Procesar pagos aprobados, rechazados o pendientes. | TODOS | Eventos, idempotencia, race condition, consistencia eventual | Render / Cloudflare Workers / Supabase | API pagos, eventos PaymentApproved/Rejected | Confirmación duplicada no duplica pago |
| 8 | Notificaciones | Consumir eventos y crear notificaciones simuladas. | TODOS | Pub/Sub, eventos, NoSQL, pagination, idempotencia | Firebase / Supabase Realtime / Upstash | API notifications, consumidor eventos | Evento duplicado no crea doble notificación |
| 7 | Reportería / Batch / Streaming | Dashboard/reportes con datos consolidados. | TODOS | Batch, streaming, storage, SQL/NoSQL, eventual consistency | Supabase / Firebase / Cloudflare R2 / GitHub Actions | Reportes ventas, batch recalculation | Consolidar venta por eventos y batch |


---

# E1 Contratos M16 Junio

E1 Contratos


Definir qué construirá cada grupo, qué datos administrará, qué contratos expondrá y cómo se integrará con el resto.


## OBJETIVO

Definir qué construirá cada grupo, qué datos administrará, qué contratos expondrá y cómo se integrará con el resto.


## ACTIVIDADES GRUPALES

| N° | Actividad | Descripción | Dependencia | Evidencia esperada | Responsable | Estado | Observación |
|---|---|---|---|---|---|---|---|
| 1 | Definir responsabilidad del servicio | Acordar qué dominio resuelve el grupo y qué queda fuera de alcance. | Asignación ecosistema | Responsabilidad escrita |  |  |  |
| 2 | Identificar dueño del dato | Definir datos propios y datos consultados. | Diseño de dominio | Ownership de datos |  |  |  |
| 3 | Diseñar contratos REST | Definir endpoints, request, response, errores y ejemplos JSON. | Responsabilidad servicio | Contrato Markdown/OpenAPI |  |  |  |
| 4 | Definir eventos | Indicar eventos que publica/consume, payload y versión. | Flujos integración | Contrato de evento |  |  |  |
| 5 | Mapear dependencias | Levantar consumidores, productores y dependencias. | Contratos | Matriz de dependencias |  |  |  |
| 6 | Diseñar arquitectura | Crear diagrama de componentes y flujo de datos. | Contratos y dependencias | Diagrama C4 simple |  |  |  |


## ENTREGABLES

| N° | Entregable | Formato sugerido | Obligatorio | Estado | Link/Evidencia | Responsable | Observación |
|---|---|---|---|---|---|---|---|
| 1 | Documento de arquitectura | PDF/Markdown/Docs | Sí |  |  |  |  |
| 2 | Contrato REST | OpenAPI/Swagger o Markdown | Sí |  |  |  |  |
| 3 | Contrato de eventos | JSON Schema/Markdown | Si aplica |  |  |  |  |
| 4 | Matriz de dependencias | Tabla | Sí |  |  |  |  |
| 5 | Diagrama de integración | Draw.io/Mermaid | Sí |  |  |  |  |


## RÚBRICA GRUPAL

| Criterio | Peso | Excelente | Bueno | Suficiente | Insuficiente | Nota 1-7 | Comentario |
|---|---|---|---|---|---|---|---|
| Definición del servicio y responsabilidad | 20% | Responsabilidad clara y acotada. | Clara con límites menores difusos. | Se entiende parcialmente. | No queda claro qué resuelve. |  |  |
| Contratos REST | 25% | Endpoints completos con request, response, errores y ejemplos. | Definidos con detalles faltantes. | Incompletos o poco precisos. | No existen contratos utilizables. |  |  |
| Integración con otros grupos | 20% | Dependencias reales y flujos claros. | Dependencias principales identificadas. | Integraciones generales. | No define integración real. |  |  |
| Modelo de datos inicial | 15% | Entidades, campos y dueño del dato claros. | Modelo entendible con vacíos menores. | Modelo básico. | No presenta modelo. |  |  |
| Diseño arquitectónico | 10% | Diagrama claro con componentes y flujo. | Diagrama entendible. | Diagrama básico. | No se entiende o no existe. |  |  |
| Riesgos, supuestos y errores | 10% | Riesgos y errores relevantes. | Algunos riesgos. | Riesgos superficiales. | No considera riesgos. |  |  |


---

# E2 Mock M23 Junio

E2 Mock


Construir contratos ejecutables o mocks funcionales para que otros grupos puedan avanzar.


## OBJETIVO

Construir contratos ejecutables o mocks funcionales para que otros grupos puedan avanzar.


## ACTIVIDADES GRUPALES

| N° | Actividad | Descripción | Dependencia | Evidencia esperada | Responsable | Estado | Observación |
|---|---|---|---|---|---|---|---|
| 1 | Crear repositorio | Repositorio GitHub con estructura inicial, ramas y README. | E1 aprobada | Link repo |  |  |  |
| 2 | Construir mock de endpoints | Publicar mock o servicio inicial para desbloquear dependencias. | Contrato REST | URL mock |  |  |  |
| 3 | Refinar modelo de datos | Ajustar entidades, campos, claves e índices básicos. | Feedback E1 | Modelo actualizado |  |  |  |
| 4 | Crear colección de pruebas | Probar endpoints con Postman, Bruno o Insomnia. | Mock disponible | Colección exportada |  |  |  |
| 5 | Alinear integración temprana | Validar con consumidores que el contrato se entiende. | Matriz dependencias | Evidencia prueba |  |  |  |
| 6 | Iniciar servicio real | Primer esqueleto técnico conectado al framework elegido. | Repo creado | Primer endpoint real |  |  |  |


## ENTREGABLES

| N° | Entregable | Formato sugerido | Obligatorio | Estado | Link/Evidencia | Responsable | Observación |
|---|---|---|---|---|---|---|---|
| 1 | Repositorio GitHub | URL | Sí |  |  |  |  |
| 2 | Mock público o servicio inicial | URL | Sí |  |  |  |  |
| 3 | README inicial | Markdown | Sí |  |  |  |  |
| 4 | Colección de pruebas | Postman/Bruno/Insomnia | Sí |  |  |  |  |
| 5 | Modelo de datos actualizado | Diagrama/Tabla | Sí |  |  |  |  |


## RÚBRICA GRUPAL

| Criterio | Peso | Excelente | Bueno | Suficiente | Insuficiente | Nota 1-7 | Comentario |
|---|---|---|---|---|---|---|---|
| Mock funcional | 25% | Disponible, estable y alineado al contrato. | Funcional con detalles menores. | Parcial o incompleto. | No existe mock. |  |  |
| Repositorio y estructura base | 15% | Ordenado, README y convenciones claras. | Funcional, mejorable. | Básico y poco documentado. | Inexistente o desordenado. |  |  |
| Modelo de datos refinado | 20% | Consistente, relaciones y tipos claros. | Entendible con vacíos. | Muy general. | No hay modelo claro. |  |  |
| Pruebas de contrato | 15% | Prueba todos los endpoints principales. | Prueba la mayoría. | Pruebas mínimas. | Sin evidencia. |  |  |
| Alineación con otros grupos | 15% | Permite avanzar sin bloqueo. | Permite integración parcial. | Requiere muchas aclaraciones. | No permite integración. |  |  |
| Avance técnico inicial | 10% | Implementación inicial coherente. | Avance básico. | Muy limitado. | Sin avance real. |  |  |


---

# E3 Cloud M30 Julio

E3 Cloud


Desplegar una primera versión funcional del servicio en cloud free con persistencia y endpoints principales.


## OBJETIVO

Desplegar una primera versión funcional del servicio en cloud free con persistencia y endpoints principales.


## ACTIVIDADES GRUPALES

| N° | Actividad | Descripción | Dependencia | Evidencia esperada | Responsable | Estado | Observación |
|---|---|---|---|---|---|---|---|
| 1 | Desplegar servicio cloud | Publicar URL funcional en cloud free. | Servicio inicial | URL pública |  |  |  |
| 2 | Implementar endpoints principales | Construir funcionalidades mínimas alineadas al contrato. | Contrato vigente | Pruebas funcionales |  |  |  |
| 3 | Agregar persistencia | Usar SQL o NoSQL según el dominio. | Modelo de datos | Evidencia BD |  |  |  |
| 4 | Manejar errores | Responder con códigos HTTP y formato estándar. | Endpoints | Casos de error |  |  |  |
| 5 | Configurar variables de entorno | Separar configuración y secretos. | Deploy cloud | README/env example |  |  |  |
| 6 | Implementar CI/CD o deploy documentado | Automatizar o documentar despliegue reproducible. | Repo GitHub | Pipeline o pasos |  |  |  |


## ENTREGABLES

| N° | Entregable | Formato sugerido | Obligatorio | Estado | Link/Evidencia | Responsable | Observación |
|---|---|---|---|---|---|---|---|
| 1 | URL servicio cloud | URL | Sí |  |  |  |  |
| 2 | Base de datos funcionando | SQL/NoSQL | Sí |  |  |  |  |
| 3 | Documentación endpoints | OpenAPI/Markdown | Sí |  |  |  |  |
| 4 | Evidencia CI/CD o deploy | GitHub Actions/capturas/README | Sí |  |  |  |  |
| 5 | Pruebas funcionales | Colección o reporte | Sí |  |  |  |  |


## RÚBRICA GRUPAL

| Criterio | Peso | Excelente | Bueno | Suficiente | Insuficiente | Nota 1-7 | Comentario |
|---|---|---|---|---|---|---|---|
| Servicio desplegado en cloud | 20% | Disponible públicamente y estable. | Desplegado con fallas menores. | Parcialmente disponible. | No hay despliegue. |  |  |
| Implementación de endpoints | 25% | Principales completos y alineados. | Mayoría funciona. | Parcialmente funcionales. | No funcionan/no existen. |  |  |
| Persistencia de datos | 15% | Uso correcto de SQL/NoSQL. | Funcional con detalles. | Básica. | Sin persistencia real. |  |  |
| Manejo de errores | 15% | Códigos HTTP y formato estándar. | Aceptable. | Poco consistente. | Inadecuado. |  |  |
| CI/CD o despliegue automatizado | 10% | Deploy automatizado. | Deploy documentado. | Manual poco claro. | Sin evidencia. |  |  |
| Documentación técnica | 10% | README completo y reproducible. | Suficiente. | Incompleto. | No útil. |  |  |
| Seguridad/configuración básica | 5% | Variables de entorno y sin secretos. | Aceptable. | Débil. | Expone secretos. |  |  |


---

# E4 Integración M7 Julio

E4 Integración


Demostrar integración real entre servicios mediante REST, eventos, pub/sub, batch, streaming o almacenamiento.


## OBJETIVO

Demostrar integración real entre servicios mediante REST, eventos, pub/sub, batch, streaming o almacenamiento.


## ACTIVIDADES GRUPALES

| N° | Actividad | Descripción | Dependencia | Evidencia esperada | Responsable | Estado | Observación |
|---|---|---|---|---|---|---|---|
| 1 | Integrar con al menos 2 grupos | Consumir/proveer servicios reales del ecosistema. | E3 funcional | Flujo integrado |  |  |  |
| 2 | Validar contratos reales | Comparar implementación contra contrato acordado. | Contratos E1/E2 | Compatibilidad |  |  |  |
| 3 | Probar casos exitosos y fallidos | Demostrar errores de integración, timeouts o respuestas inválidas. | Integración real | Pruebas documentadas |  |  |  |
| 4 | Agregar trazabilidad | Usar X-Request-Id, X-Correlation-Id o logs equivalentes. | Flujos integrados | Logs |  |  |  |
| 5 | Aplicar patrón técnico asignado | Eventos, pub/sub, batch, streaming, idempotencia, concurrencia o consistencia eventual. | Diseño grupo | Demo técnica |  |  |  |
| 6 | Actualizar arquitectura | Reflejar dependencias reales y decisiones finales. | Integraciones | Diagrama actualizado |  |  |  |


## ENTREGABLES

| N° | Entregable | Formato sugerido | Obligatorio | Estado | Link/Evidencia | Responsable | Observación |
|---|---|---|---|---|---|---|---|
| 1 | Flujo integrado | Demo/Video/Capturas | Sí |  |  |  |  |
| 2 | Pruebas integración | Colección/reporte | Sí |  |  |  |  |
| 3 | Logs/trazabilidad | Capturas/logs | Sí |  |  |  |  |
| 4 | Diagrama actualizado | Draw.io/Mermaid/Imagen | Sí |  |  |  |  |
| 5 | Evidencia patrón técnico | Demo/código/prueba | Sí |  |  |  |  |


## RÚBRICA GRUPAL

| Criterio | Peso | Excelente | Bueno | Suficiente | Insuficiente | Nota 1-7 | Comentario |
|---|---|---|---|---|---|---|---|
| Integración real con otros grupos | 25% | Integra con 2+ grupos y flujos completos. | 2 integraciones con detalles menores. | Integración parcial. | Sin integración real. |  |  |
| Cumplimiento de contratos | 20% | Respeta contratos y versiona cambios. | Ajustes menores. | Diferencias relevantes. | Rompe/no respeta contratos. |  |  |
| Manejo de errores entre servicios | 15% | Maneja errores, timeouts y estados alternativos. | Aceptable. | Limitado. | No maneja errores. |  |  |
| Trazabilidad | 10% | CorrelationId/logs claros. | Logs suficientes. | Logs básicos. | Sin trazabilidad. |  |  |
| Uso de patrones técnicos asignados | 20% | Aplica correctamente conceptos asignados. | Aplica con vacíos menores. | Superficial. | No aplica. |  |  |
| Pruebas de integración | 10% | Pruebas exitosas y fallidas documentadas. | Pruebas principales. | Incompletas. | Sin evidencia. |  |  |


---

# E5 Final M14 Julio

E5 Final


Demostrar el ecosistema funcionando, explicar la arquitectura y validar comprensión individual.


## OBJETIVO

Demostrar el ecosistema funcionando, explicar la arquitectura y validar comprensión individual.


## ACTIVIDADES GRUPALES

| N° | Actividad | Descripción | Dependencia | Evidencia esperada | Responsable | Estado | Observación |
|---|---|---|---|---|---|---|---|
| 1 | Preparar demo punta a punta | Mostrar el servicio funcionando dentro del ecosistema. | E4 integrada | Demo final |  |  |  |
| 2 | Explicar arquitectura final | Presentar decisiones, trade-offs y dependencias. | Diagrama actualizado | Presentación |  |  |  |
| 3 | Evidenciar cumplimiento del objetivo | Comparar objetivo inicial contra resultado final. | Todas las fases | Slide cumplimiento |  |  |  |
| 4 | Mostrar cloud y operación | URL, repo, BD, logs, deploy y pruebas. | Servicio cloud | Evidencia técnica |  |  |  |
| 5 | Reflexionar sobre problemas | Explicar errores, bloqueos, aprendizajes y mejoras. | Experiencia proyecto | Conclusiones |  |  |  |
| 6 | Defensa individual | Cada estudiante responde 2 preguntas orales y evidencia participación. | Presentación grupal | Registro individual |  |  |  |


## ENTREGABLES

| N° | Entregable | Formato sugerido | Obligatorio | Estado | Link/Evidencia | Responsable | Observación |
|---|---|---|---|---|---|---|---|
| 1 | Presentación final | Slides | Sí |  |  |  |  |
| 2 | Demo funcional | Sistema/Video | Sí |  |  |  |  |
| 3 | Arquitectura final | Diagrama | Sí |  |  |  |  |
| 4 | Repositorio final | URL | Sí |  |  |  |  |
| 5 | Registro preguntas individuales | Tabla evaluación | Sí |  |  |  |  |


## RÚBRICA GRUPAL

| Criterio | Peso | Excelente | Bueno | Suficiente | Insuficiente | Nota 1-7 | Comentario |
|---|---|---|---|---|---|---|---|
| Cumplimiento del objetivo del grupo | 25% | Cumple claramente su objetivo. | Cumple con detalles menores. | Cumple parcialmente. | No cumple objetivo. |  |  |
| Demostración funcional | 25% | Demo funcionando e integrada. | Funcionamiento principal. | Demo parcial/inestable. | No logra demostrar. |  |  |
| Explicación arquitectónica | 15% | Diseño, decisiones, dependencias y trade-offs claros. | Explicación suficiente. | Superficial. | No logra explicar. |  |  |
| Integración con el ecosistema | 15% | Integración real y relevante. | Funcional con detalles. | Parcial. | No evidencia integración. |  |  |
| Uso de cloud y herramientas | 10% | Cloud, repo, deploy y persistencia correctos. | Uso aceptable. | Básico o poco claro. | Sin uso real. |  |  |
| Reflexión técnica | 10% | Errores, aprendizajes y mejoras con criterio. | Aprendizajes relevantes. | Débil. | Sin reflexión. |  |  |


---

# Rubrica Individual

Rúbrica Evaluación Individual Oral


Evaluación individual con 3 componentes: cumplimiento grupal, pregunta de presentación y pregunta técnica.


| Estudiante | Grupo | Cumplimiento grupal | Pregunta presentación | Pregunta técnica | Promedio individual | Pregunta 1 realizada | Pregunta 2 realizada |
|---|---|---|---|---|---|---|---|


| Componente | Excelente 6,5-7,0 | Bueno 5,5-6,4 | Suficiente 4,0-5,4 | Insuficiente 1,0-3,9 | Ejemplos de preguntas |
|---|---|---|---|---|---|
| Cumplimiento grupal | Participó activamente, conoce sistema y domina aporte. | Participó y conoce su parte. | Participación parcial y visión incompleta. | No evidencia participación. | ¿Qué construiste tú? ¿Cómo impactó al grupo? |
| Pregunta presentación | Responde claro y conecta con arquitectura. | Responde correctamente. | Respuesta general con vacíos. | Desconoce el trabajo. | ¿Con qué grupos se integraron? ¿Qué contrato fue clave? |
| Pregunta técnica | Explica concepto, implementación y decisión. | Explica concepto correctamente. | Comprensión básica sin aterrizar. | No comprende el concepto. | ¿Qué es idempotencia? ¿Qué es race condition? |
| Fórmula | Promedio individual = promedio de los 3 componentes anteriores. |  |  |  |  |


---

# Cloud Free

Opciones Cloud Free sugeridas


Opciones para desplegar frontend, APIs, bases de datos, eventos, storage y CI/CD.


| Necesidad | Opción | Uso sugerido | Observación docente |
|---|---|---|---|
| Frontend | Vercel / Netlify / Cloudflare Pages | Web apps, dashboards, front marketplace | Ideal para Grupo 1. |
| APIs REST | Render Free / Cloudflare Workers / Oracle Cloud Free Tier | Microservicios y APIs livianas | Cuidar límites y suspensión por inactividad. |
| SQL | Supabase Free / Neon Free / Oracle Autonomous DB | Postgres o base relacional | Recomendado para pedidos, inventario y pagos. |
| NoSQL | Firebase Firestore / Supabase | Catálogo, sesiones, notificaciones | Útil para documentos y eventos simples. |
| Eventos / PubSub | Upstash Kafka Free / Cloudflare Queues / Supabase Realtime / Firebase | Pub/Sub, streaming simple, consumidores | Al menos algunos grupos deben implementarlo. |
| Storage | Supabase Storage / Firebase Storage / Cloudflare R2 | Archivos, boletas, cargas batch | Útil para reportería o documentos simulados. |
| CI/CD | GitHub Actions | Build, test y deploy | Todos deberían evidenciarlo o documentar deploy. |


---

# Reuniones

| Miercoles | 22 hrs |
|---|---|
| Sabado | 23 hrs |


Runion sabado 13 de junio


Establecer tecnologias a usar


---

