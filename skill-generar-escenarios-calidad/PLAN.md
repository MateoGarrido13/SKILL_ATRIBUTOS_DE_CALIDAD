# PLAN — skill-generar-escenarios-calidad

## Problema que resuelve
Corresponde al inciso **i)** del ejercicio 4 de TP3: "generar atributos de calidad de acuerdo con los templates de 6 partes del SEI". Dada una descripción de un sistema o de un requerimiento no funcional en lenguaje natural, la skill redacta uno o más escenarios de atributos de calidad completos, usando el template de 6 partes del SEI (estímulo, fuente del estímulo, artefacto, entorno, respuesta, medida de la respuesta).

## Inputs que recibe del usuario
- Descripción del sistema o del contexto de negocio (puede ser un enunciado de TP, una historia de usuario, o una explicación libre).
- Opcionalmente, el atributo de calidad específico que le interesa cubrir (ej. "necesito un escenario de disponibilidad"); si no lo indica, la skill debe poder proponer qué atributos son relevantes antes de generar el escenario.

## Fuente de conocimiento base

| Fuente | Sección/página específica | Va a shared/ o a docs/conocimiento/ | Para qué sirve |
|---|---|---|---|
| Notion — página "Atributos de Calidad" (raíz + subpáginas de clasificaciones, los 10 atributos del libro, los 7 "extra" de la filmina, QAW) | Migrada casi íntegra | `shared/glosario-atributos-calidad.md` | Terminología, definiciones y escenarios generales por atributo |
| Notion — página "Escenarios de Calidad: el Template SEI de 6 Partes" + subpáginas de ejercicios | Migrada casi íntegra | `shared/template-6-partes-sei.md` | Definición formal del template de 6 partes, ejemplos resueltos y método paso a paso |
| Notion — página "Árbol de Utilidad" | Migrada casi íntegra | `shared/metodo-arbol-utilidad.md` | No se usa directamente en esta skill, pero es la fuente de contexto de por qué un escenario puede venir ya priorizado |
| Notion — página "Atributos de Calidad (Tips Prácticos)" (8 subpáginas) | Migrada y luego distribuida | `docs/condiciones/condiciones.md` (los 8 tips, reescritos como reglas de "evitar al construir") | Anti-patrones concretos detectados al resolver los ejercicios 1 y 2 del TP |
| Redactado a medida (no viene de Notion ni del libro) | — | `docs/conocimiento/heuristica-inferencia.md` | Mecanismo para inferir partes faltantes del template (especialmente la medida de respuesta), marcadas siempre como asunción explícita |

**Nota:** *Software Architecture in Practice* (Bass/Clements/Kazman) y sus capítulos son la fuente original de la que Notion ya deriva el template de 6 partes y el árbol de utilidad — no hizo falta agregar ningún complemento propio del libro en `shared/`, porque Notion ya cubría ambos conceptos por completo. `caps4-5-keeling.pdf` (Design It!, Michael Keeling) **no** es fuente de ninguno de los tres archivos de `shared/` — no se usó en esta skill.

## Condiciones / reglas de la skill
Ver `docs/condiciones/condiciones.md`: 8 reglas destiladas de los tips prácticos de Notion, reescritas como checklist de cosas a evitar al construir un escenario (tanto al clasificar el atributo como al redactar las 6 partes).

## Ejemplos conocidos (testeo)
El enunciado de TP3 pide explícitamente que "la skill debe ser testeada con ejemplos conocidos". Los casos de prueba viven en `tests-no-leer/` (la skill nunca debe leerlos) y son:

- **`caso-01-monopatines/`** (6 archivos) — del ejercicio 3 de TP3 y su resolución en Notion ("TP Atributos Calidad"). Usa `generar-escenarios-input.md` / `generar-escenarios-expected.md`. Incluye valores asumidos marcados explícitamente donde el enunciado original no daba un número.
- **`caso-02-sap-disponibilidad/`** (4 archivos) — Figura 4.1 del libro SAP (falla de servidor). Usa `generar-escenarios-input.md` / `generar-escenarios-expected.md`. Sin valores asumidos: es un ejemplo ya resuelto por los autores.
- **`caso-03-sap-performance/`** (4 archivos) — Figura 9.1 del libro SAP (500 usuarios, 2000 solicitudes). Usa `generar-escenarios-input.md` / `generar-escenarios-expected.md`. Sin valores asumidos.
- `caso-04-sap-arbol-healthcare/` no aplica a esta skill (es un input directo de árbol de utilidad ya armado, sin escenario individual que generar).
