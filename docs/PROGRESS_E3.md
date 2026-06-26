# Progreso Entregable 3 (E3)

Este documento es de uso interno del equipo para hacer seguimiento de los ítems logrados para la evaluación de la E3 y asegurar la máxima puntuación (7.0).

## 🚀 Ítems Completados

### 1. Implementación de Endpoints y Pruebas (100% de la rúbrica)
- [x] Endpoints completamente alineados al contrato OpenAPI.
- [x] Gestión correcta del ciclo de vida del despacho, cálculo de costos, órdenes e historial.
- [x] Pruebas Funcionales: Tenemos nuestra colección de Postman configurada (`tests/postman_collection.json`). **Estrategia acordada:** Haremos la demostración en vivo (live-demo) utilizando esta colección de peticiones reales para cumplir el hito, ahorrándonos programación innecesaria de scripts de prueba.

### 2. Seguridad y Configuración (100% de la rúbrica)
- [x] Eliminadas credenciales quemadas (*hardcoded*) de base de datos en `database.py`.
- [x] Dependencia añadida a `python-dotenv`.
- [x] Añadido archivo `.env.example` para facilitar la inyección de variables de entorno (tanto locales como en producción).
- [x] Validación estricta que impide que la API se levante si no detecta `DATABASE_URL`.

---

## ⏳ Próximos Pasos (Pendientes)

### 3. Automatización (CI/CD)
- [ ] Implementar un pipeline de GitHub Actions (ej. para chequear estilo, tests base o build).

### 4. Documentación y Despliegue
- [ ] Actualizar `README.md` indicando dónde está el nuevo servicio desplegado y cómo levantarlo en Render.
- [ ] Conectar Render a esta nueva rama (`main` o `E3-dev`) inyectando las variables de entorno.
