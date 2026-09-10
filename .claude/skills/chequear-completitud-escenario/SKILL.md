---
name: chequear-completitud-escenario
description: Evalúa si un escenario de atributo de calidad ya redactado tiene las 6 partes del template del SEI (estímulo, fuente del estímulo, artefacto, entorno, respuesta, medida de la respuesta) presentes y correctamente definidas, y si falta o está ambigua alguna parte, sugiere cómo completarla o corregirla. Usar esta skill cuando el usuario pegue un escenario de calidad ya escrito y pida revisarlo, validarlo, chequear si está completo, o pregunte qué le falta a un escenario.
---

# Chequear completitud de un escenario de calidad

## Qué hace

Dado un escenario ya redactado (en formato libre o ya separado en partes), la skill sigue esta secuencia:

1. **Identificar las 6 partes presentes** en el escenario que el usuario pegó. Si viene en formato libre (una o dos oraciones), separar mentalmente qué fragmento corresponde a cada una de las 6 partes del template (ver `../../../shared/template-6-partes-sei.md`).
2. **Evaluar cada una de las 6 partes** con el criterio POSITIVO de `docs/conocimiento/rubric-completitud.md` — esa rúbrica define qué hace "completa" a cada parte (ej. la Medida de la respuesta solo es completa si es cuantificable). No alcanza con mirar `docs/condiciones/`, que es negativo (anti-patrones); acá el chequeo primario es contra el criterio positivo de aceptación. Presentar esta evaluación siempre como una **tabla de auditoría**, parte por parte:

   | Parte | Estado | Comentario |
   |---|---|---|
   | Fuente del estímulo | ✅ / ⚠️ Vaga / ❌ Falta | ... |
   | Estímulo | ✅ / ⚠️ Vaga / ❌ Falta | ... |
   | Artefacto | ✅ / ⚠️ Vaga / ❌ Falta | ... |
   | Entorno | ✅ / ⚠️ Vaga / ❌ Falta | ... |
   | Respuesta | ✅ / ⚠️ Vaga / ❌ Falta | ... |
   | Medida de la respuesta | ✅ / ⚠️ Vaga / ❌ Falta | ... |

   **Regla dura para la Medida de la respuesta**: solo es ✅ si contiene un número, unidad o criterio verificable. Palabras como "rápido", "seguro", "aceptable", "sin degradar" **nunca** son ✅ por sí solas, sin importar cuán bien redactadas estén las otras cinco partes.
3. **Cruzar también contra `docs/condiciones/condiciones.md`** (los mismos 8 tips prácticos, en versión "criterio de detección") para señales adicionales de mala clasificación del atributo de calidad o de confusión entre partes (ej. una regla de negocio disfrazada de QA, o una respuesta que en realidad describe una táctica técnica).
4. **Dar un veredicto explícito y binario, sin estado intermedio**: **`[ESTADO: VALIDADO]`** o **`[ESTADO: RECHAZADO]`**. Un escenario queda VALIDADO solo si las 6 partes tienen ✅ en la tabla de auditoría. Basta que una sola parte quede en ⚠️ o ❌ para que el veredicto sea RECHAZADO — no existe un "parcialmente completo". Esta marca de estado es la que consume `arbol-utilidad` para decidir si puede incorporar el escenario al árbol: sin `[ESTADO: VALIDADO]` explícito, ese escenario no puede priorizarse.
5. **Si el veredicto es RECHAZADO, entregar exactamente 3 opciones concretas de corrección por cada parte fallida** (no una sugerencia abierta ni un solo camino). Cada opción debe ser una propuesta ya redactada y verificable (con unidad, si aplica), para que el usuario elija o corrija sobre eso — no una pregunta genérica de "¿qué querés poner acá?". Si el valor correcto no se puede derivar del contexto dado, cada una de las 3 opciones debe ir marcada como `Asunción: ...` con justificación — el mismo criterio que usa `heuristica-inferencia.md` de `generar-escenarios-calidad`. Si el valor correcto sí es conocido (por ejemplo, porque el usuario lo dio en otra parte del mensaje), no marcarlo como asunción, y en ese caso alcanza con una corrección, no hacen falta las 3 opciones.
6. **Al sellar como VALIDADO un escenario que fue corregido en una vuelta anterior a partir de una respuesta del usuario** (por ejemplo, el usuario eligió una de las 3 opciones u otro valor propio), citar textualmente esa respuesta en el escenario final sellado — no resumirla como "se eligió la opción 2". Esto es lo que permite auditar después que el valor que quedó sellado salió de una decisión real del usuario y no de un supuesto del modelo.

## Fuente de conocimiento base

- `../../../shared/template-6-partes-sei.md` — para separar el escenario en sus 6 partes (paso 1).
- `docs/conocimiento/rubric-completitud.md` — criterio positivo de aceptación por parte (paso 2).

## Reglas de proceso

- `docs/condiciones/condiciones.md` — 8 criterios de detección adicionales (paso 3).

## Regla obligatoria sobre tests

**NUNCA leas, listes ni referencies el contenido de `tests-no-leer/` (ni la carpeta raíz del repo ni ninguna de sus subcarpetas), ni siquiera para consulta interna.** Esa carpeta existe solo para que una persona (no la skill) valide el comportamiento con ejemplos conocidos.
