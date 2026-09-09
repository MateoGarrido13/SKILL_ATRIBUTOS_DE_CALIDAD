# Skills de Cursor para Atributos de Calidad (SEI)

**TP3 — Diseño de Software · Ejercicio 4 · Informe técnico de construcción y validación**

| | |
|---|---|
| **Tipo de skill** | Agent Skills nativas de Cursor (formato `.cursor/rules/*.mdc`, `alwaysApply: false`, activación por glob de carpeta) |
| **Skills implementadas** | Skill i — Generador SEI (`1-generador-sei.mdc`) · Skill ii — Validador SEI (`2-validador-sei.mdc`) · Skill iii — Constructor de Árbol de Utilidad (`3-arbol-utilidad.mdc`) |
| **Alcance** | Corresponde a los 3 incisos del ejercicio 4: (i) generar escenarios con el template de 6 partes del SEI, (ii) chequear completitud / auditar un escenario dado, (iii) elaborar el árbol de utilidad una vez que el stakeholder asignó Prioridad y Dificultad |

---

## 1. Introducción

Este flujo es un pipeline de **tres Agent Skills** encadenadas. No se invocan por nombre de producto: cada una se activa al trabajar en su carpeta de `docs/arquitectura/` (o al mencionarla en el chat). El orden es **especificar primero, priorizar después**: el árbol no se arma hasta que hay escenarios sellados `[ESTADO: VALIDADO]` **y** las dos etiquetas H/M/L de cada hoja.

| Skill | Nombre | Alcance |
|---|---|---|
| **i** | Generador de Escenarios SEI | Texto libre o borrador → escenario de 6 partes (`[ESTADO: BORRADOR]`). Marca `[INFERIDO]` / `[INCOMPLETO]` y pregunta; no sella. |
| **ii** | Validador de Escenarios SEI | Auditor con veto. Rechaza con exactamente 3 opciones o sella `[ESTADO: VALIDADO]`. No hay estado intermedio. |
| **iii** | Constructor de Árbol de Utilidad | Consolida solo escenarios validados. Sin H/M/L no escribe el árbol (compuerta cerrada). Con etiquetas: taxonomía, tabla de 5 columnas, cobertura/alertas y evidencia de ASR para que el arquitecto dictamine. |

El destinatario de las tres es el dueño del sistema. Las skills no nombran la bibliografía ni las etapas internas del pipeline en la respuesta al usuario.

---

## 2. Composición y construcción de la skill

El repositorio se organiza así: la **lógica** vive en `.cursor/rules/`; el **formato** es un contrato de solo lectura en `docs/templates/`; la **bibliografía** está en `docs/referencia/`; la **única zona de escritura** es `docs/arquitectura/` (`1-borradores/` → `2-escenarios/` → `3-arbol-utilidad/`). Un directorio separado `test/` contiene los casos de validación y no es insumo del árbol.

La construcción se hizo en capas, en este orden:

### Fuente de verdad

La única fuente de verdad es `docs/referencia/`: `teoria_sei.md` (6 partes, validez de la Medida de Respuesta, ASR, semántica H/M/L), el índice maestro de Atributos de Calidad (clasificación y tradeoffs), el material de Árbol de Utilidad, las clasificaciones (ISO 9126 / MITRE / IEEE 1061) y las fichas por atributo (tácticas y medidas). Las skills **no aportan conocimiento externo**: si la bibliografía no cubre un aspecto, se declara fuera de alcance.

Antes de redactar, cada skill lee esa bibliografía en un orden fijo (RAG local): teoría → índice o árbol → ficha del atributo detectado → plantilla a rellenar. Las plantillas se rellenan; no se editan.

### Reglas que vuelven la respuesta más determinística

Estas reglas recortan la libertad del modelo y fuerzan un veredicto repetible:

- **Un escenario, un atributo.** Si el requerimiento mezcla dos, se generan dos archivos; no se combinan.
- **Inferencia acotada.** Solo se pueden inferir Ambiente y Artefacto, y van marcados `[INFERIDO]`. Fuente, Estímulo y Respuesta no se inventan. La Medida ausente o no cuantificable se marca `[INCOMPLETO]`; se proponen 3 medidas (unidad y estadístico) y el umbral lo pone el usuario. **Nunca se inventan valores numéricos**; un *Straw Man* se rótula como estimación provisional.
- **Prohibición de adelantar etapas.** El Generador no escribe `[ESTADO: VALIDADO]`, no asigna H/M/L ni dictamina ASR, tácticas o tradeoffs. El Validador no muestra “cómo quedaría” el escenario final ante un rechazo. El Árbol no asigna H/M/L ni escribe “esto es ASR”.
- **Auditoría binaria (Skill ii).** Las 6 partes más los chequeos transversales (coherencia del atributo, un atributo por escenario, marcas heredadas) dan VALIDADO o rechazo. Basta una falla. El rechazo trae exactamente 3 opciones concretas (tipo de medida, unidad y estadístico). Tras el sello, el `.md` existe una sola vez en `2-escenarios/` y ninguna en `1-borradores/`.
- **Compuerta de etiquetado (Skill iii).** Si falta una sola celda H/M/L de una sola hoja, no se escribe el árbol: solo `template_pregunta_prioridad.md`, con H, M y L al mismo peso, ancladas a *ese* escenario. Las tres opciones no se recomiendan ni se dejan por default.
- **Contrato de formato.** Cada salida rellena una plantilla fija (`template_escenarios.md`, `template_preguntas.md`, `template_sugerencias.md`, `template_pregunta_prioridad.md`, `template_arbol.md`).
- **`.txt` intocables.** El requerimiento original en texto plano no se borra, mueve ni edita.

### Casos de test

Los tests están en `test/`. No alimentan el árbol; sirven para contrastar el comportamiento esperado (casos que deben llegar a 6/6 y casos que deben rechazarse) y para comparar corridas entre integrantes.

#### `test/PRUEBA_UNO_HOSPITAL/` — prueba inicial de las Skills i y ii

Un único enunciado informal del sistema de **gestión de turnos de un hospital** mezcla dos atributos (“tiene que ser seguro para que no entren intrusos” y “andar bien cuando hay mucha gente sacando turnos”). El objetivo es ver si el Generador **parte el requerimiento** en dos escenarios (Seguridad y Rendimiento), marca lo inferido/incompleto y pregunta, y si el Validador sella solo cuando las 6 partes son cuantificables y coherentes. Los artefactos de la carpeta (`req_uno.txt`, borradores y escenarios sellados de seguridad y rendimiento) registran ese recorte.

#### `test/PRUEBA_DOS_CHATBOT_PAGOS/` — prueba inicial de las Skills i y ii

Dos inputs a la vez, deliberadamente sucios: un `.md` en borradores **fuera de formato e incompleto** (nuevas funciones / pasarela de pago) y, por chat, un atributo **mal mapeado** (“Escalabilidad” para un chatbot 24 hs). El objetivo es medir **capacidad correctiva** y respeto de templates: ¿reclasifica el atributo?, ¿sigue las plantillas?, ¿el sesgo del primer relato contamina al segundo? En la corrida documentada, reconoció el mapeo erróneo del chatbot (hacia Disponibilidad) y, en el otro borrador, adoptó Modificabilidad en lugar de Integrabilidad.

#### `test/PRUEBA_TRES_FLUJO_COMPLETO_ECOMERCE/` — prueba integral (todo el proyecto)

Corrida común del pipeline **i → ii → iii** sobre **MarketHub** (e-commerce grupal): contexto + cinco requerimientos vagos, iteración con el stakeholder, auditoría con tres opciones, etiquetado H/M/L y árbol. Sirve como **benchmark compartido** para comparar clasificación, medidas y priorización entre implementaciones (p. ej. contra el informe de la otra rama del grupo). Esta carpeta guarda los escenarios refinados, una copia del árbol y este informe.

Registro de comportamiento observado en esa corrida (detalle de la conversación: [[cursor_use_case_scenario_audit]] / [cursor_use_case_scenario_audit.md](../../../cursor_use_case_scenario_audit.md)):

- Deja explícitos los campos inferidos y respeta los templates.
- Ofrece las opciones de resolución (el entorno lo permite).
- Separa en dos escenarios distintos la protección de la base de datos y el intento malintencionado de acceso al panel de administración: mismo atributo (Seguridad), artefactos y medidas distintos.
- En auditoría (Skill ii) respeta el template de las tres opciones y resuelve ambigüedades.

![[Pasted image 20260908214219.png]]

---

## 3. Problemas detectados y correcciones aplicadas

La validación se hizo corriendo el pipeline con pedidos naturales (texto libre, borradores incompletos, contexto de sistema) y comparando si la salida era comparable entre corridas y contra lo que pide el template SEI. Se detectaron y corrigieron dos problemas de fondo, anteriores a las reglas actuales de `.cursor/rules/`.

### Problema 1 — Formato inconsistente de las respuestas a lo largo del pipeline

En las primeras corridas la skill **respondía bien en contenido y mal en forma**. Un mismo tipo de pedido (estructurar un escenario, rechazar una auditoría, pedir H/M/L) producía tablas distintas, listas sin encabezado, prosa mezclada con la medida, o un árbol inventado en ASCII. Eso impedía comparar corridas, versionar artefactos y enseñarle al modelo *dónde* termina una etapa y empieza la siguiente: no había un contrato de salida, solo una instrucción vaga de “usar el template de 6 partes”.

> **Comportamiento esperado:** cada etapa del pipeline emite **siempre** el mismo esqueleto. El usuario reconoce de un vistazo si está ante un borrador, un rechazo con opciones, una pregunta de prioridad o el árbol sellado. Las columnas no cambian entre corridas.

**⚠️ Output antes de la corrección (patrón observado):**

- El escenario salía a veces como viñetas (`Fuente: …`, `Estímulo: …`), a veces como tabla de dos columnas, a veces incrustado en un párrafo. El estado (`BORRADOR` / `VALIDADO`) faltaba o se adelantaba.
- El rechazo de auditoría se explicaba en prosa (“habría que precisar la métrica”) **sin** tres opciones accionables, o se adelantaba “cómo quedaría” la tabla ya corregida.
- El árbol aparecía como un diagrama de texto o como una tabla de 3–4 columnas distintas a las de la teoría (sin refinamiento, o con ASR ya dictaminado).

**✅ Corrección aplicada:** se introdujo un **contrato de formato de solo lectura** en `docs/templates/`. Las reglas `.cursor/rules/1-generador-sei.mdc`, `2-validador-sei.mdc` y `3-arbol-utilidad.mdc` obligan a **rellenar** esas plantillas y **prohíben alterarlas**. Cada plantilla vive a una distancia distinta del uso: más cerca del requerimiento crudo al inicio, más cerca de la priorización al final.

| Distancia en el uso | Momento del pipeline | Plantilla | Qué obliga a emitir |
|---|---|---|---|
| **Cercana al input** — el usuario acaba de pegar un `.txt`/`.md` o un párrafo en el chat | Skill i, escenario todavía incompleto | [`docs/templates/template_escenarios.md`](../../templates/template_escenarios.md) | Tabla de 6 partes con `[ESTADO: BORRADOR]`, atributo declarado y Nota de Auditoría. Es el único formato legal de un escenario, también cuando más tarde se sella. |
| **Cercana al input** — faltan Ambiente, Artefacto o Medida | Skill i (y Skill ii si el dato solo lo tiene el dueño del sistema) | [`docs/templates/template_preguntas.md`](../../templates/template_preguntas.md) | Bloque “Faltan Datos…”, una pregunta numerada por marca `[INFERIDO]` / `[INCOMPLETO]`. Cierra con el pedido de responder para regenerar la tabla. |
| **Mitad del pipeline** — el escenario ya tiene 6 partes y se pide auditar | Skill ii, rechazo (caso por defecto) | [`docs/templates/template_sugerencias.md`](../../templates/template_sugerencias.md) | “Resultado de la Auditoría”, una entrada por parte fallida, **exactamente 3 opciones** en checkboxes. No hay tabla final ni `[ESTADO: VALIDADO]`. |
| **Mitad del pipeline** — el usuario eligió una opción y la re-auditoría pasa | Skill ii, sello | de nuevo [`template_escenarios.md`](../../templates/template_escenarios.md) | Mismo esqueleto, encabezado `[ESTADO: VALIDADO]` y Nota de Auditoría de *qué eligió el usuario*. |
| **Lejos del input, cerca del diseño** — hay escenarios sellados pero no hay H/M/L | Skill iii, compuerta cerrada | [`docs/templates/template_pregunta_prioridad.md`](../../templates/template_pregunta_prioridad.md) | Un bloque por escenario: qué implicaría H / M / L **en ese artefacto y esa medida**, sin recomendar una letra. |
| **Más lejos** — las dos letras de todas las hojas ya están dichas | Skill iii, compuerta abierta | [`docs/templates/template_arbol.md`](../../templates/template_arbol.md) | Título *Árbol de Utilidad de Arquitectura* y **exactamente** cinco columnas: Atributo · Refinamiento · Escenario (ASR) · Prioridad (Negocio) · Dificultad (Técnica). Taxonomía, cobertura y evidencia de ASR envuelven la tabla; no agregan columnas. |

**Ejemplo instanciado (MarketHub, Skill i).** Un requerimiento vago (“conectarse con las APIs… si una cambia algo no se rompa el núcleo”) ya no se responde con un párrafo libre. Sale primero `template_escenarios.md` (tabla SEI, `[ESTADO: BORRADOR]`, marcas `[INFERIDO]` / `[INCOMPLETO]`) y, pegado, `template_preguntas.md` pidiendo artefacto, ambiente y tres medidas candidatas. El detalle de esa corrida está en [[cursor_use_case_scenario_audit]].

**Ejemplo instanciado (MarketHub, Skill ii).** Al auditar Integrabilidad de pasarelas, el rechazo no fue “faltan detalles”. Fue `template_sugerencias.md`: fallas de Fuente y Estímulo (el `o` entre stakeholder y proveedor, y entre cobro y logística) y tres opciones concretas (Stripe vs Andreani vs DHL). El usuario elige una; recién ahí se rellena de nuevo `template_escenarios.md` con `[ESTADO: VALIDADO]`.

**Ejemplo instanciado (MarketHub, Skill iii).** Sin etiquetas, la skill **no** dibujó el árbol: emitió `template_pregunta_prioridad.md` escenario por escenario. Recién con las seis pares H/M/L rellenó `template_arbol.md` (archivo de trabajo `docs/arquitectura/3-arbol-utilidad/arbol_base.md`).

**✅ Output después de la corrección:** las corridas de `PRUEBA_UNO_HOSPITAL/`, `PRUEBA_DOS_CHATBOT_PAGOS/` y `PRUEBA_TRES_FLUJO_COMPLETO_ECOMERCE/` ya son comparables entre si y respetan todas un mismo formato.
---

### Problema 2 — Inferencia silenciosa sobre escenarios vagos, sin validar ni reconsultar

El segundo problema era que,ante un enunciado vago de atributo de calidad (“recontra seguros”, “sin problemas raros de visualización”, “bloquearlo rápido”), la skill **completaba sola** partes que el usuario no había afirmado: Ambiente (“operación normal”), Artefacto (“el sistema”), a veces la Medida (un número tomado de un ejemplo de la bibliografía) y hasta el atributo (Usabilidad vs Portabilidad vs Rendimiento). Esas premisas **no pasaban por una validación** ni se reconsultaban: entraban al `.md` como si el stakeholder las hubiera dicho. El Validador, si existía en forma débil, tendía a aceptarlas porque ya venían “llenas”. El resultado era un escenario testeable en apariencia y falso respecto del sistema real.

> **Comportamiento esperado:** si el usuario no afirmó una parte, o no es cuantificable, el escenario **se entrega igual** pero con la marca a la vista, y **hay pregunta**. No se inventa el umbral. No se sella. El usuario confirma, corrige o elige; si abre una ambigüedad nueva, se vuelve a marcar.

**⚠️ Output antes de la corrección (extracto de patrón):**

- “Los datos de las tarjetas… recontra seguros” → escenario de Seguridad con cifrado TLS/AES y “detección en menos de 1 minuto” **sin** que el enunciado diera esos números.
- “Correr sin problemas raros de visualización” → se rellenaba Runtime, PWA y un % de páginas usables, clasificando Usabilidad o Portabilidad **sin** preguntar cuál de las dos.
- Un `o` en la fuente (“stakeholder **o** proveedor”) se resolvía eligiendo uno en silencio, en lugar de vetar la auditoría hasta que el usuario eligiera.

**✅ Corrección aplicada:** reglas más estrictas **dentro de las especificaciones** de cada skill, no como consejo en el chat.

En [`.cursor/rules/1-generador-sei.mdc`](../../../.cursor/rules/1-generador-sei.mdc):

- Solo se puede inferir **Ambiente** y **Artefacto**, y van con `[INFERIDO]`.
- Medida ausente o no cuantificable → `[INCOMPLETO]` y **3 medidas** (unidad y estadístico); el umbral lo pone el usuario. **Nunca inventar valores numéricos.** Un *Straw Man* se rótula como provisional.
- Un requerimiento que mezcla dos atributos → **dos archivos**, no uno combinado.
- Con marcas, el `.md` permanece en `docs/arquitectura/1-borradores/`. El Generador **nunca** escribe `[ESTADO: VALIDADO]`.

En [`.cursor/rules/2-validador-sei.mdc`](../../../.cursor/rules/2-validador-sei.mdc):

- Marcas `[INFERIDO]` / `[INCOMPLETO]` heredadas = **falla automática** (nunca debieron promoverse).
- Fuente no identificable, estímulo que es intención de negocio, artefacto “el sistema”, ambiente sin modo operacional, medida sin unidad o sin estadístico temporal → FALLA.
- Coherencia del atributo y “un escenario, un atributo” también fallan el sello.
- Ante falla está **prohibido** insinuar el escenario final: solo `template_sugerencias.md` con 3 opciones, o `template_preguntas.md` si el dato solo lo tiene el usuario.
- El sello exige re-correr el Paso 1 después de la elección del usuario.

En [`.cursor/rules/3-arbol-utilidad.mdc`](../../../.cursor/rules/3-arbol-utilidad.mdc):

- No lee `1-borradores/` ni escenarios sin `[ESTADO: VALIDADO]`.
- **No asigna** H/M/L. Si falta una etiqueta, no construye el árbol.
- No dictamina ASR: junta evidencia (artefacto, tácticas de la ficha, tradeoffs) y el arquitecto decide.

**✅ Output después de la corrección (MarketHub):** el Generador partió el primer requerimiento en Integrabilidad **y** Modificabilidad, marcó Ambiente/Artefacto como `[INFERIDO]` y la Medida como `[INCOMPLETO]`, y preguntó. El Validador rechazó fuentes con `o` y medidas no cuantificables, y no selló hasta que el usuario eligió (p. ej. proveedor logístico + cota de 8 componentes; Mercado Pago + 2 semanas; circuit breaker al 5.º intento). El Árbol no apareció hasta las seis pares de letras. Lo que antes se “rellenaba por cortesía” ahora o está afirmado por el usuario, o está explícitamente marcado como pendiente.

---


## 4. Limitaciones conocidas

- **Clasifica en el atributo general de la tabla, no en el subatributo.** Por defecto la skill se queda en el nombre de capítulo (Modificabilidad, Seguridad, Integrabilidad, …) y no baja al refinamiento que el escenario ya sugiere. El caso típico es tomar **Modificabilidad** cuando el estímulo es mover la interfaz a otro navegador o dispositivo: eso, en la propia ficha, es el sabor **Portabilidad** (aislar dependencias de plataforma), no un cambio de funcionalidad cualquiera. La causa está en cómo está tabulada la bibliografía que la skill puede leer. En [`docs/referencia/Atributos de Calidad 3c62d4a554c581f5875de3690ef35a05.md`](../../referencia/Atributos%20de%20Calidad%203c62d4a554c581f5875de3690ef35a05.md) el índice maestro lista los diez atributos de *Software Architecture in Practice* (Bass, Clements y Kazman); Portabilidad no tiene ficha propia, aparece como sub-sabor dentro de [`docs/referencia/atributos/🧩 Modificabilidad (Modifiability) 3cd2d4a554c581299ffff22ff5b8e181.md`](../../referencia/atributos/🧩%20Modificabilidad%20(Modifiability)%203cd2d4a554c581299ffff22ff5b8e181.md). El árbol, más tarde, declara **ISO 9126** como taxonomía por defecto ([`.cursor/rules/3-arbol-utilidad.mdc`](../../../.cursor/rules/3-arbol-utilidad.mdc); las seis características están en [`docs/referencia/atributos/Clasificaciones de Atributos de Calidad 3c62d4a554c5817e999cef3bb762d6ad.md`](../../referencia/atributos/Clasificaciones%20de%20Atributos%20de%20Calidad%203c62d4a554c5817e999cef3bb762d6ad.md), donde Portability *sí* es característica de nivel 2). El Generador y el Validador, sin embargo, clasifican *antes* del árbol y se anclan a esa tabla SAP de diez nombres: eligen la fila que existe (Modificabilidad) en lugar del subfactor ISO 9126 / sabor SAP (Portabilidad). En MarketHub hizo falta que el usuario *corrigiera* el atributo a Portabilidad; la skill no lo promovió sola. El mismo sesgo puede aplastar Escalabilidad bajo Modificabilidad, o Confidencialidad e Integridad bajo un único “Seguridad”, y deja refinamientos pobres hasta que el Árbol (Skill iii) los reconstruye a posteriori.

- **Ante un cambio de escenario que no estaba en sus opciones, completa las seis partes en vez de ser crítica con la teoría.** El Validador ([`.cursor/rules/2-validador-sei.mdc`](../../../.cursor/rules/2-validador-sei.mdc)) está diseñado para vetar con exactamente tres opciones concretas (`docs/templates/template_sugerencias.md`). Si el usuario responde con algo **fuera de esas tres** —otro atributo, otra medida, otro ambiente, un mecanismo que la ficha no lista como estímulo de ese QA— la skill **no contradice** al dueño del sistema: absorbe el dato, rellena Fuente–Estímulo–Artefacto–Ambiente–Respuesta–Medida y sigue, aunque el conjunto deje de ser coherente con [`docs/referencia/teoria_sei.md`](../../referencia/teoria_sei.md) o con la ficha del atributo declarado. Prioriza el cierre de las seis casillas y el respeto de la voluntad del usuario por sobre un segundo veto teórico. En MarketHub se vio cuando el escenario de renderizado pasó de Usabilidad a Portabilidad con una medida de latencia (25 s), propia de Rendimiento según la ficha de Performance, o cuando el de tarjetas en la base de datos se reescribió como circuit breaker en el checkout: se completó el template SEI sin frenar por incoherencia de atributo hasta que una auditoría *posterior* lo objetó. No hay un modo “el usuario se equivocó respecto de la teoría”; hay un modo “el usuario afirmó, entonces las seis partes tienen que cerrar”.

## 5. Aspectos positivos de este flujo

- **Baja fricción: el usuario no tiene que saber SEI para arrancar.** Habla en lenguaje de negocio (“recontra seguros”, “si cambia el SDK no se rompe el núcleo”) y el flujo le devuelve una tabla reconocible más la pregunta puntual de lo que todavía no afirmó — casi siempre la Medida de Respuesta, a veces Ambiente o Artefacto. No lo deja frente a un template vacío ni le exige el umbral en el primer turno. La conversación se siente guiada: confirmar o elegir, no redactar las seis partes desde cero. Eso es lo que el contrato de plantillas *hace por el usuario*; el detalle de cada archivo está en la sección 3.

- **Pipeline en el que una skill controla a la otra.** El Generador ([`.cursor/rules/1-generador-sei.mdc`](../../../.cursor/rules/1-generador-sei.mdc)) no puede sellar: produce borrador. El Validador ([`.cursor/rules/2-validador-sei.mdc`](../../../.cursor/rules/2-validador-sei.mdc)) no reescribe el requerimiento: veta lo que el Generador dejó pasar (marcas heredadas, fuente con `o`, medida sin unidad) y obliga a una segunda pasada. El Árbol no lee borradores. El control no es un segundo chatbot paralelo: es el paso siguiente del mismo pipeline, con poder de veto sobre el anterior. En MarketHub, Integrabilidad y Modificabilidad “cerradas” por el usuario no se promovieron hasta que el Validador forzó una entidad y un estímulo únicos.
