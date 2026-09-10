---
name: arbol-utilidad
description: Elabora un árbol de utilidad (utility tree) a partir de atributos de calidad identificados y sus escenarios, priorizando cada escenario según impacto de negocio y dificultad técnica. Usar esta skill cuando el usuario tenga un conjunto de atributos de calidad o escenarios ya definidos y pida armar, generar o priorizar un árbol de utilidad, o pregunte cómo priorizar escenarios de calidad para el diseño de una arquitectura.
---

# Elaborar árbol de utilidad

## Qué hace

Dado un conjunto de atributos de calidad y sus escenarios (idealmente ya generados por `generar-escenarios-calidad` y validados por `chequear-completitud-escenario`), la skill sigue esta secuencia:

1. **Recibir el conjunto de atributos de calidad y sus escenarios.** El formato esperado de cada escenario es la tabla `Parte | Valor` de 6 filas (Fuente del estímulo, Estímulo, Artefacto, Entorno, Respuesta, Medida de la respuesta) que produce `generar-escenarios-calidad` — el mismo que usan `../../../shared/template-6-partes-sei.md` y `tests-no-leer/`. Si vienen incompletos o en un formato distinto, pedir aclaración antes de continuar en vez de inventar escenarios que el usuario no dio.

   **Compuerta de validación (obligatoria, no salteable):** un escenario solo puede entrar al árbol si trae la marca `[ESTADO: VALIDADO]` de `chequear-completitud-escenario`, o si el usuario confirma explícitamente en el mismo mensaje que ya pasó ese chequeo. Si un escenario no trae esa marca ni esa confirmación, **no construir el árbol**: señalar cuáles escenarios faltan validar y pedir que pasen primero por `chequear-completitud-escenario` (o que el usuario confirme que ya están completos). Esto evita priorizar — y darle apariencia de terminado — a un escenario que en realidad tiene una parte vaga o ambigua sin detectar.
2. **Antes de priorizar, validar cada escenario contra `docs/condiciones/condiciones.md`** (los 3 tips de clasificación/derivación correcta del atributo de calidad). No tiene sentido asignar prioridad a un escenario que en realidad es una regla de negocio disfrazada, que no está anclado en una característica concreta del sistema, o que duplica otro escenario sin diferenciarse. Si un escenario falla esta validación, señalarlo y excluirlo (o corregirlo) antes de ubicarlo en el árbol.
3. **Armar la estructura del árbol** siguiendo `../../../shared/metodo-arbol-utilidad.md`: Utility (raíz) → atributo de calidad → sub-atributo/refinamiento → escenario concreto (hoja).
4. **Asignar las etiquetas H/M/L en ambos ejes** (importancia de negocio, dificultad técnica) usando `docs/conocimiento/criterio-priorizacion.md`. Evaluar los dos ejes de forma **independiente** — no dejar que la dificultad técnica influya en la importancia de negocio ni viceversa.
5. **Presentar el árbol en formato tabular**: columnas `Atributo | Refinamiento | Escenario | (Negocio, Técnica)`, seguido de una breve justificación de cada etiqueta cuando no sea obvia. Si alguna de las etiquetas surge de un valor asumido (por ejemplo, un escenario que a su vez tenía una medida de respuesta inferida), dejarlo notado.
6. **Chequeo de cobertura contra el sistema descrito (obligatorio antes de dar el árbol por terminado).** Si el usuario compartió, en este mensaje o en el hilo de la conversación, una descripción del sistema más amplia que los escenarios puntuales recibidos (ej. un contexto de negocio con varias preocupaciones mencionadas), recorrer esa descripción y verificar si hay alguna preocupación de calidad explícita que **no** quedó capturada en ningún escenario del árbol. Si encontrás una preocupación sin escenario asociado, **no presentar el árbol como completo**: listarla aparte, bajo un apartado "Cobertura — pendiente", y aclarar que hace falta generar (con `generar-escenarios-calidad`) un escenario para esa preocupación antes de considerar terminado el árbol. No es aceptable omitir en silencio una preocupación del enunciado solo porque el usuario no mandó explícitamente un escenario para ella.

## Fuente de conocimiento base

- `../../../shared/metodo-arbol-utilidad.md` — estructura del árbol y significado de las etiquetas H/M/L (paso 3).
- `docs/conocimiento/criterio-priorizacion.md` — criterio operacional para decidir el valor de cada etiqueta (paso 4).

## Reglas de proceso

- `docs/condiciones/condiciones.md` — 3 validaciones previas a priorizar (paso 2).

## Regla obligatoria sobre tests

**NUNCA leas, listes ni referencies el contenido de `tests-no-leer/` (ni la carpeta raíz del repo ni ninguna de sus subcarpetas), ni siquiera para consulta interna.** Esa carpeta existe solo para que una persona (no la skill) valide el comportamiento con ejemplos conocidos.
