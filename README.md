# skill-tp-atributos-calidad

Resuelve el ejercicio 4 del TP3 de Atributos de Calidad: desarrollar una o más skills (estilo Claude) para generar escenarios de atributos de calidad (templates de 6 partes del SEI), chequear su completitud, y elaborar un árbol de utilidad.

## Objetivo

Encapsular en 3 Claude Skills reutilizables el proceso de diseño de atributos de calidad enseñado en la materia (template de 6 partes del SEI + árbol de utilidad), de forma que cualquier sistema nuevo pueda pasar por el mismo flujo — generar escenarios, verificar que estén bien formados, y priorizarlos — sin depender de rehacer el razonamiento manual cada vez. El conocimiento de base (teoría) y el conocimiento de proceso (errores comunes, criterios de aceptación, criterios de priorización) quedan separados y documentados en `shared/` y en `docs/` de cada skill, en vez de vivir solo en la cabeza de quien resuelve el TP, y se validan contra ejemplos conocidos (`tests-no-leer/`) tal como exige el enunciado.

## Las 3 skills

| Skill | Inciso del enunciado | Qué resuelve |
|---|---|---|
| [`skill-generar-escenarios-calidad/`](./skill-generar-escenarios-calidad/SKILL.md) | i) | Genera escenarios de atributos de calidad con el template de 6 partes del SEI a partir de una descripción de sistema en lenguaje natural. |
| [`skill-chequear-completitud-escenario/`](./skill-chequear-completitud-escenario/SKILL.md) | ii) | Dado un escenario ya redactado, chequea si las 6 partes están presentes y bien definidas, y si falta alguna sugiere cómo completarla. |
| [`skill-arbol-utilidad/`](./skill-arbol-utilidad/SKILL.md) | iii) | A partir de atributos de calidad y sus escenarios, arma un árbol de utilidad priorizado por impacto de negocio y dificultad técnica. |

## Flujo sugerido de uso conjunto

```
1. skill-generar-escenarios-calidad
        ↓ (escenario en tabla Parte | Valor, 6 filas fijas)
2. skill-chequear-completitud-escenario
        ↓ (escenario validado/corregido, misma tabla)
3. skill-arbol-utilidad
        ↓ (árbol priorizado H/M/L)
```

Las 3 skills son independientes y cada una funciona sola, pero están pensadas para encadenarse: comparten el mismo formato de escenario — una tabla markdown `Parte | Valor` con exactamente 6 filas (Fuente del estímulo, Estímulo, Artefacto, Entorno, Respuesta, Medida de la respuesta) — para que el output de una sea directamente el input de la siguiente, sin necesidad de reformatear nada a mano.

## Qué es `shared/`

Es la base teórica común a las 3 skills, para evitar que cada una mantenga su propia copia de la misma taxonomía (el árbol de utilidad se construye sobre atributos de calidad, que a su vez se expresan como escenarios con el mismo template de 6 partes). Cada `SKILL.md` la referencia en vez de duplicarla.

| Archivo | Contenido |
|---|---|
| `template-6-partes-sei.md` | Las 6 partes del template SEI, los 4 ejercicios oficiales resueltos, y el desarrollo completo de un escenario de Escalabilidad. |
| `glosario-atributos-calidad.md` | Qué son los atributos de calidad, clasificaciones (ISO 9126/Mitre/IEEE 1061), los 10 atributos con capítulo propio en el libro + "Otros Atributos (Cap. 14)", los 7 atributos "extra" de la filmina, y el método QAW/Straw Man para relevarlos con stakeholders. |
| `metodo-arbol-utilidad.md` | Estructura de 4 niveles del árbol de utilidad, las etiquetas de priorización H/M/L, y su relación exacta con el template de 6 partes. |

La fuente primaria de los tres es **Notion** (páginas "Atributos de Calidad" y subpáginas, migradas casi textuales), con complementos puntuales de *Software Architecture in Practice* (Bass/Clements/Kazman) solo donde hacía falta llenar un hueco — en la práctica, Notion ya cubría el template y el árbol de utilidad por completo, así que no hizo falta agregar ningún complemento del libro.

## Qué es `docs/condiciones/` y `docs/conocimiento/` (dentro de cada skill)

Son dos cosas distintas, aunque ambas viven en `docs/` de cada skill:

- **`docs/condiciones/`** — anti-patrones: qué **evitar**. Se derivan de los 8 tips prácticos documentados en Notion a partir de errores concretos cometidos al resolver los ejercicios 1 y 2 del TP, reescritos con el verbo que corresponde a cada skill (evitar al construir / detectar al revisar / validar antes de priorizar).
- **`docs/conocimiento/`** — procedimientos propios de cada skill: la heurística de inferencia (`generar`), el rubric de completitud (`chequear`), y el criterio de priorización (`árbol`). **No vienen de ninguna fuente externa** (ni Notion ni el libro) — se redactaron a medida para cubrir lo que `shared/` no cubre: cómo actuar operacionalmente, no la teoría en sí.

## Qué es `tests-no-leer/`

> **Las skills NUNCA deben leer, listar ni referenciar el contenido de esta carpeta.** Existe solo para que una persona valide el comportamiento con ejemplos conocidos, tal como exige el enunciado.

| Caso | Fuente | Archivos | Qué valida |
|---|---|---|---|
| `caso-01-monopatines/` | TP3 + resolución propia (Notion, "TP Atributos Calidad") | 6 | Las 3 skills (generar, chequear, árbol) |
| `caso-02-sap-disponibilidad/` | Fig. 4.1 del libro SAP | 4 | generar + chequear |
| `caso-03-sap-performance/` | Fig. 9.1 del libro SAP | 4 | generar + chequear |
| `caso-04-sap-arbol-healthcare/` | Tabla 19.1 del libro SAP | 2 | Solo árbol-utilidad, con prioridades H/M/L **originales de los autores** como benchmark externo |

`caso-02` y `caso-03` no incluyen archivos de árbol de utilidad porque son escenarios aislados de un único atributo (Disponibilidad, Performance) — no un sistema completo con varios atributos de calidad para priorizar entre sí, que es lo que requiere un árbol de utilidad.

## Estructura de carpetas completa

```
skill-tp-atributos-calidad/
├── .gitignore
├── README.md
│
├── shared/
│   ├── template-6-partes-sei.md
│   ├── glosario-atributos-calidad.md
│   └── metodo-arbol-utilidad.md
│
├── skill-generar-escenarios-calidad/
│   ├── SKILL.md
│   ├── PLAN.md
│   └── docs/
│       ├── conocimiento/
│       │   └── heuristica-inferencia.md
│       └── condiciones/
│           └── condiciones.md
│
├── skill-chequear-completitud-escenario/
│   ├── SKILL.md
│   ├── PLAN.md
│   └── docs/
│       ├── conocimiento/
│       │   └── rubric-completitud.md
│       └── condiciones/
│           └── condiciones.md
│
├── skill-arbol-utilidad/
│   ├── SKILL.md
│   ├── PLAN.md
│   └── docs/
│       ├── conocimiento/
│       │   └── criterio-priorizacion.md
│       └── condiciones/
│           └── condiciones.md
│
└── tests-no-leer/
    ├── caso-01-monopatines/          (6 archivos: generar/chequear/árbol × input/expected)
    ├── caso-02-sap-disponibilidad/   (4 archivos: generar/chequear × input/expected)
    ├── caso-03-sap-performance/      (4 archivos: generar/chequear × input/expected)
    └── caso-04-sap-arbol-healthcare/ (2 archivos: árbol × input/expected)
```

## Estado del proyecto

**Completo:**
- Estructura de carpetas y nombres definitivos (kebab-case).
- `shared/` con la base teórica de las 3 skills.
- `docs/condiciones/` y `docs/conocimiento/` de las 3 skills.
- `tests-no-leer/` con los 4 casos (16 archivos en total).
- Los 3 `SKILL.md` con el cuerpo real de instrucciones.

**Pendiente:**
- Correr los 4 casos de `tests-no-leer/` contra las skills reales (invocarlas con cada `*-input.md` y comparar el resultado contra el `*-expected.md` correspondiente) para validar que coinciden.
- Commit y subida del repo a git (todavía no se inicializó ni se hizo ningún commit).

## `.gitignore`

Excluye la carpeta local de material bibliográfico crudo (PDFs) porque el repo solo versiona markdown/texto. También excluye archivos de sistema y temporales estándar (`.DS_Store`, `*.tmp`, `*.log`, `node_modules/`, etc.).
