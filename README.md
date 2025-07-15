# Examen Parcial - Análisis de Datos

Este repositorio contiene mi solución al examen parcial del curso de Análisis de Datos. A continuación se detallan las secciones y preguntas respondidas.

## Sección 0: Preguntas de temas vistos en clase (20pts)

1. **UFuncs**: Funciones vectorizadas para operaciones eficientes en grandes volúmenes de datos.
2. **Broadcasting**: Técnica para operar arrays de diferentes dimensiones (incluye ejemplo en R).
3. **Axioma de elegibilidad**: Capacidad de seleccionar subconjuntos de datos basados en condiciones.
4. **Granularidad vs Agregación**: Explicación de conceptos y ejemplo para reporte por país.

## Sección I: Preguntas teóricas (50pts)

Respondí las preguntas seleccionadas aleatoriamente (5, 6, 8, 9, 10):

5. SQL: Uso de `HAVING` para filtrar resultados de agregaciones.
6. SQL: Consulta para encontrar filas en A que no están en B (LEFT JOIN + WHERE NULL).
8. Factores en R: Comportamiento al agregar elementos no existentes (resulta en NA).
9. Vectores vs Listas: Diferencias en tipos de elementos que pueden contener.
10. Combinatoria: Cálculo de posibles exámenes (252 combinaciones de 5 preguntas de 10).

## Sección II: Preguntas prácticas (30pts)

### A. Cliente más rentable en múltiples países
- Identifiqué al cliente `a17a7558` como el más rentable entre los clientes multinacionales.
- Ventas totales: $19,817.70.
- Criterio: Mayor volumen de ventas entre clientes que operan en varios países.

### B. Territorios con pérdidas considerables
- Identifiqué 11 territorios con ventas en el percentil 10 más bajo.
- Criterio: Ventas totales por debajo del umbral de $847.95.
- Recomendación: Dejar de operar por baja rentabilidad (costos probablemente superan ingresos).

## Ejecución del código
El análisis completo se encuentra en el archivo Rmarkdown, que incluye:
- Carga y procesamiento de datos
- Agregaciones y filtros
- Visualización de resultados

Los chunks de código están documentados para explicar cada paso del análisis.

## Requisitos
- R 4.0 o superior
- Paquetes: dplyr
- Datos: parcial_anonimo.rds
