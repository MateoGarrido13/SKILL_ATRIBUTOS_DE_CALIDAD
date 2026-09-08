# Skills de Atributos de Calidad (SEI)

Tres Agent Skills para el Trabajo Práctico. Transforman un requerimiento crudo en escenarios SEI de 6 partes y cuando el stakeholder ya los priorizó, en un Árbol de Utilidad. Cada skill se activa por la carpeta en la que estás trabajando; no se invocan por nombre.

La única fuente de verdad es la bibliografía en `docs/referencia/`. Las skills no aportan conocimiento externo. La lógica vive en `.cursor/rules/`; el formato, en `docs/templates/`. Todo artefacto de trabajo se escribe en `docs/arquitectura/`.


---

## Orden de uso (flujo definitivo)

El pipeline de este workspace **especifica primero y prioriza después**. No es el orden de la filmina (árbol → template): acá el árbol no se arma hasta que hay escenarios sellados **y** las dos etiquetas H/M/L de cada hoja.

```
requerimiento crudo
        │
        ▼
┌───────────────────┐
│  Skill i          │  1-borradores/   → escenario [ESTADO: BORRADOR]
│  Generador SEI    │                    + preguntas (si hay marcas)
└─────────┬─────────┘
          │  respondés inferencias y métricas hasta 6/6
          ▼
┌───────────────────┐
│  Skill ii         │  2-escenarios/   → auditoría (3 opciones)
│  Validador SEI    │                  o [ESTADO: VALIDADO]
└─────────┬─────────┘
          │  si rechaza: el .md vuelve a 1-borradores/ (Skill i)
          │  si sella: el .md vive solo en 2-escenarios/
          ▼
┌───────────────────┐
│  Skill iii        │  3-arbol-utilidad/
│  Árbol de Utilidad│
│                   │  sin H/M/L → solo template_pregunta_prioridad.md
│                   │             (no escribe el árbol)
│                   │  con H/M/L → árbol + cobertura + evidencia ASR
└───────────────────┘
          │
          ▼
     arquitecto dictamina el impacto (qué hojas son ASR)
```

| Orden | Dónde trabajás | Skill | Qué hace | Qué tenés que hacer vos |
|------:|----------------|-------|----------|-------------------------|
| 1 | `docs/arquitectura/1-borradores/` o el chat | **i — Generador** | Estructura el texto en las 6 partes. Entrega siempre el escenario, con `[INFERIDO]` / `[INCOMPLETO]` si falta algo, y cierra con preguntas. | Confirmar inferencias y aportar métricas cuantificables. |
| 2 | Misma carpeta, mismas respuestas | **i — Generador** | Reescribe el `.md` sin marcas cuando las 6 partes están cerradas. | Revisar y promover a `2-escenarios/`. |
| 3 | `docs/arquitectura/2-escenarios/` | **ii — Validador** | Audita. Rechaza con 3 opciones o sella `[ESTADO: VALIDADO]`. | Elegir una opción, proponer otra o aportar el dato. |
| 4 | `docs/arquitectura/3-arbol-utilidad/` o el chat | **iii — Árbol** | **No construye el árbol.** Emite `template_pregunta_prioridad.md`: pide Prioridad de negocio y Dificultad técnica, y explica qué implicaría H / M / L a la hora de diseñar. | Asignar las dos letras por escenario. |
| 5 | Misma carpeta, con las letras ya dichas | **iii — Árbol** | Escribe el árbol (taxonomía + tabla de 5 columnas + cobertura/alertas + evidencia de ASR). | Validar agrupación y refinamientos. Dictaminar qué hojas son ASR. |

Activación: las tres reglas tienen `alwaysApply: false` y un glob por carpeta. Abrí o editá un archivo de esa ruta (o mencioná la carpeta en el chat) para que corra la skill que corresponde. No saltees etapas: ii no sella un borrador con marcas; iii no lee `1-borradores/` ni entra un `.md` sin `[ESTADO: VALIDADO]`.

---

## Cómo cargar el requerimiento

Formatos: `.txt` o `.md`. No hay estructura obligatoria — alcanza una frase de un stakeholder o un párrafo desordenado.

Dos vías, equivalentes:

1. Dejar el archivo en `docs/arquitectura/1-borradores/`.
2. Describirlo por chat. La Skill i deja el `.md` resultante en esa misma carpeta.

Si cargaste un `.txt`, queda intacto para siempre: es el registro del requerimiento original. El trabajo se hace sobre un `.md` aparte. Si cargaste un `.md`, ese mismo archivo se reescribe hasta validarse. Un escenario validado existe **únicamente** en `2-escenarios/`.

Un requerimiento que mezcla dos atributos produce **dos archivos**, no uno combinado.

---

## Las tres skills

### Skill i — Generador de Escenarios SEI

| | |
|---|---|
| **Regla** | `.cursor/rules/1-generador-sei.mdc` |
| **Se activa en** | `docs/arquitectura/1-borradores/*` |
| **Propósito** | Pasar de texto libre a un Escenario de Calidad SEI (Fuente → Estímulo → Artefacto → Ambiente → Respuesta → Medida de Respuesta). |
| **Entrega** | Siempre dos bloques: `template_escenarios.md` con `[ESTADO: BORRADOR]`, y `template_preguntas.md` (una pregunta por marca). |
| **Tu rol** | Confirmar lo inferido y dar las métricas. Sin eso el escenario no avanza. |

Qué puede y qué no:

- Entrega el escenario aunque falte información: marca la parte y pregunta; no se detiene en un listado de dudas.
- Solo infiere **Ambiente** y **Artefacto**, y los marca `[INFERIDO]`.
- Nunca inventa el número de la Medida de Respuesta. Si falta o no es cuantificable, marca `[INCOMPLETO]` y propone **3 medidas** (unidad y estadístico); el umbral lo ponés vos.
- No asigna H/M/L, no habla de ASR, no adelanta tácticas ni tradeoffs.

Mientras quede una marca, el `.md` permanece bloqueado en `1-borradores/`.

### Skill ii — Validador de Escenarios SEI

| | |
|---|---|
| **Regla** | `.cursor/rules/2-validador-sei.mdc` |
| **Se activa en** | `docs/arquitectura/2-escenarios/*.md` |
| **Propósito** | Auditar el escenario promovido. Es un auditor con veto, no un redactor. |
| **Entrega** | Rechazo con `template_sugerencias.md` (exactamente 3 opciones) o, si falta un dato que solo vos tenés, `template_preguntas.md`. Aprobación: el mismo `template_escenarios.md` sellado `[ESTADO: VALIDADO]`. |
| **Tu rol** | Elegir una opción, proponer otra o aportar el dato. El sello no se emite hasta que la auditoría pase limpia. |

Basta una falla para rechazar: fuente no identificable, estímulo que describe la respuesta, artefacto demasiado genérico, ambiente sin modo operacional, respuesta confundida con la métrica, medida no cuantificable, dos atributos en el mismo archivo, o marcas `[INFERIDO]` / `[INCOMPLETO]` que no debieron promoverse.

Ante un rechazo no adelanta “cómo quedaría” el escenario final. Tras el sello, el `.md` existe una sola vez en `2-escenarios/` y se borra su copia de `1-borradores/`. Si rechaza, hace lo inverso y el ciclo vuelve a la Skill i. Los `.txt` originales no se tocan. Ante duda sobre qué copia borrar, pregunta.

### Skill iii — Constructor de Árbol de Utilidad

| | |
|---|---|
| **Regla** | `.cursor/rules/3-arbol-utilidad.mdc` |
| **Se activa en** | `docs/arquitectura/3-arbol-utilidad/*.md` |
| **Propósito** | Consolidar los escenarios **ya validados** en un Árbol de Utilidad priorizado. Absorbe la recolección de H/M/L y la evidencia de ASR: no hay una cuarta skill. |
| **Entrada** | Solo `.md` con `[ESTADO: VALIDADO]` en `2-escenarios/`. Ignora borradores, marcas y medidas no cuantificables. No lee `1-borradores/` ni `dataset_test/`. |
| **Tu rol** | Poner las dos letras de cada hoja y, después, dictaminar si el impacto arquitectónico convierte a esa hoja en ASR. |

Las etiquetas son un **insumo, no una columna a rellenar después**. Las busca en este orden: archivo del árbol abierto → chat → anotación H/M/L en el `.md` validado. Si falta una sola celda de una sola hoja, la compuerta está cerrada:

- no escribe ni reescribe nada en `3-arbol-utilidad/`
- no emite tabla, taxonomía, cobertura, alertas ni evidencia de ASR
- emite **solo** `template_pregunta_prioridad.md` (un bloque por escenario)

Esa plantilla pide las dos decisiones y explica, ancladas a *ese* escenario, qué implicaría cada nivel **a la hora de diseñar** — no solo que H es “High” y L es “Low”. Las tres opciones van con el mismo peso: no recomienda, no deja default. `template_preguntas.md` no se usa acá (esa es para datos faltantes del escenario).

Cuando las dos letras de todas las hojas están dichas, escribe un único `.md` (el archivo abierto o `arbol_base.md`):

1. **Taxonomía** encima de la tabla (default ISO 9126).
2. **Tabla** de `template_arbol.md`, sin mutar las cinco columnas: Atributo · Refinamiento · Escenario · Prioridad (Negocio) · Dificultad (Técnica).
3. **Cobertura y alertas** debajo: vacíos de cobertura; cada `(H, H)` como máxima prioridad; alerta de viabilidad si hay 3 o más `(H, H)`, o al menos 2 y son más de la mitad.
4. **Evidencia de ASR** lo especificarmeos a continuacion solo para Prioridad H o M: artefactos afectados, tácticas de la ficha, tradeoffs. La skill **nunca** escribe “esto es ASR”.

Las etiquetas viven en las columnas del árbol. No se copian hacia atrás a `2-escenarios/`.

---

## Qué significan las etiquetas y el ASR

Las asignás vos. La skill explica; no elige.

| | Prioridad de negocio | Dificultad técnica |
|---|---|---|
| **H** | Must-have: entregar el sistema sin cumplirlo pone el proyecto en riesgo. | Riesgo alto de no lograrlo. Alerta activa: condiciona decisiones estructurales, tácticas específicas y tradeoffs. |
| **M** | Importante; omitirlo no hace fracasar el proyecto. | Preocupa, pero el riesgo no es alto. |
| **L** | Deseable; no justifica esfuerzo significativo. | Alta confianza. No arrastra arquitectura; se puede postergar en el diseño inicial. |

Un requisito es **ASR** si y solo si cumple las dos condiciones de la teoría: impacto profundo en la arquitectura (juicio del arquitecto sobre la evidencia) **y** Prioridad **H o M**. Una hoja `(L, L)` entra al árbol y no es ASR. El encabezado de la tabla dice *Escenario (ASR)* porque así está la plantilla; no toda fila lo es.

---

## Marcas

| Marca | Significado | Quién la cierra |
|-------|-------------|-----------------|
| `[INFERIDO]` | Ambiente o Artefacto deducidos, no afirmados por vos | Confirmás o corregís (Skill i) |
| `[INCOMPLETO]` | Dato que la skill no puede inventar; casi siempre la Medida | Aportás el número (Skill i) |
| `[ESTADO: BORRADOR]` | Salió del Generador; no pasó auditoría | Lo reemplaza el Validador |
| `[ESTADO: VALIDADO]` | Auditoría limpia. Única llave de entrada al árbol | Lo emite la Skill ii |

No existe marca de ASR ni de H/M/L sobre el escenario sellado. Tampoco hay un tercer estado `[INDETERMINADO]`: sin etiquetas, la Skill iii no emite el árbol.

---

## Plantillas (contrato de formato)

Se rellenan; no se editan.

| Plantilla | Quién la emite | Para qué |
|-----------|----------------|----------|
| `template_escenarios.md` | i (borrador) y ii (sello) | Único formato de un escenario |
| `template_preguntas.md` | i, y ii si falta un dato que solo vos tenés | Cerrar partes del escenario |
| `template_sugerencias.md` | ii, ante rechazo | Tres opciones de corrección |
| `template_pregunta_prioridad.md` | iii, compuerta cerrada | Pedir H/M/L con implicancias de diseño |
| `template_arbol.md` | iii, compuerta abierta | Tabla de cinco columnas del árbol |

---

## Invariantes

- `docs/referencia/`, `docs/templates/` y `dataset_test/` son de **solo lectura**. La única zona de escritura es `docs/arquitectura/`.
- Un escenario = un `.md`. Vive en `1-borradores/` o en `2-escenarios/`, nunca en ambos. Sin versiones ni sufijos.
- Las skills no inventan métricas ni asignan H/M/L.
- Solo escenarios `[ESTADO: VALIDADO]` entran al árbol.
- Las tres skills le hablan al dueño del sistema: no nombran archivos de la bibliografía ni etapas del pipeline.

Antes de redactar, cada skill lee la bibliografía en el orden que fija su regla (teoría → índice o árbol → ficha del atributo → plantilla) y recién entonces toca `docs/arquitectura/`.

---

## Mapa del workspace

```
.cursor/rules/
  1-generador-sei.mdc          Skill i
  2-validador-sei.mdc          Skill ii
  3-arbol-utilidad.mdc         Skill iii

docs/referencia/               bibliografía — solo lectura
  teoria_sei.md                6 partes, validez de la medida, ASR, H/M/L
  Atributos de Calidad …md     índice maestro y tradeoffs
  Árbol de Utilidad …md        4 niveles y semántica de las etiquetas
  atributos/                   fichas, taxonomías, ejercicios resueltos

docs/templates/                contrato de formato — solo lectura

docs/arquitectura/             única zona de escritura
  1-borradores/                .txt originales (intactos) y .md en elaboración
  2-escenarios/                un .md por escenario sellado
  3-arbol-utilidad/            un .md del árbol (se reescribe in situ)

dataset_test/                  fixtures de contraste — solo lectura es conveniente quitarlo al utilizar la skill
```

`dataset_test/` no es insumo del árbol. Sirve para contrastar el comportamiento esperado (casos 6/6 y casos que deben rechazarse).
