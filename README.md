Este workspace aloja tres Agent Skills para el Trabajo Práctico: generación y validación de escenarios SEI, y construcción del árbol de utilidad; cada una está aislada mediante globs de Cursor.

## Propósito del proyecto

El proyecto aplica la teoría de **Atributos de Calidad del SEI** —escenarios de 6 partes y Árboles de Utilidad— sobre requerimientos reales de arquitectura, usando **exclusivamente la bibliografía local de `docs/referencia/` como fuente de verdad**. Las skills no aportan conocimiento externo: todo lo que afirman debe estar respaldado por un archivo de la bibliografía.

Las tres skills se orquestan para:

1. Afinar escenarios de atributos de calidad a partir de borradores o texto libre.
2. Forzar al usuario a aportar el contexto que falta, con preguntas concretas.
3. Hacer sugerencias fundamentadas en la bibliografía (tácticas, tipos de medida).
4. Una vez obtenida toda la información, generar el árbol de utilidad.

El flujo es **híbrido**: borrador inicial → priorización → escenario completo → árbol consolidado. La especificación fina no arranca hasta que el atributo está clasificado, y el árbol no se arma hasta que hay escenarios validados.

Una **capa de presentación** basada en plantillas (`docs/templates/`) aísla la lógica de validación del formato de salida. Cada skill decide *qué* decir; la plantilla fija *cómo* se ve. Así el pipeline entre skills es determinístico y sin fricciones: lo que una emite es exactamente lo que la siguiente espera recibir.

**Fuera de alcance:** QAW (Quality Attribute Workshop) y la captura formal de restricciones no forman parte de este flujo, aunque la bibliografía los documente.

---

## Mapa del workspace

### Zonas y permisos

| Zona | Rol | Permiso |
|------|-----|---------|
| `docs/referencia/` | Fuente de verdad (bibliografía de la cátedra) | **Solo lectura** |
| `docs/templates/` | Contrato de formato: plantillas de salida de las skills | **Solo lectura** |
| `dataset_test/` | Fixtures de prueba para verificar el comportamiento de las skills | **Solo lectura** |
| `docs/arquitectura/` | Artefactos de trabajo del TP | **Única zona de escritura** |
| `.cursor/rules/` | Definición de las tres skills | Lectura (se edita solo al implementar prompts) |

### Estructura de archivos

```
.cursor/rules/
  1-generador-sei.mdc          Skill i — Generador de escenarios SEI (implementada)
  2-validador-sei.mdc          Skill ii — Validador de escenarios (implementada)
  3-arbol-utilidad.mdc         Skill iii — Constructor del árbol (stub)

docs/referencia/               BIBLIOGRAFÍA — solo lectura
  teoria_sei.md                Reglas estructurales de las 6 partes y criterios de validez de la medida
  Atributos de Calidad 3c62d4a554c581f5875de3690ef35a05.md
                               Índice maestro: qué son los atributos, tradeoffs y mapa de subpáginas
  Árbol de Utilidad 3c62d4a554c5817cbb44ee62602cb06a.md
                               Los 4 niveles del árbol y la semántica de las etiquetas (H, M, L)
  atributos/                   Fichas por atributo (tácticas y consideraciones de métricas)
    Clasificaciones de Atributos de Calidad 3c62d4a554c5817e999cef3bb762d6ad.md
                               Taxonomías ISO 9126 / MITRE / IEEE 1061
    QAW – Quality Attribute Workshop 3c62d4a554c5816bb895fe558601795f.md
    🌍 De la Facultad a la Realidad 3c62d4a554c5815bb5d3f0902eaf474c.md
    🟢 Disponibilidad, ⚡ Rendimiento, 🔒 Seguridad, ⛑️ Safety, 🧩 Modificabilidad,
    🔌 Integrabilidad, 🚀 Desplegabilidad, 🧪 Testeabilidad, 👥 Usabilidad,
    🔋 Eficiencia Energética, 🌐 Otros Atributos de Calidad (Cap. 14 SAP)
    Escenarios de Calidad el Template SEI de 6 Partes/
      Ejercicios de la Filmina (Resueltos) 3c62d4a554c5815cbe08d01280ee895b.md
      Escenario de Escalabilidad 3c62d4a554c581c89dbfe96a32da6722.md

docs/templates/                CONTRATO DE FORMATO — solo lectura
  template_escenarios.md       Único formato válido de un escenario; encabezado [ESTADO: ...]
  template_sugerencias.md      Sugerencias y auditoría, con 3 opciones de resolución
  template_preguntas.md        Preguntas de cierre por contexto que solo el usuario posee
  template_arbol.md            Formato del consolidado final (Skill iii)

docs/arquitectura/             ZONA DE ESCRITURA
  1-borradores/                Entrada: .txt del usuario (nunca se borran) y .md en elaboración
    req_ejemplo.txt            Placeholder vacío
  2-escenarios/                Escenarios sellados: un único .md por escenario validado
    escenario_01.md            Placeholder vacío
  3-arbol-utilidad/            Árbol de utilidad consolidado
    arbol_base.md              Placeholder vacío

dataset_test/                  FIXTURES — solo lectura
  escenarios_completos.txt     Casos 6/6 válidos del libro (Disponibilidad, Seguridad, Testability, Interoperabilidad)
  escenarios_incompletos.txt   Casos defectuosos con la acción esperada de la skill (falta Entorno, medida no cuantificable)
```

### Referencias que las skills consultan

| Rol | Ruta |
|-----|------|
| Reglas de las 6 partes y validez de la medida | `docs/referencia/teoria_sei.md` |
| Índice maestro (clasificación del atributo) | `docs/referencia/Atributos de Calidad 3c62d4a554c581f5875de3690ef35a05.md` |
| Estructura del árbol y etiquetas (H, M, L) | `docs/referencia/Árbol de Utilidad 3c62d4a554c5817cbb44ee62602cb06a.md` |
| Taxonomías para el nivel 2 del árbol | `docs/referencia/atributos/Clasificaciones de Atributos de Calidad 3c62d4a554c5817e999cef3bb762d6ad.md` |
| Tácticas y métricas por atributo | `docs/referencia/atributos/` |
| Ejercicios resueltos y medidas absolutas/relativas | `docs/referencia/atributos/Escenarios de Calidad el Template SEI de 6 Partes/` |
| Plantillas de salida (contrato de formato) | `docs/templates/` |

---

## Reglas de juego

Invariantes que aplican a las tres skills:

- **`docs/referencia/`, `docs/templates/` y `dataset_test/` nunca se modifican.** Son fuente de verdad, contrato de formato y fixtures. Todo output va a `docs/arquitectura/`.
- **Recuperación de contexto obligatoria antes de procesar cualquier requerimiento**, en este orden:
  1. `docs/referencia/teoria_sei.md` → reglas estructurales de las 6 partes.
  2. El índice maestro `Atributos de Calidad ...md` → clasificar el atributo de calidad detectado.
  3. **Drill-down obligatorio** al archivo correspondiente en `docs/referencia/atributos/` → extraer tácticas y consideraciones de métricas de *ese* atributo.
  4. La plantilla de `docs/templates/` que corresponda a la salida que se va a emitir.
  5. Recién entonces leer o escribir el artefacto en `docs/arquitectura/`.
- **La lógica vive en las reglas; el formato, en las plantillas.** Las skills no improvisan estructura de salida: rellenan una plantilla de `docs/templates/` sin agregar, quitar ni renombrar filas o encabezados. Esto hace que el handoff entre skills sea verificable por forma.
- **Las skills nunca inventan métricas.** Si falta la Medida de Respuesta, se marca y se sugiere; no se rellena con un número plausible.
- **Prioridad y Dificultad (H/M/L) las asigna exclusivamente el stakeholder/usuario.** Las skills estructuran y preguntan, no deciden.
- **El rótulo ASR se reserva para las hojas que pasan el criterio de `teoria_sei.md` §2**: impacto profundo en la arquitectura **y** Prioridad de Negocio **H o M**. No todo nodo hoja del árbol es un ASR: una hoja `(L, L)` es legítima y no es ASR. La condición de impacto arquitectónico es un **juicio del arquitecto**; la skill solo reúne y presenta la evidencia (componentes/artefactos afectados, tácticas que exige la ficha del atributo y tradeoffs con otros atributos), nunca dictamina.
- **El veredicto de ASR se emite en la priorización (paso 4), no durante la generación del borrador**, porque depende de una etiqueta que solo el stakeholder puede poner. Sin esa etiqueta la skill **omite el bloque de ASR por completo**: no hay tercer estado ni marca de indeterminación.
- **El input puede ser `.txt` o `.md`**, cargado en `docs/arquitectura/1-borradores/` o dictado por chat. No hay estructura obligatoria: el texto crudo alcanza.
- **Un escenario, un archivo.** Cada escenario vive en un solo `.md`, en `1-borradores/` mientras se elabora o en `2-escenarios/` una vez validado, nunca en ambos. Los `.txt` que cargás como input original quedan intactos y son el registro del avance entre iteraciones.
- **Solo escenarios validados entran al árbol de utilidad.**
- Las tres reglas tienen `alwaysApply: false` y globs por carpeta: se activan al trabajar sobre archivos de la ruta correspondiente, no por invocación explícita.

---

## Las tres skills

### Skill i — Generador de Escenarios SEI

| | |
|---|---|
| **Archivo** | `.cursor/rules/1-generador-sei.mdc` |
| **Glob** | `docs/arquitectura/1-borradores/*` |
| **Estado** | **Implementada** |
| **Input** | Requerimientos en texto libre o borradores incompletos, como `.txt` o `.md` en `docs/arquitectura/1-borradores/`, o dictados por chat |
| **Output** | Dos bloques: escenario con `[ESTADO: BORRADOR]` y preguntas de cierre con las medidas sugeridas |
| **Responsabilidad del usuario** | Responder las preguntas: confirmar las inferencias y aportar las métricas |

Transforma el borrador en un **Escenario de Calidad bajo el Template SEI de 6 partes** (Fuente → Estímulo → Artefacto → Ambiente → Respuesta → Medida de Respuesta), ejecutando primero el flujo de recuperación de contexto descrito arriba.

Su salida se compone **siempre de dos bloques**: el escenario (`template_escenarios.md`, encabezado con `[ESTADO: BORRADOR]`) y las preguntas de cierre (`template_preguntas.md`). Si hay marcas, hay preguntas: nunca entrega un borrador sin ellas. Las medidas sugeridas viajan dentro de la pregunta que corresponde a la marca `[INCOMPLETO]`.

**Le habla al usuario, no a sí misma.** No nombra archivos de la bibliografía ni menciona la lógica interna o las etapas del pipeline: la bibliografía es su insumo, no su tema.

Su política de entrega e inferencia:

- **Siempre entrega el escenario.** Ante vacíos críticos no detiene la redacción ni devuelve solo preguntas: entrega el escenario con las partes problemáticas marcadas de forma explícita y visible.
- **Un escenario por atributo.** Si el requerimiento mezcla dos atributos de calidad, genera un archivo por cada uno en lugar de combinarlos.
- **Puede inferir Ambiente y Artefacto** cuando son evidentes desde el borrador, marcándolos explícitamente como `[INFERIDO]`.
- **Nunca infiere la Medida de Respuesta.** Si falta o no es cuantificable, marca la parte como `[INCOMPLETO]` y en su pregunta propone las **3 medidas más pertinentes** al caso, con unidad y estadístico, dejando el umbral en tus manos.
- **Nunca inventa valores numéricos ni asigna Prioridad o Dificultad (H/M/L)**, y **no emite veredicto de ASR ni la evidencia para sustentarlo**: tácticas aplicables, tradeoffs entre atributos e impacto arquitectónico no van en el borrador, porque pertenecen a la etapa de priorización.
- **Todo output cierra con preguntas numeradas y accionables**, una por cada marca, que obligan al usuario a confirmar o resolver las inferencias y ambigüedades detectadas (*"1. ¿Cuál es la carga pico esperada de usuarios?"*, *"2. ¿Qué componente exacto es el artefacto estimulado?"*).

Mientras el escenario contenga `[INFERIDO]` o `[INCOMPLETO]` **no es un escenario definitivo**: queda como borrador bloqueado en `1-borradores/`.

### Skill ii — Validador de Escenarios SEI

| | |
|---|---|
| **Archivo** | `.cursor/rules/2-validador-sei.mdc` |
| **Glob** | `docs/arquitectura/2-escenarios/*.md` |
| **Estado** | **Implementada** |
| **Input** | Escenarios promovidos, con las inferencias ya confirmadas por el usuario |
| **Output** | Reporte de auditoría con 3 opciones de resolución, o escenario sellado con `[ESTADO: VALIDADO]` como archivo único |
| **Responsabilidad del usuario** | Elegir una opción de resolución, proponer otra o aportar el dato faltante |

Es un **auditor con poder de veto**, no un redactor. Un escenario solo sale de esta skill de dos formas: rechazado con opciones concretas de corrección, o renderizado como definitivo. No hay estado intermedio.

**Paso 1 — Auditoría.** Evalúa las 6 partes con condiciones de falla explícitas: fuente no identificable, estímulo que describe la respuesta en vez del disparo, artefacto con granularidad insuficiente cuando el escenario permite precisarlo, ambiente sin modo operacional concreto, respuesta que se confunde con la métrica, y medida que incumple los criterios de validez de `teoria_sei.md`. Suma tres chequeos transversales: coherencia entre el atributo declarado y las partes, un solo atributo por escenario, y falla automática si el archivo todavía arrastra marcas `[INFERIDO]` o `[INCOMPLETO]` —esas marcas significan que nunca debió promoverse—. **Basta una falla para rechazar.**

**Paso 2 — Freno.** Ante cualquier falla tiene prohibido redactar, insinuar o adelantar el escenario final. Renderiza `docs/templates/template_sugerencias.md` con exactamente **3 opciones concretas**, cada una trazable a la ficha del atributo o a los criterios de la teoría; una opción no puede ser "definir la métrica", tiene que proponer una métrica específica con su forma. Si ninguna falla admite opciones porque falta información que solo el usuario posee, usa `docs/templates/template_preguntas.md` en su lugar. Un valor propuesto como **Straw Man** se rotula siempre como estimación provisional sujeta a confirmación, nunca como dato validado.

**Paso 3 — Sello.** Solo tras la resolución del usuario emite el escenario con `docs/templates/template_escenarios.md`, encabezado por `[ESTADO: VALIDADO]` y con el campo *Nota de Auditoría* completo. Antes de emitirlo vuelve a correr la auditoría: si la resolución abrió una falla nueva, regresa al paso 2.

**Paso 4 — Consolidación.** Mantiene el invariante de **archivo único**: un escenario sellado existe exactamente una vez en `2-escenarios/` y ninguna en `1-borradores/`, sin versiones ni sufijos. Si el veredicto es rechazo, devuelve el archivo a `1-borradores/` con `[ESTADO: BORRADOR]` y lo quita de `2-escenarios/`, de modo que el ciclo de corrección vuelve a manos de la Skill i. La consolidación **solo toca archivos `.md`**: los `.txt` que cargaste como input original nunca se borran, mueven ni editan, porque son el registro de tu avance a lo largo de las iteraciones. Ante cualquier ambigüedad sobre qué copia eliminar, no borra nada y pregunta.

**Le habla al usuario, no a sí misma.** Igual que la Skill i, explica cada falla en términos de tu sistema y de lo que falta decidir, sin citar archivos de la bibliografía ni etapas del pipeline.

Los casos de `dataset_test/escenarios_incompletos.txt` describen el comportamiento esperado del rechazo.

### Skill iii — Constructor de Árbol de Utilidad

| | |
|---|---|
| **Archivo** | `.cursor/rules/3-arbol-utilidad.mdc` |
| **Glob** | `docs/arquitectura/3-arbol-utilidad/*.md` |
| **Estado** | **Pendiente de implementar** (stub: solo front-matter, sin prompt) |
| **Input previsto** | Escenarios validados de `docs/arquitectura/2-escenarios/` |
| **Output previsto** | Árbol de utilidad en `docs/arquitectura/3-arbol-utilidad/`, renderizado con `docs/templates/template_arbol.md` |
| **Responsabilidad del usuario** | Asignar la etiqueta `(Prioridad, Dificultad)` de cada hoja |

Contrato previsto:

- **Taxonomía declarada explícitamente.** Se elige el estándar más adecuado a cada caso —ISO 9126, MITRE o IEEE 1061, según lo documentado en la ficha de *Clasificaciones de Atributos de Calidad*— y el árbol declara cuál se usó.
- **Formato tabular según `docs/templates/template_arbol.md`**, que es el contrato que gobierna la salida: una tabla de cinco columnas —Atributo de Calidad, Refinamiento, Escenario, Prioridad (Negocio) y Dificultad (Técnica)—. Los cuatro niveles del árbol siguen presentes, aplanados en columnas: *Utility* es la raíz implícita en el título del documento, y las columnas de prioridad y dificultad son la etiqueta de la hoja.
- **El rótulo ASR no se aplica a toda fila.** El encabezado de la plantilla dice *Escenario (ASR)*, pero según `teoria_sei.md` §2 solo son ASR las hojas que pasan ambas condiciones; una fila `(L, L)` es un escenario legítimo del árbol y no es un ASR.
- Solo consume escenarios validados: nada con `[INFERIDO]` o `[INCOMPLETO]` llega al árbol.

---

## Workflow paso a paso

| Paso | Dónde trabajás | Skill | Input | Output | Lo que se te va a pedir |
|------|----------------|-------|-------|--------|-------------------------|
| 0 (opcional) | `dataset_test/` | — | Fixtures de escenarios completos e incompletos | Verificación offline del comportamiento esperado | Nada; es material de contraste |
| 1 | `docs/arquitectura/1-borradores/` o el chat | i — Generador | Requerimiento en texto libre (`.txt` o `.md`) o escenario incompleto | Escenario de 6 partes con `[INFERIDO]`/`[INCOMPLETO]` + preguntas | Confirmar inferencias y aportar métricas cuantificables |
| 2 | `docs/arquitectura/1-borradores/` | i — Generador | Tus respuestas | Escenario 6/6 sin marcas, listo para promover | Revisar y aprobar la promoción |
| 3 | `docs/arquitectura/2-escenarios/` | ii — Validador | Escenario promovido | Auditoría con 3 opciones, o escenario sellado como archivo único | Elegir una opción de resolución o aportar el dato |
| 4 | `docs/arquitectura/2-escenarios/` | — | Escenarios validados | Escenarios etiquetados y veredicto de ASR | Asignar `(Prioridad, Dificultad)` en H/M/L a cada escenario |
| 5 | `docs/arquitectura/3-arbol-utilidad/` | iii — Árbol *(pendiente)* | Escenarios validados y etiquetados | Tabla consolidada con taxonomía declarada | Validar la agrupación por atributo y refinamiento |

### Cómo cargar tu requerimiento

**Formatos aceptados:** `.txt` o `.md`, indistintamente. No hay estructura obligatoria: podés escribir una frase suelta de un stakeholder, un párrafo desordenado o un escenario a medio armar.

**Dos vías de entrada, equivalentes:**

- **Dejar el archivo** en `docs/arquitectura/1-borradores/`. Al trabajar en esa carpeta se activa la Skill i.
- **Describirlo por chat**, sin crear ningún archivo. La skill aplica el mismo contrato y deja el `.md` resultante en `docs/arquitectura/1-borradores/`.

**Qué pasa con lo que cargaste.** Si tu entrada fue un `.txt`, queda intacto para siempre: es el registro de tu requerimiento original y del avance entre iteraciones. Si fue un `.md`, ese mismo archivo es el que la skill va reescribiendo hasta que se valide. Una vez validado, el escenario existe **únicamente** en `docs/arquitectura/2-escenarios/`.

### Detalle de cada etapa

**Paso 1 — Cargá el requerimiento.** Por cualquiera de las dos vías, tan crudo como lo tengas. La Skill i lee la teoría, clasifica el atributo, baja a su ficha específica y devuelve el escenario estructurado. No esperes un resultado final en la primera pasada: lo normal es recibir un escenario con partes marcadas y una lista de preguntas al pie.

**Paso 2 — Cerrá el ciclo de confirmación.** Respondé las preguntas. Cada inferencia que confirmás elimina una marca `[INFERIDO]`; cada métrica que aportás elimina un `[INCOMPLETO]`. Si no sabés qué métrica usar, las sugerencias de la skill vienen de la ficha bibliográfica del atributo, así que sirven como punto de partida para discutir con el stakeholder.

**Paso 3 — Promoción y auditoría.** Cuando el escenario tiene **las 6 partes válidas y una medida cuantificable confirmada**, pasa a `docs/arquitectura/2-escenarios/`. Ese movimiento es la señal de que dejó de ser borrador, y activa la Skill ii. No des por hecho que la promoción implica aprobación: la auditoría es independiente y puede rechazarlo por fallas que el generador no detecta, como confundir Respuesta con Medida o mezclar dos atributos en un mismo escenario. Si te devuelve el reporte de sugerencias, elegí una de las tres opciones o proponé la tuya; el escenario no queda sellado hasta que la auditoría pase limpia.

Al sellarse, el `.md` queda consolidado en un **único archivo** en `2-escenarios/` y desaparece su copia bloqueada de `1-borradores/`. Si en cambio se rechaza, el `.md` vuelve a `1-borradores/` para que la Skill i retome el ciclo de marcas y preguntas. Tu `.txt` original no se toca en ningún caso.

**Paso 4 — Priorización.** Asignás vos, como stakeholder, el par `(Prioridad de Negocio, Dificultad Técnica)` en escala H/M/L. La semántica de cada valor está en la ficha del Árbol de Utilidad: H de negocio es un *must-have* cuya omisión pone en riesgo el proyecto; H de dificultad es alerta activa para el arquitecto. Las skills no deciden estas etiquetas.

Recién acá —y no antes— se puede emitir el **veredicto de ASR**, porque su segunda condición *es* la Prioridad de Negocio que acabás de asignar: se cumple con **H o M**. La primera condición, el impacto profundo en la arquitectura, la decidís vos como arquitecto sobre la evidencia que aporta la skill (componentes/artefactos afectados, tácticas que exige la ficha del atributo y tradeoffs con otros atributos de calidad). Las hojas que no pasan ambas condiciones siguen siendo escenarios legítimos del árbol, simplemente no son ASR.

**Paso 5 — Consolidación del árbol.** Con los escenarios validados y etiquetados se construye el árbol en `docs/arquitectura/3-arbol-utilidad/`, declarando la taxonomía elegida y agrupando cada escenario bajo su atributo y refinamiento. La salida se renderiza con `docs/templates/template_arbol.md`, que aplana los cuatro niveles en una tabla de cinco columnas.

---

## Convenciones de marcado

| Marca | Significado | Quién la resuelve |
|-------|-------------|-------------------|
| `[INFERIDO]` | La skill dedujo esa parte (solo Ambiente o Artefacto) porque era evidente en el borrador, pero no fue afirmada por el usuario | El usuario confirma o corrige |
| `[INCOMPLETO]` | Falta información que la skill no puede inventar; típicamente la Medida de Respuesta. Va acompañado de sugerencias de medidas tomadas de la ficha del atributo | El usuario aporta el dato |
| `[ESTADO: BORRADOR]` | Encabeza todo escenario emitido por la Skill i. Señala que aún no pasó por la auditoría | Se reemplaza solo al aprobar la auditoría |
| `[ESTADO: VALIDADO]` | Sello que emite la Skill ii tras una auditoría limpia. Encabeza el escenario y es la única llave de entrada al árbol de utilidad | Nadie: lo emite la skill, y solo cuando no queda ninguna falla |

**Estado del escenario:** mientras contenga `[INFERIDO]` o `[INCOMPLETO]`, el escenario permanece como **borrador bloqueado**. No avanza de etapa y no entra al árbol de utilidad. Que esté bloqueado no impide su entrega: la skill igual devuelve el escenario completo con las marcas a la vista.

**No hay marca para el ASR.** No existe un tercer estado tipo `[INDETERMINADO]`: mientras el escenario no tenga la etiqueta de Prioridad de Negocio asignada por el stakeholder, la skill **omite el bloque de ASR por completo** —no lo emite vacío ni como "no determinable"—.

**Ciclo de confirmación que desbloquea la promoción:**

1. La skill entrega el escenario con sus marcas y cierra con preguntas numeradas, una por marca.
2. El usuario confirma las inferencias y aporta las métricas faltantes.
3. La skill reescribe el escenario sin marcas.
4. Con 6/6 partes válidas y medida cuantificable confirmada, el escenario se promueve a `docs/arquitectura/2-escenarios/`.

Si una respuesta abre una ambigüedad nueva, el ciclo se repite: el escenario vuelve a quedar bloqueado hasta que no queden marcas. El veredicto de ASR queda fuera de este ciclo: se emite en el paso 4 del workflow, cuando ya existe la etiqueta de Prioridad.

---

## Estado de implementación

- **Skill i (`1-generador-sei.mdc`)**: implementada, con flujo de RAG local sobre `docs/referencia/` y regla de entrega con clarificación interactiva (entrega siempre, marca, sugiere y pregunta).
- **Skill ii (`2-validador-sei.mdc`)**: implementada, con auditoría determinística de las 6 partes, freno mediante `template_sugerencias.md` y renderizado final mediante `template_escenarios.md`.
- **Skill iii (`3-arbol-utilidad.mdc`)**: **stub**. Contiene front-matter (`description`, `globs`, `alwaysApply`) y el título, con la nota *"El prompt determinístico se desarrollará en el siguiente paso"*. Hasta que se implemente, el paso 5 del workflow se ejecuta manualmente siguiendo el contrato descrito arriba.
- **Placeholders vacíos**: `docs/arquitectura/1-borradores/req_ejemplo.txt`, `docs/arquitectura/2-escenarios/escenario_01.md` y `docs/arquitectura/3-arbol-utilidad/arbol_base.md` existen sin contenido; marcan la ubicación de cada artefacto.
- **Formato del árbol resuelto**: gobierna `docs/templates/template_arbol.md`, es decir la tabla de cinco columnas. Los cuatro niveles quedan aplanados en columnas, con *Utility* como raíz implícita en el título.
- **Ausente en la bibliografía**: la ficha *Straw Man y Estimación de Medidas de Respuesta* está enlazada desde la página de QAW pero no existe en el repositorio, así que las skills no disponen del método detallado para estimar ese valor provisional.
