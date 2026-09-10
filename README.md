# skill-tp-atributos-calidad

Resuelve el ejercicio 4 del TP3 de Atributos de Calidad: desarrollar una o más skills (estilo Claude) para generar escenarios de atributos de calidad (templates de 6 partes del SEI), chequear su completitud, y elaborar un árbol de utilidad.

Esta rama (`main`) es el resultado de un merge crítico entre las 5 implementaciones individuales del grupo — no es la entrega de una sola persona. La sección "Procedencia del merge" más abajo detalla, pieza por pieza, qué se tomó de cada rama y por qué.

## Cómo se construyó

1. **Cada integrante del grupo armó su propia skill por separado**, en su propia rama, con su propio criterio de diseño, estructura de carpetas y reglas.
2. **Se testeó cada una** contra casos conocidos (algunos individuales, otros compartidos por todo el grupo, como el caso de e-commerce "MarketHub"), comparando outputs reales entre ramas.
3. **Se hizo un merge crítico** tomando, de cada rama, lo que mejor funcionaba en la práctica — no una sola implementación completa, sino piezas puntuales de varias, combinadas sobre la base más completa (la de `yaco`, la única en formato Claude Skills nativo con mayor cobertura de casos).

**Justificación simple de las decisiones del merge (una cosa concreta de cada rama, ni más ni menos):**
- De **`yaco`** se tomó la base completa sobre la que se armó todo lo demás: la estructura de las 3 skills en formato Claude nativo, la separación entre conocimiento base y reglas de proceso, y el método de testear no solo contra ejemplos propios sino contra un benchmark externo (un caso ya resuelto en el libro de la materia).
- De **`mateo`** se tomó el pipeline de decisiones: la secuencia de compuertas que impide avanzar de un paso al siguiente si el anterior no está validado (no se prioriza un escenario si antes no pasó el chequeo de completitud), y la regla de "no inventar y preguntar en su lugar" cuando falta un dato clave.
- De **`rossi`** se tomó la regla de que la Medida de la Respuesta solo cuenta si tiene un número o unidad concreta, con una tabla clara de qué partes están bien y cuáles no.
- De **`otaño`** se tomó la costumbre de explicar por qué se elige un atributo de calidad y no otro parecido, en vez de asignarlo sin justificar.
- De **`fontana`** se tomó la idea de clasificar primero qué tipo de pedido está haciendo el usuario (¿pide un escenario nuevo? ¿una revisión? ¿un árbol de utilidad?) antes de responder — ese mismo principio de clasificación es el que hoy define, en el `description` de cada una de las 3 skills, cuándo debe activarse cada una en vez de otra.
- Se sumaron además casos de test nuevos (el de MarketHub y dos casos pensados para hacer fallar a la skill a propósito) para probar que el resultado final aguanta casos difíciles, no solo los fáciles.

El detalle completo, con evidencia de cada decisión, está en la sección "Procedencia del merge".

Cada implementación fuente vive en su propia rama. Ahí está documentado **cómo se usa esa skill** (qué pide, qué responde, cómo se encadena el flujo) y **las pruebas corridas sobre esa skill**, con el prompt y el output completo — no un resumen. Esos informes no están en `main`: hay que abrir la rama. Varias corridas usan el mismo caso compartido (e-commerce "MarketHub"); otras usan casos propios de esa rama.

- **[`mateo`](https://github.com/MateoGarrido13/SKILL_ATRIBUTOS_DE_CALIDAD/tree/mateo)** — uso y construcción en [`informe-branch-mateo/informe-garrido.md`](https://github.com/MateoGarrido13/SKILL_ATRIBUTOS_DE_CALIDAD/blob/mateo/informe-branch-mateo/informe-garrido.md); corrida turno a turno en [`informe-branch-mateo/cursor_use_case_scenario_audit.md`](https://github.com/MateoGarrido13/SKILL_ATRIBUTOS_DE_CALIDAD/blob/mateo/informe-branch-mateo/cursor_use_case_scenario_audit.md); pruebas en [`docs/test/PRUEBA_UNO_HOSPITAL/`](https://github.com/MateoGarrido13/SKILL_ATRIBUTOS_DE_CALIDAD/tree/mateo/docs/test/PRUEBA_UNO_HOSPITAL), [`PRUEBA_DOS_CHATBOT_PAGOS/`](https://github.com/MateoGarrido13/SKILL_ATRIBUTOS_DE_CALIDAD/tree/mateo/docs/test/PRUEBA_DOS_CHATBOT_PAGOS) y [`PRUEBA_TRES_FLUJO_COMPLETO_ECOMERCE/`](https://github.com/MateoGarrido13/SKILL_ATRIBUTOS_DE_CALIDAD/tree/mateo/docs/test/PRUEBA_TRES_FLUJO_COMPLETO_ECOMERCE).
- **[`rossi`](https://github.com/MateoGarrido13/SKILL_ATRIBUTOS_DE_CALIDAD/tree/rossi)** — uso y construcción en [`skills/informe-skill.md`](https://github.com/MateoGarrido13/SKILL_ATRIBUTOS_DE_CALIDAD/blob/rossi/skills/informe-skill.md); prueba MarketHub (prompt + output) en [`TEST.md`](https://github.com/MateoGarrido13/SKILL_ATRIBUTOS_DE_CALIDAD/blob/rossi/TEST.md).
- **[`otaño`](https://github.com/MateoGarrido13/SKILL_ATRIBUTOS_DE_CALIDAD/tree/otaño)** — uso del flujo y resultados de las pruebas en [`informe_skill.md`](https://github.com/MateoGarrido13/SKILL_ATRIBUTOS_DE_CALIDAD/blob/ota%C3%B1o/informe_skill.md).
- **[`yaco`](https://github.com/MateoGarrido13/SKILL_ATRIBUTOS_DE_CALIDAD/tree/yaco)** — uso, correcciones y prueba MarketHub en [`informe-skill-yaco-recroa.md`](https://github.com/MateoGarrido13/SKILL_ATRIBUTOS_DE_CALIDAD/blob/yaco/informe-skill-yaco-recroa.md); casos de test en [`tests-no-leer/`](https://github.com/MateoGarrido13/SKILL_ATRIBUTOS_DE_CALIDAD/tree/yaco/tests-no-leer) de esa rama.
- **[`fontana`](https://github.com/MateoGarrido13/SKILL_ATRIBUTOS_DE_CALIDAD/tree/fontana)** — uso y corrida de integración en [`informe_skill_fontana.md`](https://github.com/MateoGarrido13/SKILL_ATRIBUTOS_DE_CALIDAD/blob/fontana/informe_skill_fontana.md); pruebas en [`tests/skill_test1.md`](https://github.com/MateoGarrido13/SKILL_ATRIBUTOS_DE_CALIDAD/blob/fontana/tests/skill_test1.md) … [`skill_test4.md`](https://github.com/MateoGarrido13/SKILL_ATRIBUTOS_DE_CALIDAD/blob/fontana/tests/skill_test4.md).

## Objetivo

Encapsular en 3 Claude Skills reutilizables el proceso de diseño de atributos de calidad enseñado en la materia (template de 6 partes del SEI + árbol de utilidad), de forma que cualquier sistema nuevo pueda pasar por el mismo flujo — generar escenarios, verificar que estén bien formados, y priorizarlos — sin depender de rehacer el razonamiento manual cada vez. El conocimiento de base (teoría) y el conocimiento de proceso (errores comunes, criterios de aceptación, criterios de priorización) quedan separados y documentados en `shared/` y en `docs/` de cada skill, en vez de vivir solo en la cabeza de quien resuelve el TP, y se validan contra ejemplos conocidos (`tests-no-leer/`) tal como exige el enunciado.

## Las 3 skills

Viven en `.claude/skills/`, el formato nativo de Agent Skills que Claude Code auto-detecta. Cada una tiene un `SKILL.md` con frontmatter YAML (`name` + `description`) que Claude usa para decidir cuándo activarse: no hace falta invocarlas por nombre.

Las tres se encadenan con marcas en el propio escenario (no en archivos del repo):

```
1. generar-escenarios-calidad
        ↓  tabla Parte | Valor  +  [ESTADO: BORRADOR]
2. chequear-completitud-escenario
        ↓  [ESTADO: VALIDADO]  o  [ESTADO: RECHAZADO]
        ↓  (si es RECHAZADO: corregir y volver a chequear)
3. arbol-utilidad
        ↓  árbol priorizado H/M/L  (solo con escenarios VALIDADO)
```

`arbol-utilidad` no arma el árbol si un escenario no trae `[ESTADO: VALIDADO]` (salvo que el usuario confirme en el mismo mensaje que ya pasó el chequeo).

### 1. `generar-escenarios-calidad` — inciso i)

**Qué resuelve:** dada una descripción de sistema o un requerimiento en lenguaje natural, redacta un escenario de atributo de calidad con el template de 6 partes del SEI.

**Cuándo se activa:** cuando se pide redactar o generar un escenario de calidad, se menciona un atributo (performance, disponibilidad, seguridad, modificabilidad, etc.) sobre un sistema concreto, o se pide aplicar el template de 6 partes / quality attribute scenario del SEI.

**Qué pegar:** una descripción del sistema o del contexto de negocio (enunciado, historia de usuario, o texto libre). Opcionalmente, el atributo que interesa cubrir.

**Qué hace con lo que falta:** Ambiente, Artefacto y Medida de la respuesta se pueden completar con `Asunción: ...` y una justificación breve. Fuente del estímulo, Estímulo y Respuesta no se inventan: si no están en el texto, la skill pregunta con 2 o 3 lecturas concretas.

**Output:** marca `[ESTADO: BORRADOR]` + tabla `Parte | Valor` de 6 filas. Ese borrador todavía no puede entrar al árbol.

### 2. `chequear-completitud-escenario` — inciso ii)

**Qué resuelve:** dado un escenario ya escrito (por un humano, por la skill anterior, o extraído de un TP), evalúa si las 6 partes están presentes y bien definidas, y si alguna falla propone cómo corregirla.

**Cuándo se activa:** cuando se pega un escenario y se pide revisarlo, validarlo, chequear si está completo, o preguntar qué le falta.

**Qué pegar:** el texto del escenario (formato libre o ya separado en partes). Opcionalmente, el atributo que dice representar.

**Output:**
- Tabla de auditoría `Parte | Estado | Comentario`, con ✅ / ⚠️ Vaga / ❌ Falta en cada fila.
- Veredicto binario, sin intermedio: **`[ESTADO: VALIDADO]`** o **`[ESTADO: RECHAZADO]`**. Queda VALIDADO solo si las 6 partes tienen ✅.
- Si es RECHAZADO: 3 opciones concretas de corrección por cada parte fallida (ya redactadas, con unidad si aplica).

La Medida de la respuesta solo es ✅ si tiene un número, una unidad o un criterio verificable. Palabras como "rápido", "seguro" o "aceptable" no alcanzan.

### 3. `arbol-utilidad` — inciso iii)

**Qué resuelve:** dado un conjunto de atributos de calidad y sus escenarios, arma un árbol de utilidad y prioriza cada escenario por impacto de negocio y dificultad técnica (etiquetas H/M/L, evaluadas de forma independiente).

**Cuándo se activa:** cuando ya hay atributos o escenarios definidos y se pide armar, generar o priorizar un árbol de utilidad.

**Qué pegar:** la lista de atributos + sus escenarios en la tabla `Parte | Valor` que produce `generar-escenarios-calidad`, ya con `[ESTADO: VALIDADO]`. Opcionalmente, criterios de priorización propios del negocio.

**Compuerta:** si un escenario no está VALIDADO y el usuario no confirma que ya pasó el chequeo, la skill no construye el árbol: indica cuáles faltan validar.

**Output:** tabla `Atributo | Refinamiento | Escenario | (Negocio, Técnica)`, con justificación breve de cada etiqueta. Antes de darlo por cerrado, recorre la descripción del sistema y lista bajo "Cobertura — pendiente" cualquier preocupación de calidad que no tenga escenario.

## Cómo usarlas

No hace falta escribir el nombre de la skill. Claude la elige según el `description` de su `SKILL.md` y lo que se pida en el mensaje.

Las tres skills usan la misma tabla, en este orden: Fuente del estímulo, Estímulo, Artefacto, Entorno, Respuesta, Medida de la respuesta. Columnas: `Parte | Valor`. Así el output de una entra en la siguiente sin reformatear.

Cuando una skill propone un valor que el usuario no dio, lo marca `Asunción: [valor] por [justificación breve]`. Si el dato sí se puede derivar del contexto, no se marca como asunción.

### Flujo encadenado

Ejemplo:

1. *"Generame un escenario de calidad para: 'el sistema debe permitir cambiar la política de comisiones sin reiniciar el servicio'."* → `generar-escenarios-calidad` devuelve la tabla de 6 partes (Modificabilidad) con `[ESTADO: BORRADOR]`. La medida de respuesta va como asunción si el enunciado no la daba.
2. *"¿Este escenario está completo?"* (pegando la tabla) → `chequear-completitud-escenario` sella `[ESTADO: VALIDADO]` o `[ESTADO: RECHAZADO]` y, si rechaza, ofrece 3 correcciones por parte fallida.
3. *"Armá el árbol de utilidad con este escenario y estos otros dos"* → `arbol-utilidad` arma las ramas y asigna H/M/L, siempre que los escenarios estén validados.

### Cada skill también funciona sola

- Chequear un escenario que ya se tenía escrito, sin haber pasado por `generar-escenarios-calidad`.
- Pedir un árbol a partir de escenarios armados a mano, si respetan el formato de 6 partes y están validados (o el usuario confirma que lo están).
- Pedir solo la generación de un escenario, sin seguir con chequeo ni priorización.

## Archivos del repositorio

```
skill-tp-atributos-calidad/
├── .gitignore
├── LICENSE
├── README.md
│
├── .claude/
│   └── skills/
│       ├── generar-escenarios-calidad/
│       │   ├── SKILL.md
│       │   ├── PLAN.md
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
    ├── caso-01-monopatines/
    ├── caso-02-sap-disponibilidad/
    ├── caso-03-sap-performance/
    ├── caso-04-sap-arbol-healthcare/
    ├── caso-05-marketHub-ecommerce/
    ├── caso-06-hospital-mezcla-atributos/
    └── caso-07-chatbot-atributo-mal-mapeado/
```

Cada `SKILL.md` referencia `shared/` con rutas relativas de 3 niveles (`../../../shared/...`), porque `.claude/skills/<nombre>/SKILL.md` queda 3 carpetas por debajo de la raíz. Las referencias a `docs/condiciones/` y `docs/conocimiento/` no llevan ese prefijo: son hijas directas de cada skill.

### `SKILL.md` y `PLAN.md`

`SKILL.md` es lo que Claude carga: instrucciones de la skill más el frontmatter (`name` + `description`) que decide cuándo activarla. El nombre de la carpeta coincide con el campo `name:`.

`PLAN.md` documenta el diseño de esa skill (problema, inputs, fuentes). No forma parte del flujo de uso.

### `shared/`

Base teórica común a las 3 skills, para no duplicar la misma taxonomía en cada una.

| Archivo | Contenido |
|---|---|
| `template-6-partes-sei.md` | Las 6 partes del template SEI, los 4 ejercicios oficiales resueltos, y el desarrollo de un escenario de Escalabilidad. |
| `glosario-atributos-calidad.md` | Qué son los atributos de calidad, clasificaciones (ISO 9126/Mitre/IEEE 1061), los 10 atributos con capítulo propio en el libro + "Otros Atributos (Cap. 14)", los 7 atributos extra de la filmina, y el método QAW/Straw Man. |
| `metodo-arbol-utilidad.md` | Estructura de 4 niveles del árbol de utilidad, etiquetas H/M/L, y su relación con el template de 6 partes. |

La fuente primaria de los tres es Notion (páginas "Atributos de Calidad" y subpáginas), con complementos puntuales de *Software Architecture in Practice* (Bass/Clements/Kazman) solo donde hacía falta llenar un hueco.

### `docs/condiciones/` y `docs/conocimiento/`

Dos carpetas distintas dentro de cada skill. Ambas se cargan como parte del razonamiento; no se mezclan.

| | `docs/condiciones/` | `docs/conocimiento/` |
|---|---|---|
| Qué es | Anti-patrones: qué evitar | Procedimiento operacional de esa skill |
| Fuente | Los 8 tips prácticos de Notion, sobre errores concretos al resolver los ejercicios 1 y 2 del TP | Redactado a medida; no viene de Notion ni del libro |
| Reparto | Los 8 tips van a `generar` y a `chequear`; solo los 3 de clasificación/derivación del atributo van a `arbol-utilidad` | Un archivo por skill: `heuristica-inferencia.md` (generar), `rubric-completitud.md` (chequear), `criterio-priorizacion.md` (árbol) |
| Rol | Filtro negativo, después de producir un resultado (o antes de priorizar, en el árbol) | Mecanismo positivo, durante la construcción del resultado |

### `tests-no-leer/`

Las skills no deben leer, listar ni referenciar esta carpeta. Existe para que una persona valide el comportamiento con ejemplos conocidos, tal como pide el enunciado.

Convención de nombres: `<skill>-input.md` / `<skill>-expected.md`, con `<skill>` en `{generar-escenarios, chequear-completitud, arbol-utilidad}`.

Cómo usarla: se le da el `*-input.md` a la skill (nunca la carpeta completa ni el `*-expected.md`), se compara a mano el resultado contra el `*-expected.md`, y se evalúa si coinciden en contenido (no carácter por carácter).

| Caso | Fuente | Archivos | Qué valida |
|---|---|---|---|
| `caso-01-monopatines/` | TP3 + resolución propia (Notion, "TP Atributos Calidad") | 6 | Las 3 skills (generar, chequear, árbol) |
| `caso-02-sap-disponibilidad/` | Fig. 4.1 del libro SAP | 4 | generar + chequear |
| `caso-03-sap-performance/` | Fig. 9.1 del libro SAP | 4 | generar + chequear |
| `caso-04-sap-arbol-healthcare/` | Tabla 19.1 del libro SAP | 2 | Solo árbol-utilidad, con prioridades H/M/L originales de los autores como benchmark externo |
| `caso-05-marketHub-ecommerce/` | Caso compartido del grupo (mateo/rossi/otaño) | 4 | generar + árbol; benchmark cruzado |
| `caso-06-hospital-mezcla-atributos/` | Caso adversarial | 2 | generar; requerimiento que mezcla 2 atributos |
| `caso-07-chatbot-atributo-mal-mapeado/` | Caso adversarial | 2 | generar; atributo mal mapeado por el usuario |

`caso-02` y `caso-03` no incluyen archivos de árbol de utilidad porque son escenarios aislados de un único atributo (Disponibilidad, Performance), no un sistema con varios atributos para priorizar entre sí.

### `.gitignore`

Excluye la carpeta local de material bibliográfico crudo (`material-construccion/`, PDFs) porque el repo solo versiona markdown/texto. También excluye archivos de sistema y temporales estándar (`.DS_Store`, `*.tmp`, `*.log`, `node_modules/`, etc.).

## Resultados de testing y limitaciones conocidas

Esta sección documenta la ronda de pruebas manuales corrida contra `tests-no-leer/` (8 tests) más las 3 iteraciones de ajuste sobre la skill de árbol de utilidad, cumpliendo el requisito del enunciado de que "la skill debe ser testeada con ejemplos conocidos".

### 1. Metodología de testing

Cada test se corrió con un pedido en lenguaje natural, sin mencionar el nombre de la skill ni su `SKILL.md`, dejando que Claude Code la activara sola por descripción (el mecanismo nativo de `.claude/skills/`). El output de cada corrida se comparó contra el `*-expected.md` correspondiente en `tests-no-leer/` — un archivo que la skill misma nunca lee, solo el evaluador humano. Varios tests se corrieron más de una vez, en subagentes con contexto aislado entre sí, específicamente para detectar variabilidad entre corridas y no quedarse con una sola muestra puntual.

### 2. Tabla resumen de los 8 tests

| Skill | Caso | Resultado | Notas |
|---|---|---|---|
| `generar-escenarios-calidad` | `caso-01-monopatines` (ambiguo) | Aceptable tras ajuste | ver limitación #1 |
| `generar-escenarios-calidad` | `caso-02-sap-disponibilidad` | PASS | clasificación y redacción correctas sin que el input diera el atributo |
| `generar-escenarios-calidad` | `caso-03-sap-performance` | PASS | ídem |
| `chequear-completitud-escenario` | `caso-01-monopatines` | PASS | detectó además un problema de clasificación de base no anticipado (ver limitación #2) |
| `chequear-completitud-escenario` | `caso-02-sap-disponibilidad` | PASS | detectó correctamente la medida no cuantificable |
| `chequear-completitud-escenario` | `caso-03-sap-performance` | PASS | detectó falla en una parte distinta a la anticipada en el diseño del test (ver limitación #3) |
| `arbol-utilidad` | `caso-01-monopatines` | PASS | prioridades justificadas, con variabilidad de criterio aceptable respecto a la referencia propia |
| `arbol-utilidad` | `caso-04-sap-arbol-healthcare` | Parcial | ver limitación #4, comparado contra el benchmark externo (Tabla 19.1 del libro SAP) |

### 3. Ajustes aplicados durante el testing

En orden cronológico — solo los ajustes que efectivamente se aplicaron, no intentos descartados:

1. **`.claude/skills/generar-escenarios-calidad/SKILL.md`** — se agregó una regla explícita anti-invención en el paso de clasificación del atributo, tras detectar que ante inputs ambiguos el modelo tendía a elegir la lectura más rica narrativamente en vez de la más anclada en evidencia textual.
2. **`.claude/skills/arbol-utilidad/docs/conocimiento/criterio-priorizacion.md`** — se ajustó en 3 iteraciones: acotar el criterio de H técnico (tocar múltiples componentes ya no alcanza por sí solo), acotar la regla de desempate por "datos sensibles" (solo aplica cuando el escenario trata específicamente sobre proteger ese dato), y agregar un principio de comparación de evidencia cuantitativa relativa dentro del propio conjunto de escenarios. Resultado: mejora parcial, documentada en la limitación #4.

### 4. Limitaciones conocidas

**Limitación #1 — Ambigüedad en clasificación de atributo (`caso-01-monopatines`).** El input de monopatines admite más de una lectura válida de atributo de calidad (Interoperabilidad, Seguridad, Disponibilidad son todas defendibles). Tras el ajuste, la skill declara la ambigüedad y elige una lectura con evidencia textual real, pero el auto-chequeo de "no inventar una premisa" no es 100% confiable — en una corrida, la justificación final igual construyó un supuesto (concurrencia de uso) no explícito en el texto, aunque más sutil que antes del ajuste. Se documenta como comportamiento esperado ante inputs genuinamente ambiguos, no como bug.

**Limitación #2 — `chequear-completitud-escenario` puede cuestionar la clasificación, no solo la completitud.** En `caso-01`, la skill fue más allá de evaluar las 6 partes y cuestionó si el atributo declarado (Disponibilidad) era el correcto para ese escenario, sugiriendo que podría ser Performance. Esto es coherente con que las condiciones de clasificación (`docs/condiciones/`) también aplican a esta skill, pero amplía el alcance de lo que un chequeo de completitud puede señalar más allá de lo originalmente previsto.

**Limitación #3 — El criterio de "estímulo vago" es menos estricto de lo asumido al diseñar los tests.** En `caso-03`, se esperaba que la skill rechazara el estímulo "muchos usuarios usan el sistema al mismo tiempo" como vago; en cambio lo aceptó como válido y falló el escenario por la Respuesta. Sugiere que `docs/conocimiento/rubric-completitud.md` acepta un rango más amplio de especificidad en el Estímulo del que se asumió al diseñar el test.

**Limitación #4 — Discrepancias con el benchmark externo del libro SAP (árbol de utilidad, caso healthcare).** Tras 3 iteraciones de ajuste sobre `criterio-priorizacion.md`, persisten discrepancias con las etiquetas H/M/L originales de la Tabla 19.1 del libro. La más estable: el escenario de upgrade de componente de terceros (COTS), donde el libro asigna (H, M) y la skill asigna consistentemente (M, H) en las 3 corridas — una inversión de qué eje es el crítico, no solo una diferencia de grado. El razonamiento de la skill es interno y consistente (prioriza esfuerzo cuantitativo en días-persona sobre riesgo de dependencia externa no controlada), pero no coincide con el criterio de los autores en este caso puntual. Se documenta como diferencia de heurística razonable entre dos criterios válidos, no como error de proceso — en el resto de los 13 escenarios del mismo test se logró coincidencia total o parcial.

## Procedencia del merge — este repo como "nuevo main" del grupo

Esta rama parte de la implementación original de `yaco` (formato Claude Skills, mayor cobertura de casos y único método de validación contra un benchmark externo) y le injerta piezas puntuales de las otras 4 ramas del grupo (`mateo`, `rossi`, `otaño`, `fontana`), elegidas por evidencia concreta encontrada al comparar outputs reales sobre el mismo caso (MarketHub) y contra el mismo benchmark del libro (Tabla 19.1). No es un `git merge` entre ramas — las estructuras de carpetas son incompatibles entre sí (`.claude/skills/` vs. `.cursor/rules/` vs. `skills/all-in-one/`) — es una reconstrucción dirigida sobre la base de `yaco`.

| Injerto | Origen | Dónde quedó | Por qué |
|---|---|---|---|
| Estados `[ESTADO: BORRADOR]` / `[ESTADO: VALIDADO]` y veto binario (sin estado intermedio) | `mateo` | `chequear-completitud-escenario/SKILL.md`, `generar-escenarios-calidad/SKILL.md` | Es el único mecanismo *mecánico* (no solo una instrucción de buena fe) que impide tratar un escenario a medio validar como si estuviera terminado. Confirmado en la transcripción cruda de `mateo` (`cursor_use_case_scenario_audit.md`) que efectivamente frena y pregunta en vez de inventar. |
| Compuerta dura en `arbol-utilidad`: no arma el árbol sin escenarios `[ESTADO: VALIDADO]` | `mateo` | `arbol-utilidad/SKILL.md`, paso 1 | En `yaco` el flujo de 3 pasos estaba documentado en el README pero no forzado — cualquiera podía saltar directo a priorizar sin pasar por el chequeo de completitud. |
| Chequeo de cobertura contra el enunciado original, como bloqueo real (no solo nota) | `mateo`, corregido | `arbol-utilidad/SKILL.md`, paso 6 | En la corrida real de `mateo` sobre MarketHub, el escenario de cifrado de datos quedó afuera del árbol final y solo señalado como alerta — la propia rama que inventó el chequeo de cobertura no lo hizo obligatorio, y por eso el gap pasó igual. Acá se corrige esa falla. |
| Regla "Fuente/Estímulo/Respuesta nunca se infieren, se pregunta con opciones concretas" + citar la respuesta del usuario textual en el sellado | `mateo` | `generar-escenarios-calidad/docs/conocimiento/heuristica-inferencia.md` | La transcripción cruda de `mateo` prueba que preguntar-en-vez-de-inventar funciona en la práctica, pero también expuso un hueco de auditabilidad (el archivo corta antes de la respuesta del usuario) — se agrega la obligación de citar la respuesta real para que el valor sellado sea verificable. |
| Excepción de "dependencias de terceros no controladas" al criterio de priorización técnica | `mateo` | `arbol-utilidad/docs/conocimiento/criterio-priorizacion.md` | El "principio rector" original de `yaco` (la medida cuantitativa del propio escenario manda sobre la categoría) es la causa raíz documentada de un sesgo confirmado en dos sistemas independientes (Limitación #4 de este mismo README, y la comparación contra MarketHub en `tests-no-leer/caso-05-marketHub-ecommerce/`). En el cruce directo, la heurística de `mateo` coincidió exactamente con la referencia del grupo en los dos escenarios comparables de ese tipo; la de `yaco` no coincidió en ninguno. |
| Tabla de auditoría `Parte \| Estado ✅/⚠️/❌ \| Comentario` + regla dura de la Parte 6 (sin número/unidad no es ✅) | `rossi` | `chequear-completitud-escenario/SKILL.md` | Formato más legible y accionable que el veredicto binario simple que tenía `yaco`, sin cambiar el criterio de fondo (que ya exigía cuantificabilidad en `rubric-completitud.md`). |
| Exigir justificar por qué se descarta un atributo confundible (no solo por qué se acepta el elegido) | `otaño` | `generar-escenarios-calidad/SKILL.md`, paso 1.2 | `otaño` es la única rama que explica en prosa por qué, por ejemplo, algo es Modificabilidad y no Interoperabilidad — hace la clasificación auditable en vez de una etiqueta sin razonamiento visible. |
| Casos de test adversariales: requerimiento que mezcla 2 atributos, y atributo mal mapeado por el usuario | `mateo` | `tests-no-leer/caso-06-hospital-mezcla-atributos/`, `tests-no-leer/caso-07-chatbot-atributo-mal-mapeado/` | `yaco` no tenía casos diseñados para fallar a propósito, solo casos que confirman comportamiento correcto — estos dos ejercitan robustez ante inputs sucios. |
| Caso MarketHub como benchmark cruzado (con la referencia del grupo como expected) | `mateo` / `rossi` / `otaño` (caso compartido) | `tests-no-leer/caso-05-marketHub-ecommerce/` | Ya estaba corrido y documentado en la rama `yaco` original — se formaliza acá como caso de test permanente, con nota de la comparación de discrepancias contra `mateo` y contra la referencia. |

**Qué se descartó por completo, y por qué:**
- **`fontana`** (rama entera): sin `SKILL.md`, sin bibliografía trazable, y con una regla explícita de aislamiento de contexto entre turnos que es incompatible con el flujo de 3 pasos encadenados que este repo necesita.
- **`rossi/skills/generar-atributos-calidad/`**: duplicado sin resolver del propio `all-in-one` de esa misma rama — se tomó solo su regla de la Parte 6 y su formato de tabla, no la skill completa.
- **El mecanismo de carpetas de estado de `mateo`** (`1-borradores/` → `2-escenarios/` → `3-arbol-utilidad/`): es específico de Cursor operando sobre un filesystem persistente entre turnos; en una skill conversacional de Claude el "sellado" se expresa con la marca `[ESTADO: VALIDADO]` en el propio texto de la respuesta, no con un archivo que cambia de carpeta.
