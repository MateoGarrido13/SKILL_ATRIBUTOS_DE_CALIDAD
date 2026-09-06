---
name: chequear-completitud-escenario
description: Evalúa si un escenario de atributo de calidad ya redactado tiene las 6 partes del template del SEI (estímulo, fuente del estímulo, artefacto, entorno, respuesta, medida de la respuesta) presentes y correctamente definidas, y si falta o está ambigua alguna parte, sugiere cómo completarla o corregirla. Usar esta skill cuando el usuario pegue un escenario de calidad ya escrito y pida revisarlo, validarlo, chequear si está completo, o pregunte qué le falta a un escenario.
---

# Chequear completitud de un escenario de calidad

## Qué hace

Dado un escenario ya redactado (en formato libre o ya separado en partes), la skill sigue esta secuencia:

1. **Identificar las 6 partes presentes** en el escenario que el usuario pegó. Si viene en formato libre (una o dos oraciones), separar mentalmente qué fragmento corresponde a cada una de las 6 partes del template (ver `../../../shared/template-6-partes-sei.md`).
2. **Evaluar cada una de las 6 partes** con el criterio POSITIVO de `docs/conocimiento/rubric-completitud.md` — esa rúbrica define qué hace "completa" a cada parte (ej. la Medida de la respuesta solo es completa si es cuantificable). No alcanza con mirar `docs/condiciones/`, que es negativo (anti-patrones); acá el chequeo primario es contra el criterio positivo de aceptación.
3. **Cruzar también contra `docs/condiciones/condiciones.md`** (los mismos 8 tips prácticos, en versión "criterio de detección") para señales adicionales de mala clasificación del atributo de calidad o de confusión entre partes (ej. una regla de negocio disfrazada de QA, o una respuesta que en realidad describe una táctica técnica).
4. **Dar un veredicto explícito**: "Completo" o "Incompleto". Un escenario es completo solo si las 6 partes pasan individualmente el criterio de la rúbrica.
5. **Si es incompleto, señalar exactamente qué parte(s) fallan y por qué** (citando el criterio de la rúbrica que no se cumple), y sugerir una corrección concreta. Si el valor correcto no se puede derivar del contexto dado, proponerlo marcado como `Asunción: ...` con justificación — el mismo criterio que usa `heuristica-inferencia.md` de `generar-escenarios-calidad`. Si el valor correcto sí es conocido (por ejemplo, porque el usuario lo dio en otra parte del mensaje), no marcarlo como asunción.

## Fuente de conocimiento base

- `../../../shared/template-6-partes-sei.md` — para separar el escenario en sus 6 partes (paso 1).
- `docs/conocimiento/rubric-completitud.md` — criterio positivo de aceptación por parte (paso 2).

## Reglas de proceso

- `docs/condiciones/condiciones.md` — 8 criterios de detección adicionales (paso 3).

## Regla obligatoria sobre tests

**NUNCA leas, listes ni referencies el contenido de `tests-no-leer/` (ni la carpeta raíz del repo ni ninguna de sus subcarpetas), ni siquiera para consulta interna.** Esa carpeta existe solo para que una persona (no la skill) valide el comportamiento con ejemplos conocidos.
