---
name: generar-escenarios-calidad
description: Genera escenarios de atributos de calidad usando el template de 6 partes del SEI (estímulo, fuente del estímulo, artefacto, entorno, respuesta, medida de la respuesta) a partir de una descripción de sistema o requerimiento en lenguaje natural. Usar esta skill cuando el usuario pida redactar, generar o completar un escenario de calidad, cuando mencione atributos de calidad (performance, disponibilidad, seguridad, modificabilidad, usabilidad, etc.) sobre un sistema concreto, o cuando pida aplicar el "template de 6 partes" o "quality attribute scenario" del SEI.
---

# Generar escenarios de atributos de calidad

## Qué hace

Dado un input del usuario (una descripción de sistema, un requerimiento suelto, o un enunciado de TP), la skill sigue esta secuencia:

1. **Clasificar el/los atributo(s) de calidad implicados.** Leer el input y, usando `../../../shared/glosario-atributos-calidad.md`, identificar qué atributo de calidad es relevante (Disponibilidad, Performance, Seguridad, Modificabilidad, Usabilidad, etc.). Si no es evidente, proponer el más probable y aclarar explícitamente esa elección antes de continuar — nunca asumirlo en silencio.
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
