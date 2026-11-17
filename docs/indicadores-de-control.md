# Indicadores de control y estándares de desempeño

Siguiendo las etapas del control del proceso administrativo (Unidad 4): **1) establecer estándares → 2) medir → 3) comparar → 4) corregir**, cada indicador tiene un estándar, una forma de medirlo con los datos del sistema, una frecuencia y un responsable del [organigrama](diagramas/organigrama.png). Los estándares técnicos retoman las metas cuantitativas de la Entrega N°2.

## A. Indicadores del sistema (auditoría técnica)

| # | Indicador | Cómo se mide | Estándar | Frecuencia | Responsable |
|---|---|---|---|---|---|
| T1 | Tiempo de respuesta | Percentil 95 del tiempo de las consultas de stock y del registro de movimientos (registro de la aplicación) | ≤ 2 s con 50 usuarios simultáneos (meta 1) | Semanal / en cada prueba de carga | Tester QA |
| T2 | Volumen soportado | Cantidad de `MovimientoStock` registrados por año sin que T1 supere su estándar | ≥ 500.000 por año (meta 2) | Trimestral | Desarrollador backend |
| T3 | Latencia de las alertas | `AlertaReposicion.fecha_hora_generacion` − `MovimientoStock.fecha_hora` del movimiento que dejó el stock bajo el mínimo | ≤ 5 min en el 100 % de los casos (meta 3) | Diaria | Desarrollador backend |
| T4 | Trazabilidad de lotes | % de movimientos de tipo `CONSUMO` e `INGRESO` con el campo `lote` completo | 100 % | Semanal | Analista funcional |
| T5 | Exactitud del inventario | \|stock físico contado − `stock_actual`\| ÷ stock físico, por insumo | ≤ 2 % por insumo | Mensual (conteo físico) | Analista funcional con Almacenes |

## B. Indicadores del proceso (impacto en la planta piloto)

| # | Indicador | Cómo se mide | Estándar | Frecuencia | Responsable |
|---|---|---|---|---|---|
| P1 | Alertas atendidas a tiempo | % de `AlertaReposicion` con `fecha_hora_atencion` − `fecha_hora_generacion` ≤ 24 h | ≥ 90 % | Semanal | Compras (usuario clave) |
| P2 | Lotes rechazados en la recepción | Recepciones con lote no conforme ÷ total de recepciones (compuerta "¿Lote conforme?" del BPMN) | ≤ 5 % | Mensual | Almacenes (usuario clave) |
| P3 | Órdenes demoradas por falta de insumos | Órdenes que no pasaron a `EN_PRODUCCION` en su `fecha_planificada` por falta de stock ÷ total de órdenes | 0 % como objetivo; alerta si supera el 2 % | Semanal | Líder de proyecto |

## C. Indicadores del proyecto (seguimiento del Gantt y del tablero)

| # | Indicador | Cómo se mide | Estándar | Frecuencia | Responsable |
|---|---|---|---|---|---|
| G1 | Cumplimiento de hitos | Hitos H1 a H4 cumplidos en su fecha ÷ hitos vencidos | 100 %; ningún hito con más de 1 semana de desvío | En cada hito | Líder de proyecto |
| G2 | Avance del backlog | Historias de usuario en "Listo" ÷ historias planificadas para el período | Según lo planificado en el Gantt | Semanal | Líder de proyecto |
| G3 | Calidad del versionado | % de cambios que llegan a `main` mediante un Pull Request revisado | 100 % | En cada integración | Líder de proyecto |

## Cómo se usan (comparación y corrección)

- Si un indicador **cumple** el estándar, se registra y se sigue midiendo.
- Si **no cumple**, se abre un *issue* en el repositorio con la etiqueta `desvío`, se analiza la causa (por ejemplo, T3 alto → revisar la consulta que compara `stock_actual` con `stock_minimo`) y se agrega la corrección al backlog.
- La retroalimentación cierra el ciclo: los desvíos repetidos vuelven a la **planificación** (nuevas historias de usuario o ajustes del Gantt), igual que el *feedback* del sistema abierto de la Entrega N°1.
