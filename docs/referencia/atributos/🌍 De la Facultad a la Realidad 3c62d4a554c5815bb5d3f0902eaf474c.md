# 🌍 De la Facultad a la Realidad

> Apartado personal: acá se van a ir marcando los puntos de contacto entre lo que se enseña en la facultad y cómo se usa (o no) en la industria real — para saber desde dónde sacarle más jugo a cada tema.
> 

# La idea de fondo de esta clase, más allá de los conceptos puntuales

Si hay que resumir en una frase lo que esta clase realmente enseña:

<aside>
💡

**La calidad de un sistema no es un accidente ni una opinión — es algo que se puede (y se debe) negociar, especificar y verificar con la misma disciplina con la que se especifica la funcionalidad.**

</aside>

Todo lo visto en esta clase —desde "no son operacionales" hasta el straw man— es, en el fondo, **una sola idea repetida en distintos niveles**: tomar algo vago, subjetivo y discutible ("el sistema debe ser rápido/seguro/escalable") y convertirlo en algo **concreto, medible y verificable**, sin perder de vista que detrás de ese número hay personas (stakeholders) con intereses en tensión.

Cada pieza de la clase ataca ese mismo problema desde un ángulo distinto:

| **Herramienta** | **Qué problema de "lo vago" resuelve** |
| --- | --- |
| No operacionalidad | Reconocer que el problema existe y que no se resuelve programando directamente |
| Clasificaciones (ISO, Mitre, IEEE) | Darle nombre y vocabulario compartido |
| Árbol de utilidad | Decidir a qué vaguedad atacar primero (priorizar) |
| Template de 6 partes | Convertir la vaguedad en una oración concreta y testeable |
| QAW / Stakeholder Map | Asegurarse de que la especificación refleje lo que la gente real necesita, no lo que el arquitecto supone |
| Straw Man | Reconocer que ni siquiera el número final es 100% cierto al principio, y dar un método honesto para ir ajustándolo |

Es, en el fondo, una clase sobre **cómo lidiar con la incertidumbre y la ambigüedad de forma sistemática**, no sobre memorizar una lista de atributos.

# Qué tan útil es esto en la vida real

<aside>
⚖️

El valor está mucho más en la **forma de pensar** que en el uso literal de cada artefacto. La disciplina mental sí se usa todos los días; el formalismo exacto de la facultad (roles del QAW, template de 6 casilleros escrito tal cual) se ve mucho menos.

</aside>

## Lo que sí se usa tal cual, todos los días en la industria

- **SLA / SLO / SLI** (Service Level Agreement / Objective / Indicator): es literalmente el mismo concepto que "medida de respuesta", pero con otro nombre — es el pan de cada día en cualquier empresa que opere servicios en producción (AWS, Google, cualquier SaaS). Un SLO como *"99.9% de disponibilidad mensual"* es un escenario de calidad ya resuelto, con straw man incluido en las negociaciones de contrato.
- **Non-Functional Requirements (NFRs)** o criterios de aceptación extendidos: en equipos ágiles maduros, las historias de usuario incluyen criterios de aceptación de performance/seguridad, aunque rara vez se llame "template SEI de 6 partes".
- **Architecture Decision Records (ADRs)**: documentan tradeoffs de forma muy similar a lo visto en clase (por qué se eligió X sobre Y, qué atributo se priorizó y cuál se sacrificó).
- **Spikes técnicos** (en Scrum): son exactamente lo que se describió como "prototipo para validar el straw man" — timeboxed, para reducir incertidumbre antes de comprometerse a un número.

## Lo que se usa mucho menos en su forma "académica" completa

- El **QAW formal** (con roles como Electronic Scribe, Parking Lot, sesión de varios días) es realista en organizaciones grandes, sistemas críticos (aeroespacial, salud, banca, gobierno) o consultoría de arquitectura — no en una startup de 5 personas ni en un proyecto universitario típico.
- El **árbol de utilidad** con etiquetas (H,H)/(M,L) formales se ve sobre todo en evaluaciones de arquitectura estructuradas como **ATAM** (Architecture Tradeoff Analysis Method, el "hermano mayor" del QAW, también del SEI) — común en consultoría de arquitectura senior, menos en el día a día de un equipo de desarrollo promedio.
- El **template de 6 partes escrito literalmente** (con esas 6 etiquetas explícitas) es raro verlo documentado así en una empresa real — lo que sí persiste es **el razonamiento**: casi ningún ingeniero senior escribe "Fuente del estímulo: usuarios", pero sí piensa automáticamente "¿quién dispara esto, en qué condición, y cómo lo mido" cuando evalúa un requerimiento no funcional.

## Por qué existe esta brecha (un dato de origen honesto)

Esta metodología viene de raíz del **SEI** (Software Engineering Institute, Carnegie Mellon) — una institución con fuerte anclaje en sistemas de **defensa, gobierno y misión crítica** de EE. UU. Gran parte de ella (ATAM, QAW) nació y se aplica más rigurosamente en ese mundo (sistemas militares, aeroespaciales, infraestructura crítica), donde un error de arquitectura puede costar vidas o millones de dólares, y por eso se justifica el costo de un workshop de varios días con roles formales.

En el mundo del desarrollo de software "de a pie" (apps, SaaS, e-commerce), la disciplina se adopta de forma **mucho más informal y liviana** — más parecida a la versión "ágil" vista en clase (Mini-QAW, entrevistas cortas, straw man rápido) que a la versión completa del SEI. La mayoría de los equipos ágiles hacen algo parecido a un Mini-QAW sin saber que tiene ese nombre: alguien pregunta "¿qué tan rápido tiene que ser esto?" en una reunión de refinamiento de backlog, y ya está aplicando el espíritu del método.

# Conclusión: de dónde sacarle más jugo a esto

<aside>
🎓

El valor real de esta clase **no es llenar literalmente un template de 6 partes en el primer trabajo** (probablemente eso no pase, salvo que se trabaje en sistemas críticos o consultoría de arquitectura). El valor real es que, de acá en más, cuando alguien diga *"el sistema tiene que ser rápido"* o *"tiene que escalar bien"*, va a estar el reflejo automático de preguntar: ¿rápido comparado con qué? ¿en qué condición? ¿cómo lo medimos? ¿quién lo necesita y por qué? — la disciplina mental que separa diseñar de forma intuitiva de diseñar de forma rigurosa y defendible ante otros.

</aside>

---

# 🔗 Facultad vs. Realidad — bitácora personal

<aside>
📝

Espacio para ir agregando, a medida que aparezcan en el trabajo/pasantías/lecturas, los puntos de contacto concretos entre lo enseñado acá y su equivalente (o ausencia) en la industria real.

</aside>

| **Concepto de la facu** | **Equivalente / uso real observado** | **Notas** |
| --- | --- | --- |
| Escenario de calidad (template 6 partes) | SLA / SLO / SLI | Mismo concepto, distinto nombre y sin las 6 etiquetas explícitas |
| Response Measure Straw Man | Spikes técnicos en Scrum | Prototipo acotado para reducir incertidumbre antes de comprometerse a un número |
| QAW completo (SEI) | ATAM, consultoría de arquitectura | Poco común fuera de sistemas críticos o empresas grandes |
| Árbol de utilidad | Priorización de backlog / criterios de aceptación | La lógica de priorizar H/M/L se aplica informalmente en refinamiento |
|  |  |  |