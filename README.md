# skill-tp-atributos-calidad

Resuelve el ejercicio 4 del TP3 de Atributos de Calidad: desarrollar una o más skills (estilo Claude) para generar escenarios de atributos de calidad (templates de 6 partes del SEI), chequear su completitud, y elaborar un árbol de utilidad.

## Objetivo

Encapsular en 3 Claude Skills reutilizables el proceso de diseño de atributos de calidad enseñado en la materia (template de 6 partes del SEI + árbol de utilidad), de forma que cualquier sistema nuevo pueda pasar por el mismo flujo — generar escenarios, verificar que estén bien formados, y priorizarlos — sin depender de rehacer el razonamiento manual cada vez. El conocimiento de base (teoría) y el conocimiento de proceso (errores comunes, criterios de aceptación, criterios de priorización) quedan separados y documentados en `shared/` y en `docs/` de cada skill, en vez de vivir solo en la cabeza de quien resuelve el TP, y se validan contra ejemplos conocidos (`tests-no-leer/`) tal como exige el enunciado.

## Las 3 skills

Viven en `.claude/skills/`, el formato nativo de **Agent Skills** que Claude Code auto-detecta — así son skills "estilo Claude" de verdad, tal como pide el enunciado del TP, y no una carpeta cualquiera con instrucciones. Cada una tiene un `SKILL.md` con frontmatter YAML (`name` + `description`) que Claude usa para decidir sola cuándo activarse, sin que el usuario tenga que invocarla por comando explícito.

### 1. `generar-escenarios-calidad` — inciso i)

**Qué resuelve:** dada una descripción de sistema o un requerimiento suelto en lenguaje natural, redacta un escenario de atributo de calidad completo usando el template de 6 partes del SEI.

**Cuándo se activa:** cuando el usuario pide redactar/generar un escenario de calidad, menciona un atributo de calidad (performance, disponibilidad, seguridad, etc.) sobre un sistema concreto, o pide aplicar el "template de 6 partes" / "quality attribute scenario" del SEI.

**Inputs esperados:** una descripción del sistema o del contexto de negocio (puede ser un enunciado de TP, una historia de usuario, o una explicación libre); opcionalmente, el atributo de calidad puntual que interesa cubrir.

**Proceso interno** (definido en su `SKILL.md`):
1. Clasificar el/los atributo(s) de calidad implicados, usando `shared/glosario-atributos-calidad.md`.
2. Extraer o inferir cada una de las 6 partes por separado, siguiendo `shared/template-6-partes-sei.md`.
3. Completar lo que el input no menciona explícitamente aplicando `docs/conocimiento/heuristica-inferencia.md` — todo valor inferido queda marcado como `Asunción: ...` con justificación, nunca presentado como dato real.
4. Validar el resultado contra los 8 anti-patrones de `docs/condiciones/condiciones.md` antes de darlo por final.
5. Redactar la salida en el formato fijo (ver "Convención de formato" más abajo).

**Output:** tabla `Parte | Valor` de 6 filas + oración final opcional que las une.

### 2. `chequear-completitud-escenario` — inciso ii)

**Qué resuelve:** dado un escenario ya redactado (por un humano, por la skill anterior, o extraído de un TP), evalúa si las 6 partes están completas y bien definidas, y si falta o está ambigua alguna, sugiere cómo completarla.

**Cuándo se activa:** cuando el usuario pega un escenario de calidad ya escrito y pide revisarlo, validarlo, chequear si está completo, o pregunta qué le falta.

**Inputs esperados:** el texto del escenario (formato libre o ya separado en partes); opcionalmente, el atributo de calidad que dice representar, para chequear coherencia.

**Proceso interno:**
1. Identificar las 6 partes presentes en el escenario pegado.
2. Evaluar cada parte con el criterio **positivo** de `docs/conocimiento/rubric-completitud.md` (qué hace "completa" a cada una — ej. la medida de respuesta solo es completa si es cuantificable).
3. Cruzar también contra `docs/condiciones/condiciones.md` (los mismos 8 tips, en versión de detección) para señales de mala clasificación del atributo o confusión entre partes.
4. Dar veredicto explícito: **Completo** o **Incompleto**.
5. Si es incompleto, señalar exactamente qué parte(s) fallan y por qué, y sugerir la corrección — marcada como `Asunción: ...` solo si el valor correcto no se puede derivar del contexto dado.

**Output:** veredicto + detalle por parte + corrección sugerida.

### 3. `arbol-utilidad` — inciso iii)

**Qué resuelve:** dado un conjunto de atributos de calidad y sus escenarios (idealmente ya generados y validados por las otras dos skills), arma un árbol de utilidad y prioriza cada escenario por impacto de negocio y dificultad técnica.

**Cuándo se activa:** cuando el usuario tiene un conjunto de atributos/escenarios ya definidos y pide armar, generar o priorizar un árbol de utilidad, o pregunta cómo priorizar escenarios de calidad para el diseño de una arquitectura.

**Inputs esperados:** lista de atributos de calidad relevantes + sus escenarios (en el formato tabla `Parte | Valor` que produce `generar-escenarios-calidad`); opcionalmente, criterios de priorización propios del negocio.

**Proceso interno:**
1. Recibir el conjunto de atributos y escenarios en el formato esperado.
2. **Antes de priorizar**, validar cada escenario contra los 3 tips de clasificación de `docs/condiciones/condiciones.md` (los mismos 3 de las otras skills, pero acá enfocados en no priorizar algo mal clasificado o mal diferenciado de un atributo solapado).
3. Armar la estructura del árbol siguiendo `shared/metodo-arbol-utilidad.md`: Utility → atributo → sub-atributo/refinamiento → escenario (hoja).
4. Asignar las etiquetas H/M/L en ambos ejes (importancia de negocio, dificultad técnica), evaluados de forma **independiente**, usando `docs/conocimiento/criterio-priorizacion.md`.
5. Presentar el árbol en formato tabular: `Atributo | Refinamiento | Escenario | (Negocio, Técnica)`.

**Output:** árbol de utilidad completo, con etiquetas y su justificación.

## Convenciones de uso

- **Formato de escenario fijo y compartido**: las 3 skills usan siempre la misma tabla de 6 filas — `Fuente del estímulo`, `Estímulo`, `Artefacto`, `Entorno`, `Respuesta`, `Medida de la respuesta` — en ese orden, con columnas `Parte | Valor`. Es el mismo formato que usan `shared/template-6-partes-sei.md` y todos los `tests-no-leer/`. No es una preferencia estética: es lo que permite que el output de `generar-escenarios-calidad` sea consumible tal cual por `chequear-completitud-escenario` y por `arbol-utilidad`, sin que el usuario tenga que reformatear nada a mano.
- **Convención de asunción explícita**: cuando una skill propone un valor que el usuario no dio (típicamente una medida de respuesta numérica), siempre lo marca como `Asunción: [valor] por [justificación breve]` — nunca lo presenta como si fuera un dato real. Esta convención es compartida por `generar-escenarios-calidad` (`docs/conocimiento/heuristica-inferencia.md`) y `chequear-completitud-escenario` (misma lógica al sugerir correcciones). Si el valor correcto sí se puede derivar del contexto (por ejemplo, viene de una fuente conocida como el libro), no se marca como asunción.
- **`docs/condiciones/` vs. `docs/conocimiento/`**: son las dos carpetas de contenido propio de cada skill, y no se mezclan (ver detalle en la sección siguiente). Ambas se cargan como parte del razonamiento de la skill, nunca como memoria libre.
- **Activación por descripción, no por comando**: ninguna skill se invoca escribiendo su nombre — Claude decide activarla según el `description` del frontmatter de su `SKILL.md`, comparándolo contra lo que pide el usuario. Redactar bien esa descripción (qué hace + cuándo usarla) es lo que hace que la skill "dispare" en el momento correcto.
- **Nomenclatura kebab-case sin prefijos redundantes**: las carpetas de skill no llevan el prefijo `skill-` (viven ya dentro de `.claude/skills/`, que deja claro qué son) y su nombre de carpeta coincide exactamente con el campo `name:` del frontmatter de su `SKILL.md`.

## Flujos de uso

### Flujo combinado (las 3 en cadena)

```
1. generar-escenarios-calidad
        ↓ (escenario en tabla Parte | Valor, 6 filas fijas)
2. chequear-completitud-escenario
        ↓ (escenario validado/corregido, misma tabla)
3. arbol-utilidad
        ↓ (árbol priorizado H/M/L)
```

Ejemplo de secuencia real de uso:
1. *"Generame un escenario de calidad para: 'el sistema debe permitir cambiar la política de comisiones sin reiniciar el servicio'."* → `generar-escenarios-calidad` devuelve la tabla de 6 partes (Modificabilidad), con la medida de respuesta marcada como asunción si el enunciado no la daba.
2. *"¿Este escenario está completo?"* (pegando la tabla anterior) → `chequear-completitud-escenario` confirma **Completo** o señala qué parte falta.
3. *"Armá el árbol de utilidad con este escenario y estos otros dos"* → `arbol-utilidad` arma las ramas y asigna H/M/L a cada uno.

### Flujos independientes

Cada skill funciona sola sin pasar por las otras dos:
- Se puede pedir directamente un chequeo de completitud sobre un escenario que el usuario ya tenía escrito de antes (sin haber pasado por `generar-escenarios-calidad`).
- Se puede pedir un árbol de utilidad a partir de escenarios ya armados manualmente, siempre que respeten el formato de 6 partes.
- Se puede pedir solo la generación de un escenario, sin seguir con chequeo ni priorización.

## Qué es `shared/`

Es la base teórica común a las 3 skills, para evitar que cada una mantenga su propia copia de la misma taxonomía (el árbol de utilidad se construye sobre atributos de calidad, que a su vez se expresan como escenarios con el mismo template de 6 partes). Cada `SKILL.md` la referencia con rutas relativas en vez de duplicar contenido.

| Archivo | Contenido |
|---|---|
| `template-6-partes-sei.md` | Las 6 partes del template SEI, los 4 ejercicios oficiales resueltos, y el desarrollo completo de un escenario de Escalabilidad. |
| `glosario-atributos-calidad.md` | Qué son los atributos de calidad, clasificaciones (ISO 9126/Mitre/IEEE 1061), los 10 atributos con capítulo propio en el libro + "Otros Atributos (Cap. 14)", los 7 atributos "extra" de la filmina, y el método QAW/Straw Man para relevarlos con stakeholders. |
| `metodo-arbol-utilidad.md` | Estructura de 4 niveles del árbol de utilidad, las etiquetas de priorización H/M/L, y su relación exacta con el template de 6 partes. |

La fuente primaria de los tres es **Notion** (páginas "Atributos de Calidad" y subpáginas, migradas casi textuales), con complementos puntuales de *Software Architecture in Practice* (Bass/Clements/Kazman) solo donde hacía falta llenar un hueco — en la práctica, Notion ya cubría el template y el árbol de utilidad por completo, así que no hizo falta agregar ningún complemento del libro.

## Qué es `docs/condiciones/` y `docs/conocimiento/` (dentro de cada skill)

Son dos cosas distintas, aunque ambas viven en `docs/` de cada skill:

| | `docs/condiciones/` | `docs/conocimiento/` |
|---|---|---|
| Qué es | Anti-patrones: qué **evitar** | Procedimiento operacional propio de la skill |
| Fuente | Los 8 tips prácticos de Notion, sobre errores concretos cometidos al resolver los ejercicios 1 y 2 del TP | Redactado a medida — **no viene de Notion ni del libro** |
| Reparto | Los 8 tips van a `generar` y a `chequear` (reescritos con el verbo que corresponde); solo los 3 de clasificación/derivación del atributo van a `arbol-utilidad` | Un archivo por skill: `heuristica-inferencia.md` (generar), `rubric-completitud.md` (chequear), `criterio-priorizacion.md` (árbol) |
| Rol en el razonamiento | Filtro negativo, aplicado *después* de producir un resultado (o *antes* de priorizar, en el caso del árbol) | Mecanismo positivo, aplicado *durante* la construcción del resultado |

## Qué es `tests-no-leer/`

> **Las skills NUNCA deben leer, listar ni referenciar el contenido de esta carpeta.** Existe solo para que una persona valide el comportamiento con ejemplos conocidos, tal como exige el enunciado.

| Caso | Fuente | Archivos | Qué valida |
|---|---|---|---|
| `caso-01-monopatines/` | TP3 + resolución propia (Notion, "TP Atributos Calidad") | 6 | Las 3 skills (generar, chequear, árbol) |
| `caso-02-sap-disponibilidad/` | Fig. 4.1 del libro SAP | 4 | generar + chequear |
| `caso-03-sap-performance/` | Fig. 9.1 del libro SAP | 4 | generar + chequear |
| `caso-04-sap-arbol-healthcare/` | Tabla 19.1 del libro SAP | 2 | Solo árbol-utilidad, con prioridades H/M/L **originales de los autores** como benchmark externo |

Convención de nombres dentro de cada caso: `<skill>-input.md` / `<skill>-expected.md`, con `<skill>` en `{generar-escenarios, chequear-completitud, arbol-utilidad}`. `caso-02` y `caso-03` no incluyen archivos de árbol de utilidad porque son escenarios aislados de un único atributo (Disponibilidad, Performance) — no un sistema completo con varios atributos de calidad para priorizar entre sí, que es lo que requiere un árbol de utilidad.

Cómo se usa cada par en la práctica: se le da el `*-input.md` a la skill correspondiente (nunca la carpeta completa ni el `*-expected.md`), se compara manualmente el resultado contra el `*-expected.md`, y se evalúa si coinciden en contenido (no necesariamente carácter por carácter).

## Estructura de carpetas completa

```
skill-tp-atributos-calidad/
├── .gitignore
├── LICENSE
├── README.md
│
├── .claude/
│   └── skills/
│       ├── generar-escenarios-calidad/
│       │   ├── SKILL.md              (frontmatter + instrucciones — lo único que Claude carga siempre)
│       │   ├── PLAN.md               (análisis de diseño: problema, inputs, fuentes, tests)
│       │   └── docs/
│       │       ├── conocimiento/
│       │       │   └── heuristica-inferencia.md
│       │       └── condiciones/
│       │           └── condiciones.md
│       │
│       ├── chequear-completitud-escenario/
│       │   ├── SKILL.md
│       │   ├── PLAN.md
│       │   └── docs/
│       │       ├── conocimiento/
│       │       │   └── rubric-completitud.md
│       │       └── condiciones/
│       │           └── condiciones.md
│       │
│       └── arbol-utilidad/
│           ├── SKILL.md
│           ├── PLAN.md
│           └── docs/
│               ├── conocimiento/
│               │   └── criterio-priorizacion.md
│               └── condiciones/
│                   └── condiciones.md
│
├── shared/
│   ├── template-6-partes-sei.md
│   ├── glosario-atributos-calidad.md
│   └── metodo-arbol-utilidad.md
│
└── tests-no-leer/
    ├── caso-01-monopatines/          (6 archivos: generar/chequear/árbol × input/expected)
    ├── caso-02-sap-disponibilidad/   (4 archivos: generar/chequear × input/expected)
    ├── caso-03-sap-performance/      (4 archivos: generar/chequear × input/expected)
    └── caso-04-sap-arbol-healthcare/ (2 archivos: árbol × input/expected)
```

Cada `SKILL.md` referencia `shared/` con rutas relativas de 3 niveles (`../../../shared/...`), porque `.claude/skills/<nombre>/SKILL.md` queda 3 carpetas por debajo de la raíz del repo. Las referencias a `docs/condiciones/` y `docs/conocimiento/` no llevan ese prefijo porque son hijas directas de cada skill, sin importar dónde esté ubicada.

## Estado del proyecto

**Completo:**
- Estructura de carpetas y nombres definitivos (kebab-case), con las 3 skills reorganizadas al formato nativo `.claude/skills/<nombre>/` que Claude Code auto-detecta.
- `shared/` con la base teórica de las 3 skills.
- `docs/condiciones/` y `docs/conocimiento/` de las 3 skills.
- `tests-no-leer/` con los 4 casos (16 archivos en total).
- Los 3 `SKILL.md` con el cuerpo real de instrucciones, con todas las rutas relativas verificadas.
- Repo commiteado y subido a la rama `yaco` del remoto del equipo.

**Pendiente:**
- Correr los 4 casos de `tests-no-leer/` contra las skills reales (invocarlas con cada `*-input.md` y comparar el resultado contra el `*-expected.md` correspondiente) para validar que coinciden.

## `.gitignore`

Excluye la carpeta local de material bibliográfico crudo (PDFs) porque el repo solo versiona markdown/texto. También excluye archivos de sistema y temporales estándar (`.DS_Store`, `*.tmp`, `*.log`, `node_modules/`, etc.).
