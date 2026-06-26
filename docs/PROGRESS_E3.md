# Progreso Entregable 3 (E3)

Este documento es de uso interno del equipo para hacer seguimiento de los ítems logrados para la evaluación de la E3 y asegurar la máxima puntuación (7.0).

## 🚀 Ítems Completados

### 1. Implementación de Endpoints y Pruebas (100% de la rúbrica)
- [x] Endpoints completamente alineados al contrato OpenAPI.
- [x] Gestión correcta del ciclo de vida del despacho, cálculo de costos, órdenes e historial.
- [x] Pruebas Funcionales: Tenemos nuestra colección de Postman configurada (`tests/postman_collection.json`). **Estrategia acordada:** Haremos la demostración en vivo (live-demo) utilizando esta colección de peticiones reales para cumplir el hito.
- [x] Agregamos pruebas automatizadas en código con `pytest` (`tests/test_api.py`) para validar los "Happy Paths" de la API y manejo de errores.

### 2. Seguridad y Configuración (100% de la rúbrica)
- [x] Eliminadas credenciales quemadas (*hardcoded*) de base de datos en `database.py`.
- [x] Dependencia añadida a `python-dotenv`.
- [x] Añadido archivo `.env.example` para facilitar la inyección de variables de entorno (tanto locales como en producción).
- [x] Validación estricta que impide que la API se levante si no detecta `DATABASE_URL`.

### 3. Manejo de Errores y Contratos (100% de la rúbrica)
- [x] Verificado que el código (`app/schemas.py`) cumple estrictamente con el formato estándar exigido en la `guia-y-lineamiento-de-desarrollo.md` (formato corto sin `status` ni `timestamp`).
- [x] Creada la versión `v1.3` del contrato en LaTeX (`docs/contratos/v1.3/G6_Contrato_API_Despacho_v1.3.tex`) para reflejar correctamente este lineamiento y corregir los ejemplos desactualizados de la `v1.2`.

### 4. Automatización y CI/CD (100% de la rúbrica) - *¡Nuevo!*
- [x] **Implementación de GitHub Actions (`.github/workflows/ci.yml`)**: 
  - **¿Qué es esto?** Es nuestro pipeline de Integración Continua (CI).
  - **¿Cómo funciona?** Creamos un archivo YAML (configuración) en la carpeta especial `.github/workflows/`. GitHub detecta esta carpeta automáticamente en su nube. No tuvimos que instalar ninguna aplicación externa.
  - **¿Qué hace exactamente?** Le dice a GitHub: *"Cada vez que alguien de nuestro grupo haga un `push` a las ramas `main` o `E3-dev`, préstame un servidor Ubuntu, instala Python, instala nuestras librerías (`requirements.txt`), y corre automáticamente el comando `pytest` para probar que nuestro código funcione correctamente."*
  - **¿Por qué lo hicimos?** Si en el futuro alguien hace un cambio que rompe la aplicación, GitHub pondrá una "X" roja antes de que se publique en producción. Además, es un requisito clave para la máxima nota en Arquitectura Cloud.
  - **Refinamiento Técnico (PostgreSQL en Actions):** Descubrimos que nuestras pruebas fallaban en la nube porque usamos tipos de datos exclusivos de PostgreSQL (como `JSONB`). Lo solucionamos configurando los "Services" de GitHub Actions para que levante un motor real de PostgreSQL 15, cree la base de datos, corra los tests y luego destruya todo. ¡Entorno 100% aislado!

### 5. Refinamiento de Contratos y Arquitectura - *¡Nuevo!*
- [x] **Refactorización Respuesta 201:** Se detectó que el endpoint `POST /api/v1/shipments` devolvía un arreglo plano, lo que es mala práctica. Se refactorizó el código (usando un nuevo esquema Pydantic) para devolver un objeto JSON estructurado: `{"shipmentIds": ["SHP-xxx"]}`.
- [x] **Sincronización Total de Contratos:** Para no perder puntos en evaluación, actualizamos todos los frentes tras el cambio:
  - Generamos nuevamente el `openapi.yaml` directamente de la memoria de FastAPI mediante un script, garantizando CERO errores humanos en el Swagger.
  - Actualizamos manualmente el contrato PDF/LaTeX (`G6_Contrato_API_Despacho_v1.3.tex`) para que refleje esta nueva estructura.
- [x] **Quality Gate (Validación Estricta):** Implementamos una prueba automatizada (`test_openapi_is_up_to_date`) en `test_api.py`. Si alguien hace un cambio en la API y olvida actualizar el contrato `openapi.yaml`, **los tests fallan** y exigen correr `python scripts/dump_openapi.py`. ¡Garantía de calidad nivel senior!

### 6. Documentación
- [x] Actualizado el `README.md` explicando detalladamente la ejecución local, las pruebas automatizadas y la nueva estrategia de Integración Continua (CI).

---

## ⏳ Próximos Pasos (Pendientes)

### Despliegue en la Nube
- [ ] Conectar o validar que Render está leyendo correctamente esta nueva rama (`main` o `E3-dev`) e inyectando las variables de entorno correspondientes para el Despliegue Continuo (CD).
