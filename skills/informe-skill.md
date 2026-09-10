# Skill de Claude para Atributos de Calidad (SEI)

**TP3 — Diseño de Software · Ejercicio 4 · Informe técnico de construcción y validación**

| | |
|---|---|
| **Tipo de skill** | Agent Skill nativa de Claude (formato `SKILL.md` con carpeta `references/`, activación automática por palabras clave del dominio — "atributo de calidad", "ATAM", "QAW", "árbol de utilidad", "requisitos no funcionales", entre otras) |
| **Skills implementadas** | Una única skill, `sei-quality-attribute-scenarios`, que resuelve las tres tareas del enunciado como **tareas internas de un mismo flujo**, no como skills separadas encadenadas |
| **Alcance** | Corresponde a los tres incisos del ejercicio 4: (i) generar escenarios con el template de 6 partes del SEI, (ii) auditar si un escenario dado está completo y, si no, cómo completarlo, y (iii) construir el árbol de utilidad una vez que hay escenarios e importancia/dificultad asignadas |

---

## 1. Introducción

A diferencia de un pipeline de varias skills que se pasan archivos entre carpetas, esta implementación resuelve las tres tareas del ejercicio dentro de **una sola skill de Claude**. El `SKILL.md` describe el criterio de activación, la plantilla de 6 partes y el procedimiento de cada una de las tres tareas; una carpeta `references/` aporta el conocimiento de dominio (catálogo de atributos de calidad y ejemplos resueltos) que Claude consulta antes de redactar, en vez de generar escenarios genéricos de memoria.

El orden de trabajo que propone la skill es el mismo que exige el método SEI: primero se especifica el escenario (general y luego concreto), después se audita su completitud, y solo al final —con Importancia y Dificultad ya asignadas para cada hoja— se arma el árbol de utilidad. No hay, sin embargo, una compuerta de archivos que impida "saltarse" un paso: al ser una sola skill conversacional, el orden se sostiene por instrucción explícita en el texto de la skill, no por control de estado entre carpetas.

---

## 2. Composición y construcción de la skill

La skill vive en un único archivo `SKILL.md` más dos documentos de referencia que actúan como base de conocimiento de solo lectura:

| Componente | Contenido | Rol |
|---|---|---|
| `SKILL.md` | Criterio de activación, plantilla de 6 partes, procedimiento de las tres tareas, formato de salida del árbol, punto de entrega en Word | Es la única fuente de instrucciones de comportamiento; no se genera contenido de dominio aquí |
| `references/quality-attributes-catalog.md` | Trece atributos de calidad (Rendimiento, Disponibilidad, Seguridad, Modificabilidad, Usabilidad, Testabilidad, Interoperabilidad, Integrabilidad, Escalabilidad, Portabilidad, Desplegabilidad, Recuperabilidad, Operabilidad, Eficiencia energética), cada uno con preguntas de elicitación y un escenario general de 6 partes ya redactado | Evita que Claude redacte escenarios genéricos o mezcle atributos vecinos (por ejemplo, distingue explícitamente Integrabilidad de Interoperabilidad, dos atributos que suelen confundirse) |
| `references/examples.md` | Cuatro ejemplos resueltos: un escenario de Disponibilidad, una auditoría de un escenario incompleto, un fragmento de árbol de utilidad, y un caso de punta a punta (generación + auditoría + árbol) sobre un sistema de monopatines eléctricos | Calibra estilo, nivel de detalle y formato esperado; sirve además como conjunto de prueba (sección 3) |

### Decisiones de diseño relevantes

- **Escenario general antes que concreto.** Al pedir "generar escenarios", la skill redacta primero la versión general (independiente del sistema, ancla el atributo) y luego la concreta, con datos reales del sistema del usuario. Al pedir "revisar/completar", trabaja directamente sobre el concreto que ya trajo el usuario.
- **La medida de la respuesta como criterio de cierre.** La regla explícita de la skill es que, si no se puede poner un número, unidad o criterio verificable en la sexta parte, el escenario no está completo — sin importar qué tan bien redactadas estén las otras cinco.
- **Delegación en la skill de Word.** Si el usuario pide el resultado como `.docx`, la skill no reimplementa el manejo de documentos: delega en la skill de `docx` disponible en el entorno, manteniendo las mismas tablas que usaría en el chat.

---

## 3. Casos de prueba

La skill se validó con los cuatro ejemplos de `references/examples.md`, elegidos para cubrir dominios distintos (e-commerce, aviación, movilidad urbana) y las tres tareas por separado y combinadas.

### Caso A — Generación: Disponibilidad en un sistema de e-commerce

Sistema: plataforma de comercio electrónico con nodo de base de datos primario y réplica secundaria. Se pidió un escenario concreto de Disponibilidad.

| Parte | Contenido |
|---|---|
| Fuente del estímulo | El nodo de base de datos primario |
| Estímulo | Deja de responder por falla de hardware |
| Artefacto | El servicio de catálogo de productos |
| Entorno | Operación normal, horario de tráfico medio |
| Respuesta | El sistema conmuta automáticamente a la réplica secundaria sin intervención manual |
| Medida de la respuesta | Conmutación completa en ≤ 30 segundos, sin pérdida de transacciones confirmadas, disponibilidad mensual ≥ 99.95% |

Resultado: las seis partes quedan concretas y con medida cuantificable en el primer intento, sin marcas de `[INFERIDO]` ni supuestos — el objetivo de este caso era comprobar que, cuando el sistema está bien descripto, la skill no agrega vaguedad de más.

### Caso B — Auditoría: escenario incompleto de un simulador de vuelo

Entrada del usuario, deliberadamente vaga: *"Cuando el sistema recibe muchos datos de sensores, debe seguir funcionando bien."*

| Parte | Estado | Comentario |
|---|---|---|
| Fuente del estímulo | ⚠️ Vaga | No dice de dónde vienen los datos (¿sensores de vuelo, red externa, otro subsistema?) |
| Estímulo | ⚠️ Vaga | "Muchos datos" no es medible — falta volumen o tasa |
| Artefacto | ❌ Falta | No se identifica el componente que recibe los datos |
| Entorno | ❌ Falta | No se dice si ocurre en vuelo normal o en una maniobra crítica |
| Respuesta | ⚠️ Vaga | "Funcionando bien" no es verificable |
| Medida de la respuesta | ❌ Falta | No hay número ni criterio de éxito |

La skill entregó, además de la tabla de auditoría, el escenario completado asumiendo un contexto de simulador militar (tasa de telemetría de 1000 mensajes/segundo, latencia < 50ms, 0% de mensajes descartados), marcando explícitamente la fuente del estímulo como supuesto a confirmar. Este caso es el que más aporta a la sección 4: mostró en la práctica la diferencia entre marcar una parte como "presente" y marcarla como "concreta".

### Caso C — Flujo completo: sistema de alquiler de monopatines eléctricos

Sistema descripto por contexto: app móvil de alquiler de monopatines (activación por QR, cobro por Mercado Pago, pausas de hasta 15 minutos, geolocalización GPS) más una app Web de administración. Se corrieron las tres tareas sobre el mismo sistema:

- **Generación** de tres escenarios concretos: Disponibilidad (finalización de viaje con validación GPS), Rendimiento (activación por QR en hora pico) y Seguridad (anulación de cuentas fraudulentas).
- **Auditoría** de un escenario incompleto de negocio dado en prosa: *"Cuando el usuario pausa el viaje, el sistema debe manejarlo bien y cobrar lo que corresponde."* La skill detectó que el estímulo real no era "pausar" sino "no reanudar dentro de los 15 minutos", que el artefacto no estaba identificado, y que "manejarlo bien" escondía dos respuestas de negocio distintas (apagar sin desasignar vs. reanudar con tarifa extra). Completó el escenario tomando el caso de mayor riesgo (reinicio con tarifa extra), marcado como la interpretación elegida.
- **Árbol de utilidad** con los cinco escenarios resultantes:

```
Utilidad
├── Disponibilidad
│   └── Finalización de viaje con validación GPS            (A, A)
├── Rendimiento
│   └── Activación de monopatín por QR en hora pico          (A, M)
├── Confiabilidad de datos de cobro
│   └── Reinicio automático de tarifa tras pausa extensa     (A, M)
├── Seguridad
│   └── Anulación de cuentas fraudulentas                     (M, B)
└── Modificabilidad
    └── Alta de nuevas tarifas extra sin re-despliegue        (M, B)
```

Este caso funcionó como benchmark de punta a punta: permitió comprobar que un escenario auditado y corregido en la Tarea 2 se puede llevar sin fricción a una hoja del árbol de la Tarea 3, y que la skill es capaz de crear una rama de primer nivel ("Confiabilidad de datos de cobro") que no estaba en el catálogo original cuando el escenario no encaja limpiamente en ninguno de los atributos predefinidos.

---

## 4. Problemas detectados y correcciones aplicadas

La validación consistió en correr la skill con las entradas de la sección 3 y revisar si el resultado cumplía la regla de cierre del SEI (parte 6 cuantificable) y si no se filtraban afirmaciones que el usuario nunca hizo. Se identificaron dos problemas de fondo.

### Problema 1 — La auditoría confundía "está mencionado" con "está completo"

**Comportamiento esperado:** que cada una de las 6 partes se evalúe con dos criterios independientes — presencia (¿se mencionó, aunque sea implícitamente?) y concreción (¿es específica y verificable, o es una palabra vaga como "rápido" o "bien"?).

**Antes de la corrección:** una primera redacción de la Tarea 2 pedía solo verificar si cada parte "estaba presente". Con esa instrucción, un enunciado como el del Caso B tendía a marcar Respuesta como ✅ porque *había* una frase de respuesta ("debe seguir funcionando bien"), y Medida como aceptable si el usuario mencionaba cualquier cosa parecida a un objetivo, aunque no fuera un número. El resultado era una auditoría que decía "completo" sobre un escenario que en realidad no se podía usar para diseñar ni para probar nada, porque ninguna de sus partes vagas daba un criterio de éxito verificable.

**Corrección aplicada:** se reescribió el paso de evaluación de la Tarea 2 exigiendo explícitamente los dos criterios por separado, con la aclaración operativa de qué cuenta como concreto ("rápido" no es una medida de respuesta; "p95 < 300ms" sí lo es; "el sistema" no es un artefacto acotado si el sistema tiene varios componentes). Con esta versión, el Caso B produce la tabla de la sección 3 —cuatro partes marcadas ⚠️ o ❌ en lugar de aceptadas por default— y ninguna de ellas queda cerrada solo porque el usuario escribió algo en esa posición.

### Problema 2 — Números y supuestos de negocio inventados sin marcar

**Comportamiento esperado:** si al completar un escenario falta un número de negocio (SLA, presupuesto, umbral de riesgo) que solo el usuario puede definir, la skill no debe inventarlo como si fuera un dato dado; debe proponerlo explícitamente como supuesto a validar, o preguntar.

**Antes de la corrección:** al generar el escenario de Rendimiento del Caso C, una versión temprana completaba la fila de Entorno con "hora pico, alta concurrencia" y directamente fijaba una cifra de concurrencia (por ejemplo, una tasa fija de activaciones por minuto) presentada igual que el resto de la tabla, sin distinguir visualmente que ese número no salía de la descripción del sistema sino que lo había puesto la skill. El riesgo es que un escenario así se use para dimensionar infraestructura o negociar un SLA como si el umbral ya estuviera acordado con el negocio.

**Corrección aplicada:** se agregó a la Tarea 1 la regla de no inventar números de negocio específicos que el usuario no haya dado, exigiendo en su lugar proponer un valor razonable marcado explícitamente como supuesto. Esto se ve aplicado en los dos escenarios del Caso C que requirieron una cifra no provista por el enunciado: la concurrencia de activaciones en hora pico y la disponibilidad mensual del proveedor de GPS quedaron ambas anotadas como *"(supuesto: ajustar según SLA/dato real)"*, en vez de presentarse como parte fija del escenario. La misma regla se extendió a la Tarea 3: cuando el usuario no asignó Importancia o Dificultad a una hoja del árbol, la skill debe proponer una asignación razonada y marcarla como propuesta a validar, no completar la etiqueta en silencio.

---

## 5. Limitaciones conocidas

- **No hay una regla que fuerce "un escenario, un atributo".** Si el requerimiento que trae el usuario mezcla dos preocupaciones de calidad en una sola frase (por ejemplo, seguridad y rendimiento juntos, como en el clásico caso del sistema de turnos hospitalarios), la skill decide caso a caso si conviene separar en dos escenarios o resolverlos como uno solo con dos aspectos, según el criterio del modelo en el momento — no hay una instrucción explícita en `SKILL.md` que obligue siempre a partir el requerimiento. Esto puede producir escenarios menos comparables entre corridas distintas frente al mismo enunciado ambiguo.

- **La priorización del árbol no tiene una compuerta dura.** La Tarea 3 permite avanzar y dibujar el árbol proponiendo Importancia/Dificultad razonadas cuando el usuario no las dio, en lugar de bloquear la construcción hasta que el stakeholder responda. Esto agiliza el flujo conversacional, pero también significa que un árbol puede mostrarse como si estuviera listo para priorizar cuando en realidad las etiquetas H/M/L de alguna hoja son una propuesta de la skill todavía sin validar por el dueño del sistema — el usuario tiene que leer la marca de "supuesto" para notar la diferencia.

- **El catálogo de referencia no es exhaustivo y lo dice explícitamente.** Cubre trece atributos de calidad de uso general; un requisito de dominio específico (por ejemplo, "seguridad del paciente" en salud, o un atributo regulatorio propio de una industria) no tiene ficha propia y depende de que la skill lo construya ad hoc con la misma plantilla de 6 partes, sin la ayuda de preguntas de elicitación ya preparadas para ese caso.

---

## 6. Aspectos positivos de este flujo

- **Baja fricción de entrada.** El usuario puede describir el sistema en lenguaje de negocio, sin conocer el método SEI, y recibe de vuelta tanto el escenario general (que ancla el atributo) como el concreto aplicado a su sistema, más una única ronda de preguntas puntuales cuando falta contexto — no un interrogatorio exhaustivo antes de producir algo.

- **Los supuestos quedan a la vista en vez de disolverse en la tabla.** Gracias a la corrección del Problema 2, cualquier número que la skill propuso en lugar del usuario queda marcado inline (`*(supuesto: ...)*`), visible en la misma fila donde se usa, lo que evita que un escenario "de relleno" se lea como si estuviera acordado con el negocio.

- **La auditoría separa dos preguntas que suelen mezclarse.** Al evaluar presencia y concreción por separado (Problema 1), la skill detecta escenarios que parecen completos a simple vista pero no sirven para diseñar ni para probar nada — el caso más claro es una "medida de respuesta" que en realidad es un adjetivo ("rápido", "bien") y no un número.

- **El catálogo de referencia previene errores de clasificación comunes.** Al distinguir explícitamente atributos que suelen confundirse —Integrabilidad de Interoperabilidad, Portabilidad de Modificabilidad, Escalabilidad de Rendimiento— la skill tiene menos tendencia a aplastar un atributo específico bajo el nombre de otro más genérico, algo que sí fue un problema reportado en implementaciones con un catálogo de atributos más reducido.

- **Salida flexible sin perder la estructura.** El árbol de utilidad se entrega por defecto como lista anidada en markdown (rápida de leer en el chat), pero la skill puede ofrecer un diagrama visual con las herramientas de visualización disponibles, una tabla ordenada por prioridad, o el resultado completo en un documento Word delegando en la skill correspondiente — sin tener que reconstruir el formato de la tabla de 6 partes en cada caso.
