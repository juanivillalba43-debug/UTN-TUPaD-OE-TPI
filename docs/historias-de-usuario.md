# Historias de usuario

Backlog inicial del **módulo de inventario y planificación de producción** (planta piloto de Grupo Bimbo).
Las tres historias se escriben en el formato formal *"Como [usuario], quiero [funcionalidad], para [beneficio]"* y sus criterios de aceptación en formato *"Dado que / Cuando / Entonces"*. Usan los campos y estados definidos en el [diccionario de datos](diccionario-de-datos.md) y corresponden a actividades del [proceso BPMN](proceso-bpmn.md).

En el tablero de GitHub Projects cada historia es un *issue* con la etiqueta `historia de usuario`, y su estado (**Por hacer → En progreso → Listo**) refleja el avance de su especificación dentro del trabajo.

---

## HU-01 · Registrar el consumo de insumos con lote

> **Como** encargado de Almacenes, **quiero** registrar cada entrega de insumos a producción indicando la orden de producción y el lote, **para** que el stock se actualice en el momento y se pueda rastrear qué lote se usó en cada orden.

- **Prioridad:** alta
- **Actividad BPMN:** "Entregar insumos y registrar consumo con lote" → "Registrar MovimientoStock (CONSUMO)"
- **Datos que usa:** `MovimientoStock` (`tipo_movimiento`, `cantidad`, `lote`, `id_orden`, `fecha_hora`), `Insumo.stock_actual`

**Criterios de aceptación**
1. **Dado que** la orden de producción está en estado `EN_PRODUCCION` y el insumo "Harina de trigo" tiene `stock_actual` = 800 kg, **cuando** registro un consumo de 50 kg del lote `L-2027-031`, **entonces** se crea un `MovimientoStock` de tipo `CONSUMO` con ese lote y esa orden, y el `stock_actual` del insumo pasa a 750 kg.
2. **Dado que** estoy cargando un consumo, **cuando** intento guardarlo sin indicar el lote, **entonces** el sistema no lo registra y me pide completar el lote.
3. **Dado que** el insumo tiene `stock_actual` = 30 kg, **cuando** intento registrar un consumo de 50 kg, **entonces** el sistema rechaza el movimiento e informa el stock disponible.

---

## HU-02 · Recibir una alerta de reposición

> **Como** responsable de Compras, **quiero** recibir una alerta cuando un insumo quede por debajo de su stock mínimo, **para** emitir la orden de compra antes de que falte en producción.

- **Prioridad:** alta
- **Actividad BPMN:** compuerta "¿stock_actual por debajo del mínimo?" → "Generar AlertaReposicion" → "Revisar alerta y emitir orden de compra"
- **Datos que usa:** `Insumo.stock_actual`, `Insumo.stock_minimo`, `AlertaReposicion` (`fecha_hora_generacion`, `stock_al_generar`, `atendida`)
- **Meta relacionada:** meta 3 (alerta en menos de 5 minutos)

**Criterios de aceptación**
1. **Dado que** un insumo tiene `stock_minimo` = 200 kg y `stock_actual` = 210 kg, **cuando** se registra un consumo de 20 kg, **entonces** en menos de 5 minutos se crea una `AlertaReposicion` con `stock_al_generar` = 190 kg y `atendida` = falso.
2. **Dado que** ese insumo ya tiene una alerta sin atender, **cuando** se registra otro consumo que lo deja aún más bajo, **entonces** no se crea una alerta duplicada.
3. **Dado que** existe una alerta sin atender, **cuando** se registra el ingreso de la mercadería (`MovimientoStock` de tipo `INGRESO`) y el stock vuelve a superar el mínimo, **entonces** la alerta queda con `atendida` = verdadero y se completa `fecha_hora_atencion`.

---

## HU-03 · Seguir el estado de las órdenes de producción

> **Como** supervisor de Producción, **quiero** ver y actualizar el estado de cada orden de producción, **para** saber en todo momento qué órdenes están en curso, en control de calidad o terminadas.

- **Prioridad:** media
- **Actividad BPMN:** "Liberar orden de producción"
- **Datos que usa:** `OrdenProduccion` (`estado`, `fecha_planificada`, `fecha_hora_cierre`)
- **Concepto relacionado:** el campo `estado` funciona como una máquina de estados finitos (Unidad 5): la orden está en un solo estado por vez y solo admite transiciones válidas.

**Criterios de aceptación**
1. **Dado que** una orden está en estado `PLANIFICADA`, **cuando** la libero para producción, **entonces** su estado pasa a `EN_PRODUCCION`.
2. **Dado que** una orden está en `EN_CONTROL_CALIDAD`, **cuando** registro el resultado del control, **entonces** pasa a `TERMINADA` o `RECHAZADA` y se guarda `fecha_hora_cierre`.
3. **Dado que** una orden está `TERMINADA`, **cuando** intento volverla a `EN_PRODUCCION`, **entonces** el sistema rechaza el cambio porque no es una transición válida.

### Transiciones válidas de `OrdenProduccion.estado`

| Estado actual | Evento | Estado siguiente |
|---|---|---|
| PLANIFICADA | Liberar orden | EN_PRODUCCION |
| EN_PRODUCCION | Terminar elaboración | EN_CONTROL_CALIDAD |
| EN_CONTROL_CALIDAD | Control aprobado | TERMINADA |
| EN_CONTROL_CALIDAD | Control rechazado | RECHAZADA |
