# Fase 2 - Analizar practicas de ingenieria

## Objetivo

Evaluar el estado actual del proyecto para identificar buenas practicas, riesgos tecnicos y reglas que permitan mantener calidad sin frenar velocidad de desarrollo.

## 1) Buenas practicas identificadas

### Arquitectura

1. Separacion clara de responsabilidades entre frontend (`frontend/`) y backend (`backend/`).
2. Backend organizado por app con entrypoint y rutas desacopladas (`backend/app/main.py`, `backend/app/routes.py`).

### Tipado y contratos

3. Uso de modelos Pydantic para contratos de respuesta y request en FastAPI.
4. Tipos explicitos en frontend para movimientos financieros y KPIs (`frontend/src/lib/financial-types.ts`).

### Testing

5. Cobertura de pruebas de utilidades de negocio en frontend (`frontend/src/lib/financial-utils.test.ts`).
6. Pruebas de endpoints y filtros en backend (`backend/tests/test_routes.py`).

### DX y ejecucion local

7. Stack de herramientas moderno y consistente (Vite, Vitest, ESLint, TypeScript, FastAPI, Pytest).
8. Docker Compose documentado como entrada unica para levantar el entorno local.

## 2) Malas practicas o riesgos detectados

### Arquitectura y dominio

1. Logica de negocio relevante concentrada en `routes.py` (archivo de alto acoplamiento y crecimiento rapido).
2. No existe una capa de servicios/repositorio para preparar una migracion a datos reales.

### Seguridad y configuracion

3. CORS abierto con `allow_origins=["*"]` sin restricciones por entorno.
4. Configuracion de entorno dispersa y fragil cuando no se usa Docker (dependencia del proxy o variables manuales).

### Calidad de datos y evolucion

5. Dependencia total de mock data en backend; riesgo de desviaciones cuando se conecte una fuente real.
6. No hay versionado explicito de la API para cambios futuros de contrato.

### Testing y confiabilidad

7. Faltan pruebas de integracion frontend-backend (hoy se prueban piezas, no el flujo completo).
8. No se observan pruebas de rendimiento basicas para endpoints de agregacion y resumen.

### Documentacion y onboarding

9. Reglas y convenciones repartidas en varios archivos con nomenclatura inconsistente.
10. Riesgo de errores por naming y ortografia en documentos operativos (impacta busqueda y mantenimiento).

## 3) Reglas propuestas para mitigar riesgos y preservar patrones utiles

### Regla A - Mantener contratos tipados y sincronizados

- **Que exige:** cualquier cambio de schema en backend debe reflejarse en tipos de frontend y pruebas en la misma tarea.
- **Riesgo que mitiga:** roturas silenciosas en UI y deuda de integracion.
- **Patron util que preserva:** tipado fuerte y contratos explicitos.

### Regla B - Extraer logica de negocio fuera de rutas

- **Que exige:** mover calculos y reglas de negocio a una capa `services` en backend cuando una ruta supere complejidad moderada.
- **Riesgo que mitiga:** archivos monoliticos y baja mantenibilidad.
- **Patron util que preserva:** separacion por responsabilidad.

### Regla C - Configuracion por entorno segura por defecto

- **Que exige:** definir CORS y origenes de API por variables de entorno con defaults seguros.
- **Riesgo que mitiga:** exposicion innecesaria y fallas por configuracion manual.
- **Patron util que preserva:** despliegue flexible local/CI/prod.

### Regla D - Pruebas obligatorias por cambio funcional

- **Que exige:** cada cambio funcional debe incluir o actualizar pruebas unitarias y, cuando aplique, una prueba de integracion.
- **Riesgo que mitiga:** regresiones en calculo de metricas y filtros.
- **Patron util que preserva:** cultura de calidad continua.

### Regla E - Versionado y compatibilidad de API

- **Que exige:** introducir prefijo de version (`/api/v1`) para nuevos endpoints o cambios no compatibles.
- **Riesgo que mitiga:** quiebres para consumidores actuales.
- **Patron util que preserva:** evolucion controlada del producto.

### Regla F - Convenciones de documentacion y naming

- **Que exige:** estandarizar nombres de archivos, secciones y terminos tecnicos en espanol o ingles consistente.
- **Riesgo que mitiga:** confusion de onboarding y baja encontrabilidad.
- **Patron util que preserva:** documentacion operable por todo el equipo.

## 4) Checklist de salida de la fase

- [x] Se identificaron al menos 5 buenas practicas.
- [x] Se identificaron al menos 5 malas practicas o riesgos.
- [x] Hallazgos agrupados por categoria.
- [x] Se definio un set de reglas para mitigar riesgos y preservar patrones utiles.
- [ ] Commit dedicado de esta fase (pendiente de ejecutar comando Git).
