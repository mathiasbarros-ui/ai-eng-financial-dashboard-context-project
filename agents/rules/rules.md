# Regla de Api

## Alcance

Estas reglas se aplican a todos los archivos Python ubicados dentro de:

`backend/**/*.py`

## 1. Los endpoints deben definir su modelo de respuesta

### Regla

Todo nuevo endpoint de FastAPI debe declarar un `response_model`
utilizando un modelo de Pydantic.

### Razón

Esto permite validar la respuesta del servidor, mantener consistente
la documentación de la API y detectar cambios accidentales en el contrato.



## Comprender la arquitectura antes de desarrollar

## Alcance
Proyecto completo: frontend, backend y ejecución local:
"*/**/"

## Regla
Antes de hacer cambios, el trabajador tiene que entender el flujo end to end: UI en React, consumo de API en FastAPI y origen de datos mock.

## Razón
Evita cambios aislados que rompen integración entre capas.

## Cómo verificarla
Puede explicar cómo llega un dato desde la API hasta un componente visual.
Identifica qué parte vive en frontend y cuál en backend.
Reconoce dónde están los puntos de entrada: main.tsx y main.py.


## Respetar el contrato de datos entre backend y frontend

## Alcance
Implica a backend y frontend:
/backend frontend/**

## Regla
No se deben cambiar nombres de campos, tipos o formatos de fecha sin actualizar ambas capas y sus pruebas.

## Razón
El dashboard depende de estructura exacta para cálculos y gráficos; cualquier descuido rompe la interfaz silenciosamente.

## Cómo verificarla
Se comparan modelos de backend con tipos del frontend.
Se valida que create_date, amount, operation_type, category y business_type sigan consistentes.
Se revisan routes.py y financial-types.ts.


# Mantener determinismo en datos de desarrollo

## Alcance
en backend con los datos mocks:
/backend/**/*.py

## Regla
En desarrollo y pruebas, mantener semilla fija para la generación de movimientos, salvo que se justifique explícitamente un cambio.

## Razón
El determinismo permite reproducir bugs, validar resultados y estabilizar tests.

## Cómo verificarla
La generación de mock data usa semilla fija.
Ejecuciones repetidas producen resultados consistentes en endpoints.
Se valida en routes.py y test_routes.py.


# Priorizar lógica de negocio en utilidades, no en componentes

## Alcance
Frontend del dashboard:
/frontend/**/*.tsx

## Regla
Los cálculos de KPI, agregaciones mensuales y formateos deben estar en funciones utilitarias, no embebidos en componentes visuales.

## Razón
Mejora mantenibilidad, testabilidad y legibilidad de la interfaz.

# Cómo verificarla
Componentes muestran datos, no contienen lógica pesada.
Cálculos centrales están en utilidades y cubiertos por tests.
Se revisan financial-utils.ts, financial-utils.test.ts y App.tsx.


# No romper la estrategia de filtros de la API

## Alcance
Backend, especialmente endpoints de métricas y segmentos B2B/B2C:
/backend/**/*.B2B B2C

## Regla
Nuevos filtros deben seguir el patrón actual de parámetros opcionales y composición de filtros existente.

## Razón
Mantiene comportamiento predecible y simplifica el consumo desde frontend o clientes externos.

# Cómo verificarla
Los filtros nuevos se implementan como query params opcionales.
No se duplica lógica si ya existe una función de filtrado.
Se validan rutas y helpers en routes.py.


# Toda mejora funcional debe venir con pruebas

## Alcance
Frontend y backend:
/backend frontend/**/*.py *.tsx

## Regla
Si se agrega o cambia comportamiento, debe añadirse o actualizarse al menos una prueba relevante en la misma capa.

## Razón
Seguridad:Protege contra regresiones y documenta comportamiento esperado.

# Cómo verificarla
Cada cambio funcional incluye test nuevo o ajustado.
Tests pasan localmente en frontend y backend.
Se revisan financial-utils.test.ts y test_routes.py.


# Mantener ejecución local clara y consistente

## Alcance
Onboarding y entorno de desarrollo.
/backend frontend/

## Regla
El nuevo trabajador debe usar el flujo de arranque documentado y no asumir configuraciones implícitas del equipo.

## Razón
Reduce fricción en onboarding y evita errores por diferencias de entorno.

# Cómo verificarla
Puede levantar servicios siguiendo documentación.
Verifica disponibilidad de frontend, backend y documentación API.
Usa como referencia README.md y docker-compose.yml.

# Manejar configuración de API desde variables de entorno

## Alcance
Frontend y comunicación con backend.
/frontend backend/**

## Regla
No hardcodear orígenes de API en componentes; usar variable de entorno para apuntar a backend cuando sea necesario.

## Razón
Permite mover el frontend entre local, contenedor y otros entornos sin tocar código.

# Cómo verificarla
El origen de API se obtiene desde entorno o proxy.
No hay URLs fijas dispersas en componentes.
Se valida en App.tsx y README.md.


# Documentar endpoints nuevos en la convención existente

## Alcance
Backend y documentación funcional.
/backend/

## Regla
Cualquier endpoint nuevo debe mantener estilo de nombres, response_model y semántica de ruta coherente con la API actual.

## Razón
Facilita descubrimiento en la documentación automática y mantiene consistencia para consumidores.

# Cómo verificarla
Nuevas rutas siguen el prefijo y estilo actual.
Tienen modelos de respuesta definidos cuando aplica.
Se comprueba en routes.py y en la documentación servida por backend.


# Evitar cambios de estilo no relacionados en la misma tarea

## Alcance
Contribuciones de código en cualquier capa.

## Regla
No mezclar refactors masivos, cambios cosméticos o renombrados amplios con una tarea funcional puntual.

## Razón
Hace revisiones más simples, reduce riesgo y mejora trazabilidad de errores.

# Cómo verificarla
El diff de cada tarea refleja un objetivo único y claro.
No hay modificaciones colaterales sin justificación.
Se puede explicar el cambio en una frase funcional concreta.