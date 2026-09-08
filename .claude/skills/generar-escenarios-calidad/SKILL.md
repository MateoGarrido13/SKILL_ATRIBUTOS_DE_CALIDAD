---
name: generar-escenarios-calidad
description: Genera escenarios de atributos de calidad usando el template de 6 partes del SEI (estímulo, fuente del estímulo, artefacto, entorno, respuesta, medida de la respuesta) a partir de una descripción de sistema o requerimiento en lenguaje natural. Usar esta skill cuando el usuario pida redactar, generar o completar un escenario de calidad, cuando mencione atributos de calidad (performance, disponibilidad, seguridad, modificabilidad, usabilidad, etc.) sobre un sistema concreto, o cuando pida aplicar el "template de 6 partes" o "quality attribute scenario" del SEI.
---

# Generar escenarios de atributos de calidad

## Qué hace

Dado un input del usuario (una descripción de sistema, un requerimiento suelto, o un enunciado de TP), la skill sigue esta secuencia:

1. **Clasificar el/los atributo(s) de calidad implicados**, siguiendo estos sub-pasos en orden, sin saltarlos:
   1. **Recorrer sistemáticamente los atributos de `../../../shared/glosario-atributos-calidad.md`** contra el input, y para cada uno que tenga alguna señal textual (aunque sea débil), anotar brevemente qué evidencia concreta del texto la respalda. No evaluar un único candidato "obvio" y parar ahí — revisar la lista completa antes de decidir.
   2. **Si un solo atributo tiene evidencia clara** y el resto no tiene anclaje real en el texto (solo se podría "estirar" hasta ahí), elegir ese atributo y seguir con el paso 2.
   3. **Si dos o más atributos tienen evidencia igualmente válida** (ambigüedad real en el texto, no una lectura forzada), no elegir en silencio: señalarlo explícitamente al usuario, explicar en una línea qué evidencia respalda cada lectura, y resolver la ambigüedad priorizando la interpretación con el anclaje **más directo y explícito** en el texto por sobre la que requiere más pasos de inferencia. Ofrecer generar también las otras lecturas si el usuario las pide.
   4. **Prohibido elegir una interpretación que dependa de un hecho inventado.** Está prohibido elegir una interpretación que requiera asumir un evento, actor o condición que el texto **no** menciona (ej. "el sistema externo falla", "dos usuarios actúan al mismo tiempo", "alguien intenta estafar"), si existe una interpretación alternativa que se pueda sostener usando únicamente hechos explícitamente presentes en el texto. Antes de redactar el escenario final, aplicar como paso obligatorio y visible esta autopregunta: *"¿La interpretación que elegí depende de algo que el texto no dice? Si sí, ¿hay otra lectura que no lo necesite? Si la hay, descarto la que inventa y uso la que no inventa, aunque la que inventa sea más rica o interesante de desarrollar."* Esto no significa que siempre haya una única respuesta correcta: cuando dos o más atributos tienen evidencia real y **ninguno** requiere inventar nada, la ambigüedad es legítima y hay que declararla (sub-paso 3), no forzar una sola elección. Esta regla solo descarta las lecturas que necesitan una premisa inventada — no elige "la mejor" entre las que no inventan nada.
2. **Extraer o inferir cada una de las 6 partes por separado.** No redactar el escenario de corrido: ir parte por parte (Fuente del estímulo, Estímulo, Artefacto, Entorno, Respuesta, Medida de la respuesta), siguiendo la definición y los ejemplos de `../../../shared/template-6-partes-sei.md`.
3. **Completar las partes que el input no menciona explícitamente**, aplicando `docs/conocimiento/heuristica-inferencia.md`. Esto es especialmente crítico para la Medida de la respuesta: proponer siempre un valor concreto y cuantificable, marcado como `Asunción: ...` con una justificación breve ligada al contexto del sistema — nunca presentarlo como si fuera un dato dado por el usuario.
4. **Validar el escenario resultante contra `docs/condiciones/condiciones.md`** (los 8 anti-patrones destilados de los ejercicios 1 y 2 del TP) antes de darlo por final. Si el escenario cae en alguno de esos errores (regla de negocio disfrazada de QA, estímulo genérico, respuesta puramente funcional, respuesta que describe una táctica en vez de un resultado observable, etc.), corregirlo antes de presentarlo.
5. **Redactar el escenario completo en el formato de salida fijo**, siempre una tabla markdown de dos columnas `Parte | Valor` con exactamente estas 6 filas, en este orden: Fuente del estímulo, Estímulo, Artefacto, Entorno, Respuesta, Medida de la respuesta. Después de la tabla, agregar opcionalmente la oración final que las une en un escenario legible. Este formato es el mismo que usan `../../../shared/template-6-partes-sei.md` y `tests-no-leer/` — no improvisar un formato distinto, porque `chequear-completitud-escenario` y `arbol-utilidad` esperan consumir escenarios en esta misma estructura.

## Fuente de conocimiento base

- `../../../shared/glosario-atributos-calidad.md` — para clasificar el atributo (paso 1).
- `../../../shared/template-6-partes-sei.md` — para la estructura y ejemplos de las 6 partes (paso 2).
- `docs/conocimiento/heuristica-inferencia.md` — mecanismo de inferencia con asunción explícita (paso 3).

## Reglas de proceso

- `docs/condiciones/condiciones.md` — 8 reglas de "evitar al construir" (paso 4).

## Regla obligatoria sobre tests

**NUNCA leas, listes ni referencies el contenido de `tests-no-leer/` (ni la carpeta raíz del repo ni ninguna de sus subcarpetas), ni siquiera para consulta interna.** Esa carpeta existe solo para que una persona (no la skill) valide el comportamiento con ejemplos conocidos.
