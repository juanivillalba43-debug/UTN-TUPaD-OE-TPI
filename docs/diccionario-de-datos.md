# Diccionario de datos

Definido en la Entrega N°3. Tipos de dato: **Integer** (identificadores y cantidades enteras), **Decimal** (kilos o litros), **String** (textos y códigos), **Boolean** (sí o no), **Date** y **DateTime** (fecha, con o sin hora). **PK** = clave primaria · **FK** = clave foránea.

## Insumo

| Campo | Tipo | Clave | Descripción |
|---|---|---|---|
| id_insumo | Integer | PK | Identificador único del insumo dentro del sistema. |
| codigo | String | | Código interno con el que la planta identifica el insumo. |
| nombre | String | | Nombre del insumo (por ejemplo, harina de trigo o bolsa de polipropileno). |
| tipo | String | | Clasificación: materia prima o material de empaque. |
| unidad_medida | String | | Unidad en la que se controla el stock: kg, litros o unidades. |
| stock_actual | Decimal | | Cantidad disponible en este momento. Se actualiza con cada movimiento. |
| stock_minimo | Decimal | | Nivel por debajo del cual el sistema genera una alerta de reposición. |
| activo | Boolean | | Indica si el insumo se sigue usando o fue dado de baja. |

## OrdenProduccion

| Campo | Tipo | Clave | Descripción |
|---|---|---|---|
| id_orden | Integer | PK | Identificador único de la orden de producción. |
| numero_orden | String | | Número visible de la orden, el que usan Producción y Logística. |
| producto | String | | Producto terminado que se va a fabricar. |
| cantidad_planificada | Integer | | Unidades del producto que se planifica producir. |
| fecha_planificada | Date | | Día en que la orden debe ejecutarse. |
| estado | String | | PLANIFICADA, EN_PRODUCCION, EN_CONTROL_CALIDAD, TERMINADA o RECHAZADA. |
| fecha_hora_cierre | DateTime | | Momento en que la orden se terminó o se rechazó. |

## MovimientoStock

| Campo | Tipo | Clave | Descripción |
|---|---|---|---|
| id_movimiento | Integer | PK | Identificador único del movimiento. |
| id_insumo | Integer | FK | Insumo afectado (referencia a Insumo). |
| id_orden | Integer | FK | Orden que consumió el insumo (referencia a OrdenProduccion). Queda vacío en ingresos y ajustes. |
| tipo_movimiento | String | | INGRESO (compra recibida), CONSUMO (uso en producción) o AJUSTE (corrección por inventario físico). |
| cantidad | Decimal | | Cantidad que entra o sale, en la unidad de medida del insumo. |
| fecha_hora | DateTime | | Momento exacto del movimiento. |
| lote | String | | Lote del insumo, para la trazabilidad que exige la industria alimentaria. |
| usuario | String | | Persona que registró el movimiento. |

## AlertaReposicion

| Campo | Tipo | Clave | Descripción |
|---|---|---|---|
| id_alerta | Integer | PK | Identificador único de la alerta. |
| id_insumo | Integer | FK | Insumo que bajó del stock mínimo (referencia a Insumo). |
| fecha_hora_generacion | DateTime | | Momento en que el sistema generó la alerta. |
| stock_al_generar | Decimal | | Stock que tenía el insumo cuando se generó la alerta. |
| atendida | Boolean | | Indica si Compras ya gestionó la reposición. |
| fecha_hora_atencion | DateTime | | Momento en que se cerró la alerta. |

## Interacción en el tiempo

Agregado en la Entrega N°4, a partir de la devolución de la Entrega N°3:

1. Un **`MovimientoStock`** (consumo, ingreso o ajuste) es siempre el **primer evento**.
2. Ese movimiento **actualiza `Insumo.stock_actual`**.
3. Recién entonces el sistema **compara `stock_actual` con `stock_minimo`**: si queda por debajo y no hay otra alerta abierta, **crea una `AlertaReposicion`**.
4. Cuando un `MovimientoStock` de tipo **INGRESO** vuelve a dejar el stock por encima del mínimo, la alerta pasa a **`atendida` = verdadero**.

Por eso la meta 3 se mide como la diferencia entre `MovimientoStock.fecha_hora` y `AlertaReposicion.fecha_hora_generacion`.
