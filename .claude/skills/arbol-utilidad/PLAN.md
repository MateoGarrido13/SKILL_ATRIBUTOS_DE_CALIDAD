# PLAN — arbol-utilidad

## Problema que resuelve
Corresponde al inciso **iii)** del ejercicio 4 de TP3: "elaborar un árbol de utilidad". Dado un conjunto de atributos de calidad identificados y sus escenarios (idealmente ya generados y validados por las otras dos skills), la skill arma un árbol de utilidad (utility -> atributos de calidad -> sub-características -> escenarios) y prioriza cada escenario según impacto de negocio y dificultad técnica.

## Inputs que recibe del usuario
- Lista de atributos de calidad relevantes para el sistema.
- Escenarios asociados a esos atributos (generados con `generar-escenarios-calidad` y, opcionalmente, ya validados con `chequear-completitud-escenario`).
- Opcionalmente, criterios de priorización propios del negocio (ej. qué atributo es más crítico para el cliente).

## Fuente de conocimiento base

| Fuente | Sección/página específica | Va a shared/ o a docs/conocimiento/ | Para qué sirve |
|---|---|---|---|
| Notion — página "Árbol de Utilidad" | Migrada casi íntegra | `shared/metodo-arbol-utilidad.md` | Estructura de 4 niveles, etiquetas H/M/L y relación con el template de 6 partes |
| Notion — página "Atributos de Calidad" (raíz + subpáginas) | Migrada casi íntegra | `shared/glosario-atributos-calidad.md` | Terminología de los atributos que forman el nivel 2 del árbol |
| Notion — página "Atributos de Calidad (Tips Prácticos)" (solo 3 de los 8 subpáginas, las de clasificación) | Migrada y luego distribuida | `docs/condiciones/condiciones.md` (tips 1, 4 y 5, reescritos como validación previa a priorizar) | Evitar priorizar un escenario mal clasificado o mal diferenciado de uno solapado |
| Redactado a medida (no viene de Notion ni del libro) | — | `docs/conocimiento/criterio-priorizacion.md` | Criterio operacional para decidir el valor de cada etiqueta H/M/L en ambos ejes (negocio y dificultad técnica) |

**Nota:** *Software Architecture in Practice* (Bass/Clements/Kazman) es la fuente original del método de árbol de utilidad, ya cubierta por completo en el Notion migrado — no hizo falta agregar complemento propio del libro en `shared/`. `caps4-5-keeling.pdf` (Design It!, Michael Keeling) no es fuente de ninguno de los archivos de `shared/`.

## Condiciones / reglas de la skill
Ver `docs/condiciones/condiciones.md`: solo los tips 1, 4 y 5 de los 8 tips prácticos de Notion (los relacionados a clasificación/derivación correcta del atributo de calidad), reescritos como validación previa a ubicar un escenario en una rama del árbol. Los 5 tips restantes (sobre cómo redactar bien las 6 partes de un escenario) no aplican acá — son responsabilidad de las otras dos skills.

## Ejemplos conocidos (testeo)
El enunciado de TP3 pide explícitamente que "la skill debe ser testeada con ejemplos conocidos". Los casos de prueba viven en `tests-no-leer/` (la skill nunca debe leerlos). **Solo dos de los cuatro casos aplican a esta skill** — `caso-02-sap-disponibilidad/` y `caso-03-sap-performance/` son escenarios aislados de un único atributo (no un sistema completo con varios QA para priorizar) y no incluyen archivos de árbol de utilidad, así que el testeo de esta skill depende únicamente de:

- **`caso-01-monopatines/`** — usa `arbol-utilidad-input.md` (los 6 escenarios completos + stakeholders del ejercicio 3-A de TP3) / `arbol-utilidad-expected.md` (árbol con las 6 ramas y etiquetas H/M/L). Estas etiquetas son una priorización **nueva**, derivada aplicando `criterio-priorizacion.md` — no existían en la resolución original de Notion.
- **`caso-04-sap-arbol-healthcare/`** — usa `arbol-utilidad-input.md` (13 escenarios de un sistema de salud, sin etiquetas) / `arbol-utilidad-expected.md` (árbol con las etiquetas H/M/L **originales de la Tabla 19.1 del libro SAP**, no derivadas por nosotros). Sirve como benchmark externo: si la skill llega a etiquetas distintas de las de los autores, no es necesariamente un error, pero vale la pena revisar la justificación caso por caso.
