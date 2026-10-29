# Módulo de Inventario y Planificación de Producción · Grupo Bimbo (planta piloto)

**Trabajo Práctico Integrador (TPI) · Organización Empresarial**
Tecnicatura Universitaria en Programación a Distancia · Universidad Tecnológica Nacional

| | |
|---|---|
| **Integrante** | Juan Ignacio Villalba (Comisión 6). Trabajo individual. |
| **Docente titular** | Prof. Gabriela Martínez |
| **Docente tutor** | Raúl Alejandro Lencinas Berenguer |
| **Tablero de gestión** | [GitHub Projects: backlog del TPI](ENLACE_TABLERO) |
| **Documento final** | [TPI consolidado (Entregas 1 a 4)](docs/entregas/TPI_OE_Juan.Ignacio.Villalba_Final.pdf) |

---

## Descripción del sistema

Grupo Bimbo es una empresa multinacional de panificación y snacks. En la Entrega N°1 la analicé como un **sistema abierto** y detecté que el control del inventario conecta muchas áreas: Compras, Producción, Almacenes, Calidad, Logística, Ventas y Finanzas.

El proyecto propone un **módulo de software de inventario y planificación de producción**, acotado a **una planta piloto**, que:

- registra las existencias de materias primas y envases y cada movimiento de stock (consumo, ingreso o ajuste) **con su número de lote**;
- relaciona el consumo de insumos con las **órdenes de producción** y su estado;
- **genera alertas de reposición** cuando un insumo queda por debajo del stock mínimo;
- permite medir el desempeño del proceso con **indicadores de control**.

> El trabajo es de **planificación y diseño**: no incluye el desarrollo del software. Los roles del equipo técnico son **hipotéticos**, porque el trabajo es individual.

## Recorrido del TPI

| Entrega | Contenido | Documento |
|---|---|---|
| N°1 · La organización y su enfoque sistémico | Misión, visión y sector; modelado como sistema abierto (TGS); módulo propuesto | [PDF](docs/entregas/Entrega01_La-organizacion-y-su-enfoque-sistemico.pdf) · [diagrama](docs/diagramas/sistema-abierto.png) |
| N°2 · Diagnóstico y planeación | FODA del proyecto, objetivo y metas cuantitativas, diagrama de Gantt con ruta crítica | [PDF](docs/entregas/Entrega02_Diagnostico-y-planeacion.pdf) · [FODA](docs/diagramas/matriz-foda.png) · [Gantt](docs/diagramas/diagrama-gantt.png) |
| N°3 · Estructura formal y diseño de datos | Organigrama piramidal vertical del área de sistemas; diccionario de datos | [PDF](docs/entregas/Entrega03_Estructura-formal-y-diseno-de-datos.pdf) · [organigrama](docs/diagramas/organigrama.png) · [diccionario](docs/diccionario-de-datos.md) |
| N°4 · Procesos, trabajo colaborativo y control | Proceso BPMN, repositorio y tablero ágil, historias de usuario, indicadores de control | [BPMN](docs/proceso-bpmn.md) · [historias de usuario](docs/historias-de-usuario.md) · [indicadores](docs/indicadores-de-control.md) |

## Estructura del repositorio

```
├── README.md                      ← portada del proyecto (este archivo)
└── docs/
    ├── entregas/                  ← PDF de cada entrega y documento final consolidado
    ├── diagramas/                 ← sistema abierto, FODA, Gantt, organigrama y BPMN (PNG)
    ├── planificacion/             ← lista de tareas del Gantt (CSV)
    ├── proceso-bpmn.md            ← proceso de negocio modelado
    ├── diccionario-de-datos.md    ← entidades, campos e interacción en el tiempo
    ├── historias-de-usuario.md    ← backlog: 3 historias con criterios de aceptación
    └── indicadores-de-control.md  ← estándares de desempeño para la auditoría técnica
```

## Forma de trabajo con Git

- `main` contiene solo trabajo revisado. Cada cambio se hace en una **rama secundaria** con prefijo (`docs/…`, `feature/…`) y se integra mediante un **Pull Request**.
- Los mensajes de commit describen el cambio en infinitivo (por ejemplo, *"Agregar diagrama BPMN del proceso de reposición"*).
- Los Pull Requests referencian los *issues* del tablero (`Closes #n`), así el avance del backlog queda vinculado a los cambios del repositorio.

---

## Declaración de Uso de IA

Para este trabajo usé **Claude (Anthropic)** como asistente, a lo largo de las cuatro entregas. La IA:

- redactó borradores del texto;
- propuso contenido (puntos de la FODA, metas, tareas del Gantt, estructura del organigrama, campos del diccionario, historias de usuario e indicadores);
- generó mediante código los diagramas (FODA, Gantt, organigrama y BPMN) y el armado del documento.

Las decisiones fueron mías. A continuación explico cómo estructuré los pedidos y qué corregí sobre lo que propuso.

### Prompts usados y por qué los formulé así

| Prompt (resumido, con mis palabras) | Por qué lo planteé así |
|---|---|
| *"Acá están la entrega 1, la devolución del profesor y la consigna de la entrega 2. Primero corrijamos los errores de la entrega 1 y después, paso por paso, hagamos la 2."* | Darle **la consigna, la rúbrica y la devolución** como contexto, para que no inventara requisitos, y trabajar **por pasos** para poder revisar cada parte antes de seguir. |
| *"Hagamos los roles hipotéticos y aclaremos que es así porque soy el único integrante."* | La consigna habla de un "equipo técnico" y trabajo solo: preferí declararlo en vez de forzar una situación irreal. |
| *"¿Por qué esas fechas?"* | No acepté la fecha de inicio propuesta sin entender de dónde salía. |
| *"No entiendo la opción 1. ¿Cuál me recomendás?"* | Pedir que me explicara las alternativas antes de elegir, para poder defender la decisión. |
| *"¿Podrías agregar en la parte que corresponda que apliqué lo que aprendo en el bootcamp de asistente ejecutivo? Analizá si aporta a la consigna."* | Sumar conocimiento propio (WBS, mapeo del negocio, riesgo de persona clave) y pedir que se evaluara si correspondía, no agregarlo porque sí. |
| *"Continuemos sobre el mismo trabajo, agregando lo que pide cada consigna."* | Mantener la coherencia entre entregas: mismo caso, mismos roles, mismas metas. |
| *"Adjunto la consigna de la entrega 4, la devolución de la 3 y el formato de entrega."* | Que las recomendaciones del profesor (lanes según el organigrama, criterios "Dado que / Cuando / Entonces", interacción temporal de las entidades) se incorporaran de forma explícita. |

### Correcciones humanas sobre los resultados de la IA

1. **Fechas del proyecto.** La IA propuso empezar el 02/11/2026. Al preguntar por qué, quedó en evidencia que el proyecto caía en las fiestas y en las vacaciones de enero, cuando una planta trabaja con menos personal. **Lo cambié al 01/03/2027.**
2. **Feriados.** Con las fechas nuevas, el primer hito quedaba en Viernes Santo. **Decidí fechar cada hito el último día hábil de su semana y aclararlo en el texto**, y acepté, con una justificación, que la especificación de requerimientos tuviera solo 2 días hábiles.
3. **Alcance de la FODA.** La primera versión tenía 4 o 5 puntos por cuadrante. **La reduje a 3**, elegidos para cubrir recursos, experiencia e infraestructura, como pide la consigna.
4. **Datos sin respaldo.** Las metas cuantitativas (2 segundos, 500.000 movimientos, 5 minutos) **no son datos de Bimbo**: son estimaciones, y así lo aclaré en el documento para no presentar como reales cifras que no lo son.
5. **Organigrama.** Elegí representar el **área de sistemas** y no toda la empresa, porque no conozco la estructura interna real de Grupo Bimbo y no quería inventarla.
6. **Devoluciones del profesor.** Incorporé las observaciones de cada entrega:
   - en la E1, el diagrama de bloques rehecho con frontera, flujos y *feedback*;
   - en la E3, la secuencia `MovimientoStock → stock_actual → AlertaReposicion`;
   - para la E4, las recomendaciones sobre los lanes y las historias de usuario.
7. **Decisión de modelado en el BPMN.** Los roles de desarrollo **no** son lanes del proceso, porque no participan de la operación de la planta. Usé las áreas del comité de usuarios clave y el sistema.

Revisé cada parte y elegí entre las alternativas propuestas. Cualquier error que haya quedado es responsabilidad mía.
