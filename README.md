# Skill: `sei-quality-attribute-scenarios`

## Objetivo del trabajo

La consigna pide desarrollar una o más *skills* (estilo Claude) que, en base a experiencias previas y al material bibliográfico disponible sobre el método SEI (Software Engineering Institute) / ATAM, permitan:

1. **Generar** atributos de calidad de acuerdo con la plantilla de 6 partes del SEI.
2. **Chequear** si un escenario dado está completo y, si no lo está, proponer cómo completarlo.
3. **Elaborar un árbol de utilidad** (utility tree) para priorizar los escenarios.

Además, la consigna exige que la skill sea **testeada con ejemplos conocidos** (casos clásicos de la literatura del SEI), de modo que su comportamiento pueda calibrarse y verificarse contra un estándar reconocido.

Para resolver esto se construyó una única skill, `sei-quality-attribute-scenarios`, que cubre las tres tareas dentro de un mismo flujo de trabajo conceptual (todas giran en torno al mismo artefacto: el escenario de 6 partes), en lugar de fragmentarlas en skills separadas que deberían compartir la misma plantilla y el mismo catálogo de todos modos.

## ¿Qué es una skill de Claude?

Una skill es una carpeta con instrucciones reutilizables (y, opcionalmente, archivos de referencia) que Claude consulta cuando detecta que la tarea del usuario coincide con lo descrito en su `description`. No es código que se ejecuta: es una guía estructurada que Claude sigue como si fuera un experto en la materia, incluyendo criterios de calidad, formatos de salida y ejemplos de calibración.

## Estructura de archivos

```
sei-quality-attribute-scenarios/
├── SKILL.md                                  # Instrucciones principales (siempre se carga)
└── references/
    ├── quality-attributes-catalog.md         # Catálogo de atributos con preguntas y escenarios generales
    └── examples.md                           # Casos resueltos para calibrar estilo y testear la skill
```

- **`SKILL.md`**: contiene el frontmatter (`name` y `description`, usados para decidir cuándo activar la skill) y el cuerpo con las instrucciones de las tres tareas.
- **`references/quality-attributes-catalog.md`**: se consulta *bajo demanda* (no se carga siempre) al generar escenarios nuevos, para no partir de cero por cada atributo de calidad.
- **`references/examples.md`**: contiene ejemplos completos resueltos, usados tanto como referencia de estilo para Claude como para el testeo funcional de la skill (ver sección de pruebas).

Este diseño en capas (instrucciones core + referencias consultables) evita que el prompt principal crezca demasiado y le permite a Claude "ir a buscar" detalle solo cuando la tarea lo requiere.

## Cuándo se activa la skill

La `description` del frontmatter dispara la skill cuando el usuario menciona explícitamente términos como "atributo de calidad", "escenario de calidad", "ATAM", "QAW", "árbol de utilidad" o "requisitos no funcionales", **o** cuando describe un sistema y pide identificar, revisar, completar o priorizar sus atributos de calidad (rendimiento, disponibilidad, seguridad, modificabilidad, usabilidad, testabilidad, interoperabilidad, etc.) aunque no use esos términos exactos. Esto último es importante porque muchos usuarios describen el problema en lenguaje natural ("necesito que el sistema aguante mucha carga en Black Friday") sin conocer la terminología SEI.

## Funcionamiento: las tres tareas

### 1. Generar escenarios nuevos

La plantilla central que atraviesa toda la skill es la de **6 partes** del SEI:

| # | Parte | Pregunta que responde |
|---|-------|------------------------|
| 1 | Fuente del estímulo | ¿Quién o qué genera el estímulo? |
| 2 | Estímulo | ¿Qué condición o evento dispara la respuesta? |
| 3 | Artefacto | ¿Qué parte del sistema recibe el estímulo? |
| 4 | Entorno | ¿En qué condiciones ocurre? |
| 5 | Respuesta | ¿Qué debe hacer el sistema? |
| 6 | Medida de la respuesta | ¿Cómo se verifica, con un número o criterio concreto, que la respuesta fue satisfactoria? |

Regla práctica incorporada en la skill: **si la parte 6 no tiene un número, unidad o criterio verificable, el escenario no está completo**, sin importar qué tan bien redactadas estén las otras cinco partes.

La skill distingue entre:
- **Escenario general**: independiente del sistema, sirve como plantilla para cualquier sistema del mismo dominio de atributo.
- **Escenario concreto**: instancia específica, con las 6 partes llenas con detalles reales del sistema del usuario.

Flujo: se identifican los atributos relevantes → se consulta el catálogo de referencia por cada atributo → se redacta cada escenario como tabla de 6 filas → cualquier número de negocio no provisto por el usuario se marca explícitamente como *supuesto a validar*, en vez de inventarse silenciosamente.

### 2. Auditar si un escenario está completo

Cuando el usuario entrega un escenario (en prosa, parcial o ya en tabla), la skill:

1. **Mapea** lo dado a las 6 partes, separando explícitamente frases que mezclan varias partes en una sola oración.
2. **Evalúa cada parte** con dos criterios: **presencia** (¿se mencionó?) y **concreción** (¿es específica y verificable, o es vaga como "rápido" o "el sistema" en general?).
3. Presenta una **tabla de auditoría** con estado (✅ / ⚠️ Vaga / ❌ Falta) y comentario por cada parte.
4. Para cada parte incompleta, propone cómo completarla: si se puede inferir razonablemente del contexto ya dado, la completa y lo dice explícitamente; si la información depende de un dato que solo el usuario tiene (un SLA, un umbral de riesgo), no lo inventa — pregunta puntualmente o entrega opciones plausibles para elegir.
5. Entrega al final el **escenario completado** como tabla limpia de 6 filas, lista para usar.

### 3. Construir un árbol de utilidad

El árbol organiza los escenarios en una jerarquía de tres niveles para priorizarlos frente a stakeholders:

```
Utilidad (raíz)
├── Atributo de calidad (rama de primer nivel)
│   └── Preocupación específica (sub-rama)
│       └── Escenario concreto, etiquetado (Importancia, Dificultad)
```

Los escenarios etiquetados **(Alta, Alta)** son los candidatos prioritarios para el análisis de arquitectura, por combinar mayor impacto de negocio con mayor riesgo técnico.

Por defecto el árbol se entrega como lista anidada en markdown; si el usuario pide un diagrama visual, la skill indica usar las herramientas de visualización disponibles en la conversación en lugar de "dibujarlo" a mano en texto. Si se pide una tabla de priorización, se complementa con una tabla ordenada por prioridad.

### Entrega en Word

Si el usuario pide el resultado como documento descargable (.docx), la skill delega explícitamente en la skill `docx` en lugar de reimplementar el manejo de archivos Word, manteniendo las mismas tablas y estructura que se usarían en el chat.

## Testeo con ejemplos conocidos

Tal como exige la consigna, la skill fue pensada para poder testearse contra casos clásicos de la literatura SEI (p. ej. *Software Architecture in Practice*, Bass/Clements/Kazman). Para esto, `references/examples.md` incluye tres casos resueltos que sirven como *ground truth*:

1. **Disponibilidad — e-commerce**: un escenario concreto completo de las 6 partes (falla del nodo de base de datos → conmutación automática), usado para validar que la skill produce escenarios con medidas verificables (ej. "conmutación ≤ 30 segundos, disponibilidad mensual ≥ 99.95%").
2. **Auditoría de un escenario incompleto — simulador de vuelo**: un escenario vago ("cuando el sistema recibe muchos datos de sensores, debe seguir funcionando bien") con su tabla de auditoría completa y el escenario ya corregido, usado para validar que la skill detecta correctamente qué partes faltan o son vagas y las completa marcando los supuestos.
3. **Fragmento de árbol de utilidad**: combina los dos escenarios anteriores más dos adicionales (modificabilidad y seguridad), usado para validar el formato de salida del árbol y el criterio de priorización (A, A).

Para testear la skill se recomienda:
- Pedirle a Claude que genere escenarios para un sistema conocido y comparar el resultado contra el Ejemplo 1 (¿usa la misma estructura de 6 partes? ¿la medida de respuesta es verificable?).
- Darle el escenario vago del simulador de vuelo (Ejemplo 2) tal cual y verificar que la tabla de auditoría que produce coincide en diagnóstico con la de referencia, y que el escenario completado es coherente.
- Pedir un árbol de utilidad con los escenarios de los ejemplos y verificar que la jerarquía y las etiquetas (Importancia, Dificultad) se arman según el formato del Ejemplo 3.

Este enfoque permite verificar la skill de forma reproducible sin depender de un motor de evaluación externo: los propios ejemplos incluidos en la skill actúan como casos de prueba.
