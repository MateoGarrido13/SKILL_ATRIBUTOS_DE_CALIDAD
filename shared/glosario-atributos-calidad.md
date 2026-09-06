# Glosario de Atributos de Calidad

> Fuente: página de Notion "Atributos de Calidad" (raíz) y sus subpáginas "Clasificaciones de Atributos de Calidad", "QAW – Quality Attribute Workshop" (con "Dinámicas Ágiles para QAW" y "Straw Man y Estimación de Medidas de Respuesta"), las 10 páginas de atributos con capítulo propio en el libro más "Otros Atributos de Calidad (Cap. 14 SAP)", y las 7 páginas de atributos "extra" mencionados en la filmina. Migrado casi íntegro. Basado en Clase 5 — Diseño de Sistemas de Software (J. Andrés Díaz Pace) y en *Software Architecture in Practice* (4th ed.), Bass/Clements/Kazman.
>
> No se migró la subpágina "De la Facultad a la Realidad" (bitácora personal de reflexión, no es contenido teórico de base que las skills necesiten como conocimiento).

## ¿Qué son los Atributos de Calidad?

Los **atributos de calidad** son propiedades sistémicas de un producto de software, a través de las cuales los stakeholders juzgan la calidad de ese producto. No alcanza con que el sistema implemente la funcionalidad correcta — un sistema puede hacer exactamente lo que tiene que hacer y aun así ser un mal sistema si:

- "Anda lento"
- Permite que atacantes roben datos
- Está caído la mayor parte del tiempo
- No escala cuando crece la demanda
- Es difícil de modificar o integrar con otros sistemas

Cuando el sistema sí tiene bien definidos sus atributos de calidad, estos:

- Hablan sobre la **calidad esperada** del sistema
- Están definidos desde el **punto de vista de los stakeholders**
- Se definen de manera **precisa** (no ambigua)
- Permiten **"testear"** si el requerimiento de calidad se satisface o no

## No son operacionales: la clave para entenderlos

Esta es la diferencia central entre un requerimiento funcional y un atributo de calidad, y por eso conviene detenerse en ella.

Un **requerimiento funcional** es *operacional*: se puede traducir casi directamente en una operación o función del sistema. Por ejemplo, "el sistema debe permitir cancelar un pedido" se traduce en un método como `cancelarPedido()`. Podés señalar una línea de código, una pantalla, un botón, y decir "esto implementa ese requerimiento".

Un **atributo de calidad**, en cambio, *no se puede señalar en una sola parte del sistema* ni traducir en una función concreta. "El sistema debe ser seguro" o "el sistema debe ser rápido" no son cosas que se implementen en un módulo aislado — son **propiedades que emergen de cómo está construido el sistema como un todo**: de la arquitectura, de decisiones de diseño distribuidas en múltiples componentes, de la infraestructura, de cómo interactúan las partes entre sí.

> **Ejemplo comparativo**
> - "El sistema debe autenticar usuarios" → **operacional**: es una función concreta, `login(usuario, password)`.
> - "El sistema debe ser seguro" → **no operacional**: no es una función, es una cualidad que depende de cómo se implementó la autenticación, el cifrado de datos, los logs de auditoría, el manejo de sesiones, la validación de inputs, etc. — todo distribuido por el sistema.

Por no ser operacionales:

1. **No se pueden "codear" directamente** — no existe un método `serRapido()`. La performance surge de decisiones combinadas: el algoritmo elegido, la arquitectura de capas, el uso de cache, la base de datos, etc.
2. **Son transversales (cross-cutting)** — atraviesan todo el sistema en vez de vivir en un solo lugar. Por eso su alcance debe considerarse a lo largo del diseño, implementación y deployment.
3. **Necesitan ser interpretados en contexto** — decir "el sistema debe ser rápido" no significa nada por sí solo. ¿Rápido comparado con qué? ¿En qué condición? ¿Cuánto es "rápido"? Por eso se necesita capturarlos como **escenarios** con una estructura de 6 partes (ver `template-6-partes-sei.md`), que es la forma de tomar algo abstracto y no operacional y volverlo concreto, medible y testeable.

## El panorama de los Requerimientos de Software

Para diseñar un sistema se necesitan cuatro insumos:

- El **contexto** del sistema
- Los **requerimientos funcionales** → capturados como casos de uso o historias de usuario
- Los **requerimientos de atributos de calidad** → capturados como escenarios de atributos de calidad
- Las **restricciones** impuestas por el contexto o por los stakeholders (condiciones no negociables: tecnología obligatoria, infraestructura existente, plataformas soportadas, etc. — ver sección QAW más abajo)

Los atributos de calidad son no operacionales, y por lo tanto deben ser interpretados en contexto.

## ¿Cómo se usan los Atributos de Calidad en la práctica?

- En licitaciones
- Liderando un grupo o equipo
- Cuando se analiza un requerimiento
- Evaluando o eligiendo arquitecturas (o herramientas)
- Complementando metodologías ágiles
- Comunicando decisiones de diseño

## Tradeoffs (puntos de balance)

Los atributos de calidad pueden entrar en conflicto unos con otros. Algunos ejemplos típicos:

- Performance **versus** Seguridad
- Seguridad **versus** Disponibilidad
- Performance **versus** Modificabilidad

El objetivo del diseño **no** es maximizar cada atributo por separado — eso normalmente es imposible, porque mejorar uno empeora otro. El objetivo real es **evaluar múltiples atributos de calidad y diseñar un sistema que sea "good enough" (suficientemente bueno) para los stakeholders**, balanceando las tensiones entre ellos.

## Resumen

- Funcionalidad y atributos de calidad son **ortogonales** (independientes entre sí).
- El alcance de los atributos de calidad debe ser considerado a lo largo del **diseño, implementación y deployment**.
- Los atributos de calidad normalmente **llevan a tradeoffs**.
- Existen los **escenarios** para relevar atributos de calidad.
- Los resultados satisfactorios dependen tanto de la **arquitectura** (big picture) como de la **correcta implementación** (los detalles).

---

# Clasificaciones de Atributos de Calidad

Los atributos de calidad no viven sueltos: distintos organismos y normas los agruparon bajo **taxonomías** —esencialmente, "nombres" o categorías estandarizadas— para poder manejarlos y comunicarlos como entidades reconocibles.

## ¿Para qué sirve clasificarlos?

1. **Que distintas personas/organizaciones hablen el mismo idioma**: si dos equipos dicen "eficiencia", "confiabilidad" o "usabilidad", se refieren al mismo concepto aunque trabajen en proyectos distintos.
2. **Comparar sistemas entre sí** usando el mismo marco de referencia.
3. **Evaluar y auditar sistemas de forma sistemática** (por eso son *normas*, provienen de organismos de estandarización).
4. **Servir como checklist** al analizar requerimientos, para no olvidarse de considerar ciertos aspectos de calidad.

## Las tres clasificaciones de la filmina

| ISO 9126 | Mitre | IEEE 1061 |
|---|---|---|
| Efficiency | Efficiency | Efficiency |
| Functionality | Reliability | Functionality |
| Maintainability | Usability | Maintainability |
| Reliability | Maintainability | Portability |
| Portability | Expandability | Reliability |
| Usability | Interoperability | Usability |
| (+ distintos sub-factores) | Reusability | |
| | Integrity | |
| | Survivability | |
| | Correctness | |
| | Verifiability | |
| | Flexibility | |
| | Portability | |

- **ISO 9126**: norma internacional (ISO) con 6 características principales, cada una con "sub-factores".
- **Mitre**: organización/contratista de investigación (ligada a proyectos de defensa y gobierno de EE. UU.) que propuso una lista más extensa.
- **IEEE 1061**: estándar del IEEE (organismo de ingeniería eléctrica/electrónica y de software).

## La idea clave: no hay una única clasificación "correcta"

Fijáte que **hay superposición entre las tres** (todas incluyen Efficiency, Reliability, Usability, Maintainability, Portability), pero **no son idénticas** — cada organismo agregó o quitó categorías según su propio enfoque y propósito.

> Esto es similar a cómo distintas disciplinas científicas a veces usan taxonomías levemente distintas para clasificar el mismo fenómeno: la realidad (que el software puede fallar, ser lento, ser difícil de cambiar, etc.) es la misma, pero el "nombre y la caja" en la que se mete cada problema puede variar según quién hizo la clasificación.

## Ejemplos sueltos mencionados en la filmina

Además de las tres clasificaciones formales, la filmina tira una lista abierta de ejemplos de atributos de calidad que no siempre encajan prolijamente en una única norma:

- Performance
- Interoperabilidad
- Modificabilidad
- Desplegabilidad
- Seguridad
- Escalabilidad
- Estabilidad
- "Webifyability"
- Sustentabilidad
- …

Esto refuerza que las clasificaciones son herramientas de organización, no un catálogo cerrado ni definitivo.

---

# Atributos con capítulo propio en el libro (Software Architecture in Practice, 4th ed.)

Cada uno de los siguientes atributos tiene su propio **escenario general** (general scenario) en el libro de Bass/Clements/Kazman, con tácticas y patrones asociados.

## 🟢 Disponibilidad (Availability) — Cap. 4

### ¿Qué es?

Es la capacidad del sistema de seguir prestando servicio a pesar de que ocurran fallas (de hardware, software, comunicación o incluso humanas), minimizando el tiempo de inactividad y detectando/reparando esas fallas lo más rápido posible. No se trata de evitar que las fallas ocurran (eso es imposible), sino de que el sistema las tolere sin dejar de funcionar, o degradando su servicio de forma controlada.

### Escenario General

| Parte | Descripción | Valores posibles |
|---|---|---|
| Fuente | De dónde proviene la falla | Interna/externa: personas, hardware, software, infraestructura física, entorno físico |
| Estímulo | El estímulo de un escenario de disponibilidad es una falla (*fault*) | Falla: omisión, caída (crash), timing incorrecto, respuesta incorrecta |
| Artefacto | Qué partes del sistema son responsables de, o se ven afectadas por, la falla | Procesadores, canales de comunicación, almacenamiento, procesos, artefactos afectados en el entorno del sistema |
| Ambiente | El estado del sistema cuando ocurre el estímulo (no solo operación "normal") | Operación normal, arranque, apagado, modo reparación, operación degradada, operación sobrecargada |
| Respuesta | La respuesta más común es evitar que la falla se convierta en fallo (*failure*), pero también puede implicar notificar o registrar | Prevenir que la falla se vuelva fallo · Detectar la falla: registrarla, notificar a entidades apropiadas, recuperarse de ella, deshabilitar la fuente del evento, quedar temporalmente no disponible durante la reparación, enmascarar/reparar el fallo o contener el daño, operar en modo degradado |
| Medida de Respuesta | Distintas métricas según la criticidad del servicio | Tiempo/intervalo en que el sistema debe estar disponible · Porcentaje de disponibilidad (ej. 99.999%) · Tiempo de detección de la falla · Tiempo de reparación · Tiempo/intervalo en modo degradado · Proporción o tasa de fallas que el sistema previene o maneja sin fallar |

**Ejemplo concreto (del libro):** un servidor de una granja de servidores falla durante la operación normal; el sistema informa al operador y continúa operando sin tiempo de inactividad.

### Tácticas

Se agrupan en tres categorías según su propósito: **detectar fallas**, **recuperarse de fallas** y **prevenir fallas**.

**Detectar Fallas**
- **Monitor**: componente que vigila el estado de salud de otras partes del sistema (procesadores, procesos, I/O, memoria); puede orquestar otras tácticas de detección.
- **Ping/echo**: par de mensajes petición/respuesta asíncronos entre nodos para verificar alcanzabilidad y latencia; requiere un umbral de tiempo (timeout).
- **Heartbeat**: intercambio periódico de mensajes entre un monitor y el proceso monitoreado; a diferencia de ping/echo, la iniciativa la tiene el proceso monitoreado (o el monitor, según la variante).
- **Timestamp**: detecta secuencias incorrectas de eventos en sistemas distribuidos de paso de mensajes.
- **Condition monitoring**: chequea condiciones o valida supuestos de diseño (ej. checksums); el monitor debe ser simple para no introducir nuevos errores.
- **Sanity checking**: verifica la validez/razonabilidad de una operación o salida, típicamente en interfaces.
- **Voting**: compara resultados de múltiples fuentes redundantes y decide cuál usar. Variantes: **replicación** (clones idénticos, protege solo contra fallas aleatorias de hardware), **redundancia funcional** (implementaciones diversas, protege contra fallas de modo común) y **redundancia analítica** (diversidad también en especificación de entradas/salidas, tolera errores de especificación).
- **Exception detection**: detecta condiciones que alteran el flujo normal (excepciones de sistema, parameter fence, parameter typing, timeout).
- **Self-test**: un componente o subsistema se testea a sí mismo.

**Recuperarse de Fallas — Preparación y Reparación**
- **Redundant spare**: uno o más componentes duplicados toman el control si falla el primario (hot/warm/cold spare).
- **Rollback**: revertir a un estado bueno conocido anterior (checkpoint) tras detectar una falla.
- **Exception handling**: manejar la excepción detectada (desde códigos de error simples hasta clases de excepción con info de correlación).
- **Software upgrade**: actualizar código en servicio sin afectarlo (function patch, class patch, hitless ISSU).
- **Retry**: reintentar una operación asumiendo que la falla es transitoria (con límite de reintentos).
- **Ignore faulty behavior**: ignorar mensajes de una fuente que se determina espuria.
- **Graceful degradation**: mantener las funciones más críticas y descartar las menos críticas ante fallas de componentes.
- **Reconfiguration**: reasignar responsabilidades a los recursos que siguen funcionando.

**Recuperarse de Fallas — Reintroducción**
- **Shadow**: operar un componente recién reparado/actualizado en modo "sombra" antes de devolverlo a rol activo.
- **State resynchronization**: sincronizar el estado entre componente activo y de respaldo (vía checksum o hash).
- **Escalating restart**: variar la granularidad del reinicio (de hilos afectados hasta reinicio completo) minimizando el impacto en el servicio.
- **Nonstop forwarding**: separar plano de control y plano de datos (típico en routers) para seguir operando mientras se recupera el control.

**Prevenir Fallas**
- **Removal from service**: sacar temporalmente un componente de servicio para "limpiarlo" antes de que acumule fallas (software rejuvenation).
- **Transactions**: semántica ACID para mensajes asíncronos entre componentes distribuidos (ej. two-phase commit); evita condiciones de carrera.
- **Predictive model**: monitorea el estado de salud para predecir fallas y actuar preventivamente.
- **Exception prevention**: técnicas como wrappers, smart pointers o código de corrección de errores para evitar excepciones.
- **Increase competence set**: diseñar un componente para manejar más casos/fallas como parte de su operación normal (en vez de lanzar excepción y "tirar la toalla").

### Patrones

- **Active redundancy (hot spare) / Passive redundancy (warm spare) / Spare (cold spare)**: variantes de redundant spare según qué tan sincronizado está el backup con el activo (más sincronizado = recuperación más rápida pero más costoso).
- **Triple modular redundancy (TMR)**: 3 componentes redundantes + lógica de votación; simple y eficaz, punto óptimo costo/disponibilidad.
- **Circuit breaker**: evita reintentos infinitos ante una falla persistente, cortando el ciclo hasta que se "resetea"; previene fallas en cascada en sistemas distribuidos.
- **Process pairs**: checkpointing + rollback, el backup toma el control al fallar el primario.
- **Forward error recovery**: avanzar hacia un estado seguro (posiblemente degradado) en vez de retroceder, usando redundancia de datos.

### Cómo reconocerla en un escenario

Aparece cuando el estímulo es una **falla** (de hardware, software, comunicación o entorno) y lo que importa es si el sistema sigue prestando servicio, cuánto tiempo está caído, y qué tan rápido detecta/repara. Palabras clave: *falla, caída, uptime/downtime, tiempo de reparación, redundancia, failover, recuperación*.

## 🚀 Desplegabilidad (Deployability) — Cap. 5

### ¿Qué es?

Es qué tan fácil, rápido y de bajo riesgo resulta llevar una nueva versión de un componente (o del sistema entero) a producción, o revertir un despliegue que salió mal. El foco está puesto en el proceso de release en sí —el pipeline, el rollout, el rollback—, no en el comportamiento del sistema una vez que ya está corriendo.

### Escenario General

| Parte | Descripción | Valores posibles |
|---|---|---|
| Fuente | El disparador del despliegue | Usuario final, desarrollador, administrador del sistema, personal de operaciones, marketplace de componentes, dueño del producto |
| Estímulo | Qué provoca el disparador | Hay un nuevo elemento disponible para desplegar (típicamente reemplazar un elemento de software por una nueva versión: corregir un defecto, aplicar un parche de seguridad, actualizar a la última versión de un componente/framework) · Un nuevo elemento fue aprobado para incorporación · Un elemento/conjunto de elementos existente necesita revertirse (rollback) |
| Artefactos | Qué se va a cambiar | Componentes o módulos específicos, la plataforma del sistema, su interfaz de usuario, su entorno, u otro sistema con el que interopera; puede ser un único elemento, varios, o todo el sistema |
| Ambiente | Staging, producción (o un subconjunto específico de cualquiera de los dos) | Despliegue completo · Despliegue a un subconjunto específico de usuarios, VMs, contenedores, servidores, plataformas |
| Respuesta | Qué debería pasar | Incorporar los nuevos componentes · Desplegar los nuevos componentes · Monitorear los nuevos componentes · Revertir un despliegue anterior |
| Medida de Respuesta | Medida de costo, tiempo o efectividad del proceso, para un despliegue o para una serie de despliegues | Costo en términos de: número/tamaño/complejidad de artefactos afectados, esfuerzo promedio/peor caso, tiempo transcurrido, dinero, nuevos defectos introducidos · Grado en que el despliegue/rollback afecta otras funciones o atributos de calidad · Número de despliegues fallidos · Repetibilidad del proceso · Trazabilidad del proceso · Tiempo de ciclo del proceso |

**Ejemplo concreto (del libro):** una nueva versión de un servicio de autenticación/autorización aparece en el marketplace de componentes y el dueño del producto decide incorporarla; se prueba y despliega a producción en 40 horas y no más de 120 horas-persona, sin introducir defectos ni violar ningún SLA.

### Conceptos clave previos

- **Pipeline de despliegue**: secuencia de herramientas y actividades desde que el código se sube a control de versiones hasta que la app queda desplegada. Ambientes: **desarrollo** (unit tests) → **integración** (build + tests de integración) → **staging** (performance, seguridad, licencias, tests de usuario) → **producción** (monitoreo continuo).
- **Despliegue continuo** (sin intervención humana) vs. **entrega continua** (con intervención humana en el paso final).
- Tres medidas de calidad del pipeline: **cycle time** (velocidad de avance por el pipeline), **trazabilidad** (poder recuperar todos los artefactos/versiones que llevaron a un problema) y **repetibilidad** (obtener el mismo resultado con las mismas entradas).
- **DevOps**: conjunto de prácticas para reducir el tiempo entre un commit y su paso a producción manteniendo alta calidad; el despliegue continuo es su núcleo conceptual. **DevSecOps** incorpora seguridad a todo el proceso.

### Tácticas

**Gestionar el Pipeline de Despliegue**
- **Scale rollouts**: desplegar gradualmente a subconjuntos controlados de usuarios en vez de a todos a la vez, para monitorear y poder revertir si algo falla.
- **Rollback**: revertir un despliegue defectuoso a su estado previo (idealmente de forma automatizada, incluso con múltiples servicios/datos coordinados).
- **Script deployment commands**: automatizar y orquestar los pasos del despliegue mediante scripts versionados y testeados.

**Gestionar el Sistema Desplegado**
- **Manage service interactions**: permitir que convivan múltiples versiones de un servicio simultáneamente, mediando las interacciones para evitar incompatibilidades.
- **Package dependencies**: empaquetar un elemento junto con sus dependencias (librerías, versión de SO, contenedores utilitarios) usando contenedores, pods o VMs.
- **Feature toggle**: "interruptor" para deshabilitar una funcionalidad en runtime sin necesidad de un nuevo despliegue.

### Patrones

**Para estructurar servicios:**
- **Microservice architecture**: servicios pequeños, independientemente desplegables, que solo se comunican por mensajes vía interfaces. Beneficios: reduce el time-to-market, cada equipo elige su tecnología, fácil de escalar. Costos: overhead de red, poco apta para transacciones complejas, requiere catálogos para mantener control intelectual.

**Para reemplazo completo de servicios:**
- **Blue/green**: se crean N instancias nuevas ("green"); cuando funcionan bien se conmuta el tráfico y luego se eliminan las instancias viejas ("blue"). Pico de uso: 2N instancias.
- **Rolling upgrade**: se reemplazan las instancias de a una (o pocas) por vez. Pico de uso: N+1 instancias. Riesgo de inconsistencia temporal (un cliente atendido a veces por la versión vieja, a veces por la nueva) y de incompatibilidad de interfaz si conviven ambas versiones.

**Para reemplazo parcial (multi-versión simultánea):**
- **Canary testing**: un grupo reducido de usuarios (a veces "power users") prueba la nueva versión en producción antes del rollout completo.
- **A/B testing**: se muestran variantes distintas a distintos grupos de usuarios para medir cuál da mejor resultado de negocio (ej. las 41 tonalidades de azul de Google).

### Cómo reconocerla en un escenario

Aparece cuando el estímulo es la **llegada de una nueva versión/elemento a desplegar** (o la necesidad de revertir uno), y lo que importa es el costo/tiempo/riesgo de llevarlo a producción (o hacer rollback), no el comportamiento en producción en sí. Palabras clave: *release, versión, pipeline CI/CD, rollout, rollback, feature flag, staging/producción*.

## 🔋 Eficiencia Energética (Energy Efficiency) — Cap. 6

### ¿Qué es?

Es cuánta energía consume el sistema (o los dispositivos donde corre) y qué tan bien gestiona ese consumo —apagando, degradando o reasignando recursos— sin resignar demasiado otras cualidades como el rendimiento. Es especialmente relevante en sistemas móviles, IoT y data centers, donde la batería o el costo energético son una restricción real.

### Escenario General

| Parte | Descripción | Valores posibles |
|---|---|---|
| Fuente | Quién/qué dispara la gestión de energía | Un individuo (usuario, administrador) o el propio sistema (ej. un proceso que decide autogestionar su consumo) |
| Estímulo | La necesidad de ahorrar o gestionar energía | Se necesita gestionar el consumo de un recurso computacional |
| Artefacto | Qué recurso se gestiona | Dispositivos, servidores, VMs, clusters, etc. específicos |
| Ambiente | Se gestiona típicamente en runtime, pero hay casos especiales según características del sistema | Runtime, conectado, alimentado por batería, modo de batería baja, modo de conservación de energía |
| Respuesta | Qué acciones toma el sistema para conservar/gestionar el uso de energía | Deshabilitar servicios · Liberar servicios en runtime · Cambiar la asignación de servicios a servidores · Correr servicios en un modo de menor consumo · Asignar/liberar servidores · Cambiar niveles de servicio · Cambiar la planificación (scheduling) |
| Medida de Respuesta | Girar en torno a la energía ahorrada/consumida y su efecto sobre otras funciones o atributos de calidad | Carga máxima/promedio en kilowatts · Cantidad promedio/total de energía ahorrada · Total de kilowatts-hora usados · Período durante el cual el sistema debe permanecer encendido — manteniendo el nivel de funcionalidad requerido y niveles aceptables de otros atributos de calidad |

**Ejemplo concreto (del libro):** un gerente quiere ahorrar energía en runtime liberando recursos no utilizados en períodos de baja demanda; el sistema libera recursos manteniendo una latencia máxima de 2 segundos en consultas a la base de datos, ahorrando en promedio el 50% de la energía total requerida.

### Tácticas

Se agrupan en tres categorías: **monitorear recursos**, **asignar recursos** y **reducir la demanda de recursos**.

**Monitorear Recursos**
- **Metering**: medir el consumo real de energía vía sensores en (casi) tiempo real (medidores de potencia, PDUs medidas, sistemas de gestión de baterías).
- **Static classification**: estimar el consumo catálogando recursos y sus características conocidas (benchmarks, specs del fabricante) cuando no hay datos en tiempo real.
- **Dynamic classification**: estimar el consumo con modelos que consideran condiciones transitorias (carga de trabajo), vía tabla de búsqueda, regresión o simulación.

**Asignar Recursos**
- **Reduce usage**: reducir el uso a nivel de dispositivo (bajar el refresh rate, apagar CPUs/servidores no usados, correr a menor clock, consolidar VMs en menos servidores físicos, delegar cómputo a la nube).
- **Discovery**: anotar los pedidos de servicio con información energética para elegir el proveedor más eficiente (ej. un "green service directory").
- **Schedule resources**: planificar tareas considerando el consumo energético además del rendimiento, migrando dinámicamente hacia el proveedor más eficiente.

**Reducir la Demanda de Recursos**

Comparte tácticas con Performance (Cap. 9): manage event arrival, limit event response, prioritize events, reduce computational overhead, bound execution times, increase resource usage efficiency. La diferencia con "reduce usage" es que acá se reduce la demanda misma, no solo el consumo dado un nivel de demanda constante.

### Patrones

- **Sensor fusion**: usar datos de sensores de bajo consumo para inferir si vale la pena consultar un sensor de mayor consumo (ej. acelerómetro antes de actualizar el GPS). Riesgo: si la inferencia consulta seguido el sensor caro, puede terminar consumiendo más.
- **Kill abnormal tasks**: monitorear y matar tareas que consumen energía de forma anómala (útil en apps móviles de origen desconocido); cuidado con el impacto en usabilidad.
- **Power monitor**: apagar automáticamente dispositivos/interfaces no usados activamente; el costo es la latencia extra al reactivarlos.

### Cómo reconocerla en un escenario

Aparece cuando el foco del escenario es el **consumo de energía/batería** en sí (no la velocidad de respuesta ni la disponibilidad), y la medida de respuesta se expresa en kW, kWh, autonomía de batería o "% de energía ahorrada". Muy relevante en sistemas móviles, IoT y data centers.

## 🔌 Integrabilidad (Integrability) — Cap. 7

### ¿Qué es?

Es qué tan fácil (rápido, barato, con poco riesgo) resulta sumar o combinar componentes a un sistema —ya sea uno completamente nuevo, una versión nueva de uno existente, o combinar componentes ya presentes de una forma en la que antes no interactuaban. La dificultad depende del **tamaño** (cuántas dependencias potenciales tiene el componente) y la **distancia** (qué tan grandes son las diferencias sintácticas, semánticas, temporales o de recursos que hay que resolver en cada dependencia). En este libro es, además, el atributo que más se acerca a lo que otras normas llaman "interoperabilidad".

### Escenario General

| Parte | Descripción | Valores posibles |
|---|---|---|
| Fuente | De dónde viene el estímulo | Stakeholder de la misión/sistema · Marketplace de componentes · Proveedor del componente |
| Estímulo | Qué tipo de integración se describe | Agregar un componente nuevo · Integrar una nueva versión de un componente existente · Integrar componentes existentes de una forma nueva |
| Artefacto | Qué partes del sistema están involucradas | Sistema entero · Conjunto específico de componentes · Metadata de un componente · Configuración de un componente |
| Ambiente | En qué estado está el sistema cuando ocurre el estímulo | Desarrollo · Integración · Despliegue · Runtime |
| Respuesta | Cómo responde un sistema "integrable" al estímulo | Los cambios se completan/integran/testean/despliegan · Los componentes en la nueva configuración intercambian información correctamente (sintáctica y semánticamente) · Colaboran exitosamente · No violan límites de recursos |
| Medida de Respuesta | Cómo se mide la respuesta | Costo en términos de: número de componentes cambiados, % de código cambiado, líneas de código cambiadas, esfuerzo, dinero, tiempo calendario · Efectos sobre las medidas de respuesta de otros atributos de calidad (tradeoffs permitidos) |

**Ejemplo concreto (del libro):** un nuevo componente de filtrado de datos aparece en el marketplace de componentes; se integra y despliega en 1 mes, con no más de 1 persona-mes de esfuerzo.

### Concepto clave: "distancia" entre componentes

La dificultad de integración depende del **tamaño** (cantidad de dependencias potenciales) y la **distancia** (dificultad de resolver diferencias en cada dependencia) entre los elementos y el sistema. Tipos de distancia:

- **Sintáctica**: tipo/número de datos compartidos no coincide (ej. entero vs. flotante).
- **Semántica de datos**: mismo tipo de dato, pero interpretado distinto (ej. metros vs. pies).
- **Semántica de comportamiento**: desacuerdo sobre estados/modos del sistema (ej. quién inicia una interacción).
- **Temporal**: supuestos distintos sobre tiempos/tasas (ej. 10 Hz vs. 60 Hz).
- **De recursos**: supuestos distintos sobre recursos compartidos (memoria, ancho de banda, acceso exclusivo vs. compartido).

### Tácticas

Se agrupan en **limitar dependencias**, **adaptar** y **coordinar**.

**Limitar Dependencias**
- **Encapsulate**: introducir una interfaz explícita y forzar que todo el acceso pase por ella (base de las demás tácticas de esta categoría).
- **Use an intermediary**: romper dependencias directas (ej. bus publish-subscribe, repositorio de datos compartido, discovery dinámico).
- **Restrict communication paths**: limitar con quién puede comunicarse un elemento (visibilidad + autorización); típico en SOA con un enterprise service bus.
- **Adhere to standards**: adoptar estándares (IEEE, ISO, OMG) o convenciones locales para reducir dependencias y distancia.
- **Abstract common services**: ocultar servicios similares detrás de una abstracción común, para que futuros componentes se integren con una sola interfaz.

**Adaptar**
- **Discover**: catálogo de direcciones/servicios (discovery service) para localizar dinámicamente a quién integrar.
- **Tailor interface**: agregar u ocultar capacidades de una interfaz existente sin cambiar su API (ej. filtros que validan datos o traducen formatos).
- **Configure behavior**: permitir configurar el comportamiento de un componente en build, inicialización o runtime (ej. soportar distintas versiones de un estándar).

**Coordinar**
- **Orchestrate**: mecanismo de control que coordina la invocación de servicios para que permanezcan ajenos entre sí (workflows, BPEL).
- **Manage resources**: un gestor de recursos intermedia el acceso a recursos computacionales compartidos (similar a restrict communication paths pero para recursos).

### Patrones

- **Wrappers / Bridges / Mediators**: las tres giran en torno a tailor interface. Wrapper: encapsula un componente y traduce su interfaz. Bridge: traduce entre "requires" de un componente y "provides" de otro, independiente de componentes específicos, definido en tiempo de construcción. Mediator: como el bridge pero con planificación en runtime, con mayor autonomía y rol de primera clase en la arquitectura.
- **Service-Oriented Architecture (SOA)**: componentes distribuidos que proveen/consumen servicios estándar (WSDL, SOAP), típicamente entre organizaciones distintas (vs. microservicios, que son de una sola organización).
- **Dynamic discovery**: aplica la táctica discover en runtime, permitiendo bindear consumidor y servicio concreto dinámicamente.

### Cómo reconocerla en un escenario

Aparece cuando el estímulo es la **necesidad de sumar o combinar componentes/sistemas** (nuevos o existentes) y lo que importa es el costo/riesgo de esa integración (no el cambio en sí de una funcionalidad, que sería Modificabilidad). Muy asociada a integraciones con **terceros** y a la palabra "distancia" sintáctica/semántica/temporal/de recursos.

## 🧩 Modificabilidad (Modifiability) — Cap. 8

### ¿Qué es?

Es qué tan fácil, rápido y barato resulta hacer un cambio sobre el sistema propio: agregar, sacar o modificar funcionalidad, cambiar un atributo de calidad, migrar de plataforma o tecnología, corregir un defecto, etc. A diferencia de Integrabilidad (que es sobre sumar/combinar componentes, muchas veces de terceros), acá el foco es el cambio en general sobre lo que ya es propio del sistema. Escalabilidad, portabilidad y variabilidad son "sabores" específicos de este atributo.

### Escenario General

| Parte | Descripción | Valores posibles |
|---|---|---|
| Fuente | El agente que provoca el cambio. La mayoría son actores humanos, pero puede ser el propio sistema si aprende o se automodifica | Usuario final, desarrollador, administrador del sistema, dueño de la línea de producto, el propio sistema |
| Estímulo | El cambio que el sistema necesita acomodar (corregir un defecto también cuenta como cambio) | Directiva de agregar/borrar/modificar funcionalidad, o cambiar un atributo de calidad, capacidad, plataforma o tecnología · Directiva de agregar un nuevo producto a una línea de productos · Directiva de cambiar la ubicación de un servicio |
| Artefactos | Los artefactos que se modifican | Código, datos, interfaces, componentes, recursos, casos de prueba, configuraciones, documentación |
| Ambiente | El momento/etapa en que se hace el cambio | Runtime, tiempo de compilación, tiempo de build, tiempo de inicio, tiempo de diseño |
| Respuesta | Hacer el cambio e incorporarlo al sistema | Hacer la modificación · Testear la modificación · Desplegar la modificación · Automodificarse |
| Medida de Respuesta | Los recursos que se gastaron en hacer el cambio | Costo en términos de: número/tamaño/complejidad de artefactos afectados, esfuerzo, tiempo transcurrido, dinero · Grado en que la modificación afecta otras funciones/atributos de calidad · Nuevos defectos introducidos · Cuánto tardó el sistema en adaptarse |

**Ejemplo concreto (del libro):** un desarrollador quiere cambiar la interfaz de usuario; el cambio se hace en el código en tiempo de diseño, tarda menos de 3 horas en hacerse y testearse, y no genera efectos secundarios.

> **Aclaración: ¿puede ser "usuario final" la Fuente?** Sí, y no es un caso raro — la tabla ya lo contempla. La confusión típica es mezclar **quién pide el cambio** (Fuente) con **quién lo implementa** (que casi siempre es un desarrollador, pero eso es parte de la Respuesta, no de la Fuente).
> - **Developer como fuente**: el cambio nace de una decisión técnica/interna, sin que medie un pedido externo (ej. el ejemplo del libro: un dev quiere refactorizar la UI).
> - **Usuario final como fuente**: el cambio nace de un pedido de negocio/usuario que después un desarrollador va a implementar (ej. "un usuario pide poder exportar a Excel", "un cliente pide un idioma nuevo"). El developer sigue respondiendo al estímulo (hace/testea/despliega el cambio), pero eso es la Respuesta — la Fuente sigue siendo el usuario que lo pidió.
>
> Regla práctica: si el enunciado dice explícitamente "un usuario/cliente pidió X" → Fuente = usuario final. Si el enunciado no aclara quién pide el cambio y se centra en el trabajo técnico ("hay que modificar el algoritmo") → Fuente = developer, que es el default más común.

### "Sabores" específicos de Modificabilidad

- **Escalabilidad**: acomodar más carga. **Horizontal** (scaling out: sumar más nodos, en la nube = *elasticidad*) vs. **vertical** (scaling up: sumar más recursos a una unidad física).
- **Variabilidad**: soportar la producción de variantes preplanificadas de un sistema (clave en líneas de producto).
- **Portabilidad**: facilidad de mover el software a otra plataforma (minimizando y aislando dependencias de plataforma).
- **Independencia de ubicación**: dos partes distribuidas pueden interactuar sin conocer de antemano (o pudiendo cambiar) su ubicación física/lógica.

### Los 4 parámetros que determinan el costo de un cambio

**Acoplamiento** (coupling, cuanto más bajo mejor), **cohesión** (cuanto más alta mejor), **tamaño del módulo** (cuanto más chico mejor) y **momento de binding** (cuanto más tardío, mejor — pero cuesta más prepararlo).

### Tácticas

**Aumentar Cohesión**
- **Split module**: dividir un módulo con responsabilidades no cohesivas en varios módulos más cohesivos.
- **Redistribute responsibilities**: agrupar responsabilidades similares que están dispersas en varios módulos.

**Reducir Acoplamiento**
- **Encapsulate / Use an intermediary / Abstract common services**: las mismas tácticas que en Integrabilidad — reducir dependencias es el objetivo compartido.
- **Restrict dependencies**: limitar con qué módulos puede interactuar un módulo dado (visibilidad + autorización); típico en arquitecturas en capas.

**Diferir el Binding (bindear lo más tarde posible que sea rentable)**
- **En compilación/build**: component replacement, compile-time parameterization, aspects.
- **En despliegue/inicio**: configuration-time binding, resource files.
- **En runtime**: discovery, interpret parameters, shared repositories, polymorphism.

### Patrones

- **Client-server**: bajo acoplamiento cliente–servidor; fácil de escalar; riesgo de latencia de red y de requerir seguridad extra en la comunicación.
- **Plug-in (microkernel)**: núcleo mínimo + plug-ins que agregan funcionalidad vía interfaces fijas; permite evolución independiente pero facilita vulnerabilidades si los plug-ins son de terceros.
- **Layers (capas)**: división en capas con relación de uso unidireccional; una capa solo usa las de más abajo. Beneficio: cambios en capas bajas no afectan a las de arriba (si la interfaz no cambia). Riesgo: overhead de performance y "layer bridging" mal usado rompe la modificabilidad.
- **Publish-subscribe**: comunicación asíncrona por eventos/tópicos, publicadores y suscriptores desacoplados. Riesgo: performance, determinismo y testeabilidad más difíciles de razonar.

### Cómo reconocerla en un escenario

Aparece cuando el estímulo es un **pedido de cambio** (agregar, borrar o modificar funcionalidad, plataforma o atributo) y lo que importa es el costo/tiempo/riesgo de hacer ese cambio sobre el sistema propio. Se diferencia de Integrabilidad en que acá el foco es el cambio en general (no específicamente sumar/combinar componentes externos) y de Desplegabilidad en que acá el foco es hacer el cambio, no llevarlo a producción.

## ⚡ Rendimiento (Performance) — Cap. 9

### ¿Qué es?

Es qué tan rápido responde el sistema a un evento o pedido, y cuántos eventos/pedidos puede procesar por unidad de tiempo, tanto en condiciones normales como bajo carga o sobrecarga. La latencia total de una respuesta se explica por dos factores: el tiempo que el sistema está activamente procesando, y el tiempo que pasa bloqueado esperando un recurso o cómputo del que depende.

### Escenario General

| Parte | Descripción | Valores posibles |
|---|---|---|
| Fuente | De un usuario (o varios), de un sistema externo, o de alguna parte del propio sistema | Externa: pedido de usuario, pedido de sistema externo, datos de un sensor · Interna: un componente pide algo a otro, un timer genera una notificación |
| Estímulo | La llegada de un evento: pedido de servicio o notificación de estado | Llegada de un evento periódico (intervalo predecible), estocástico (según una distribución de probabilidad) o esporádico (ni periódico ni estocástico) |
| Artefacto | Puede ser todo el sistema o solo una parte | Sistema completo · Componente dentro del sistema |
| Ambiente | El estado del sistema/componente cuando llega el estímulo | Runtime, en modo: normal, emergencia, corrección de errores, pico de carga, sobrecarga, operación degradada, u otro modo definido |
| Respuesta | El sistema procesa el estímulo; puede tomar tiempo por cómputo o por bloqueo por contención de recursos | El sistema devuelve una respuesta · Devuelve un error · No genera respuesta · Ignora el pedido si está sobrecargado · Cambia el modo/nivel de servicio · Atiende un evento de mayor prioridad · Consume recursos |
| Medida de Respuesta | Medidas de tiempo o de recursos | Tiempo (máx/mín/promedio/mediana) de respuesta (latencia) · Número/porcentaje de pedidos satisfechos en un intervalo (throughput) · Número/porcentaje de pedidos no satisfechos · Variación del tiempo de respuesta (jitter) · Nivel de uso de un recurso computacional |

**Ejemplo concreto (del libro):** 500 usuarios inician 2000 pedidos en un intervalo de 30 segundos, en operación normal; el sistema procesa todos los pedidos con una latencia promedio de 2 segundos.

### Los dos contribuyentes a la latencia

- **Tiempo de procesamiento**: el sistema está activamente trabajando y consumiendo recursos (CPU, almacenamiento, red, memoria, hilos, buffers).
- **Tiempo bloqueado**: por contención de recursos (varios clientes compiten por el mismo recurso), por no disponibilidad de un recurso, o por dependencia de otro cómputo (sincronización, espera de resultado).

### Tácticas

Se agrupan en **controlar la demanda de recursos** y **gestionar recursos**.

**Controlar la Demanda de Recursos**
- **Manage work requests**: reducir la cantidad de pedidos que entran al sistema.
  - *Manage event arrival*: acordar un SLA que limite la tasa máxima de eventos entrantes.
  - *Manage sampling rate*: reducir la frecuencia de muestreo (ej. fps de un video) a costa de fidelidad.
- **Limit event response**: procesar eventos hasta una tasa máxima, encolando o descartando el resto (con política de qué hacer con los descartados).
- **Prioritize events**: atender primero los eventos más importantes; ignorar los de baja prioridad si faltan recursos.
- **Reduce computational overhead**: *reduce indirection* (menos intermediarios, a costa de modificabilidad), *co-locate communicating resources* (juntar componentes que se comunican mucho para evitar latencia de red), *periodic cleaning* (recalcular/reinicializar estructuras que se vuelven ineficientes).
- **Bound execution times**: limitar cuánto tiempo/iteraciones se usa para responder (a costa de precisión).
- **Increase efficiency of resource usage**: optimizar los algoritmos/lógica crítica (la táctica más "clásica", pero solo una de muchas).

**Gestionar Recursos**
- **Increase resources**: más/mejores procesadores, memoria o red (a veces la forma más barata de mejorar rápido).
- **Introduce concurrency**: procesar en paralelo distintos streams/hilos para reducir el tiempo bloqueado.
- **Maintain multiple copies of computations**: réplicas de un servicio + load balancer para repartir la carga.
- **Maintain multiple copies of data**: replicación de datos o cache (distintas velocidades de acceso); hay que decidir qué cachear.
- **Bound queue sizes**: limitar el tamaño máximo de colas de espera (y definir qué pasa si se desbordan).
- **Schedule resources**: elegir la política de planificación adecuada para cada recurso en contención (procesadores, buffers, red).

### Patrones

- **Service mesh**: infraestructura (sidecars) que maneja concerns transversales de comunicación entre servicios; permite ubicar utilidades en el mismo procesador para reducir tráfico de red, pero agrega procesos y overhead.
- **Load balancer**: reparte pedidos entre múltiples réplicas de un servicio (round-robin, menor carga, etc.); riesgo de convertirse él mismo en cuello de botella o punto único de falla.
- **Throttling**: limita la tasa de pedidos entrantes para manejar picos de demanda con elegancia; la lógica de throttling debe ser muy rápida.
- **Map-reduce**: procesamiento paralelo de grandes volúmenes de datos no ordenados vía funciones map (distribuye/hashea) y reduce (agrega). No conviene con datasets chicos ni si no se pueden particionar en subconjuntos similares.

### Cómo reconocerla en un escenario

Aparece cuando el estímulo es la **llegada de un evento/pedido** (único o en ráfaga) y lo que importa es **cuánto tarda** el sistema en responder o **cuántos pedidos por unidad de tiempo** puede procesar. Palabras clave: *latencia, throughput, tiempo de respuesta, carga, concurrencia, jitter*. Suele confundirse con Escalabilidad (que en el libro es un "sabor" de Modificabilidad: qué tan fácil es *agregar* recursos, no la performance en sí) y con Eficiencia Energética (que comparte varias tácticas de "reducir demanda" pero con el foco puesto en la energía, no en el tiempo).

## ⛑️ Safety (Seguridad Operacional) — Cap. 10

> En español suele traducirse como "seguridad operacional" o "seguridad física/funcional", para no confundirla con Security (Cap. 11, seguridad informática).

### ¿Qué es?

Es la capacidad del sistema de evitar entrar en un estado que cause daño físico, lesión o riesgo a personas o al entorno —y si igual entra en ese estado, detectarlo, contenerlo y recuperarse lo antes posible. Muy frecuente en sistemas médicos, automotrices, aeroespaciales o industriales. No hay que confundirla con Disponibilidad: ahí lo que importa es que el servicio siga andando; acá lo que importa es que nadie salga lastimado.

### Escenario General

| Parte | Descripción | Valores posibles |
|---|---|---|
| Fuente | Una fuente de datos (sensor, componente de software que calcula un valor, canal de comunicación), una fuente de tiempo (reloj), o una acción de usuario | Instancias específicas de: sensor, componente de software, canal de comunicación, dispositivo (ej. reloj) |
| Estímulo | Una omisión, comisión, u ocurrencia de datos o timing incorrectos | Omisión: un valor nunca llega / una función nunca se ejecuta · Comisión: una función se ejecuta mal / un dispositivo produce un evento o dato espúreo · Datos incorrectos: un sensor o componente reporta mal · Falla de timing: datos llegan tarde/temprano, un evento ocurre en el orden equivocado |
| Ambiente | El modo de operación del sistema | Operación normal · Operación degradada · Operación manual · Modo de recuperación |
| Artefactos | Alguna parte del sistema | Porciones críticas para la seguridad del sistema |
| Respuesta | El sistema no abandona el espacio de estados seguro, o vuelve a él, o continúa en modo degradado para prevenir o minimizar daño/lesión. Se avisa a los usuarios y se registra el evento | Reconocer el estado inseguro y: evitarlo · recuperarse · continuar en modo degradado/seguro · apagarse · pasar a operación manual · conmutar a un sistema de respaldo · notificar a entidades apropiadas · registrar el estado inseguro (y la respuesta dada) |
| Medida de Respuesta | Tiempo de vuelta al estado seguro; daño o lesión causados | Cantidad/% de entradas a estados inseguros evitadas · Cantidad/% de estados inseguros de los que el sistema se recupera automáticamente · Cambio en la exposición al riesgo: tamaño(pérdida) × prob(pérdida) · % de tiempo en que el sistema puede recuperarse · Tiempo en modo degradado/seguro · Cantidad/% de tiempo apagado · Tiempo transcurrido para entrar y salir de un modo manual/degradado |

**Ejemplo concreto (del libro):** un sensor de un sistema de monitoreo de pacientes falla en reportar un valor crítico después de 100 ms; se registra la falla, se enciende una luz de advertencia en la consola y se activa un sensor de respaldo (menor fidelidad); el sistema monitorea al paciente con el sensor de respaldo en no más de 300 ms.

### Tácticas

Se agrupan en **evitar estado inseguro**, **detectar estado inseguro**, **contener** y **recuperar**.

**Evitar Estado Inseguro**
- **Substitution**: usar mecanismos de protección (típicamente hardware: watchdogs, monitores, interlocks) en lugar de sus versiones en software, que pueden quedarse sin recursos.
- **Predictive model**: predice el estado de salud del sistema para advertir tempranamente de un problema potencial (ej. control de crucero que calcula la tasa de acercamiento a un obstáculo).

**Detectar Estado Inseguro**
- **Timeout**: detecta que un componente no cumplió sus restricciones de timing.
- **Timestamp**: detecta secuencias incorrectas de eventos (igual que en Disponibilidad).
- **Condition monitoring**: chequea condiciones/supuestos de diseño; alimenta al predictive model y al sanity checking.
- **Sanity checking**: verifica validez/razonabilidad de resultados, entradas o salidas.
- **Comparison**: compara salidas de componentes replicados/sincronizados para detectar un estado inseguro (con 3+ réplicas, también identifica cuál falló).

**Contener (limitar el daño de un estado inseguro ya ocurrido)**
- **Redundancy**: replication, functional redundancy, analytic redundancy (igual lógica que en Disponibilidad, pero acá el objetivo es seguir operando en vez de un total shutdown).
- **Limit consequences**: *abort* (abortar la operación insegura antes de que cause daño), *degradation* (mantener funciones críticas, descartar el resto de forma controlada), *masking* (enmascarar la falla comparando/votando entre componentes redundantes).
- **Barrier**: *firewall* (limita acceso a recursos), *interlock* (protege contra secuenciación incorrecta de eventos controlando el acceso a componentes protegidos).

**Recuperar**
- **Rollback**: volver a un estado bueno conocido (rollback line) tras detectar una falla.
- **Repair state**: reparar el estado erróneo y continuar (ej. lane keep assist que corrige la posición del vehículo); no apto para fallas no anticipadas.
- **Reconfiguration**: remapear la arquitectura lógica sobre los recursos que quedan funcionando, manteniendo toda o parte de la funcionalidad.

> Nota: hay fuerte solapamiento con las tácticas de Disponibilidad, porque los problemas de disponibilidad suelen derivar en problemas de safety y comparten muchas soluciones de diseño.

### Cómo reconocerla en un escenario

Aparece cuando lo que está en juego es **evitar daño físico, lesión o entrada a un estado peligroso** para personas o el entorno (no solo la continuidad del servicio, que sería Disponibilidad). Típico en sistemas médicos, automotrices, aeroespaciales e industriales. Palabras clave: *estado seguro/inseguro, hazard, daño, lesión, modo degradado seguro, apagado de emergencia*.

## 🔒 Seguridad (Security) — Cap. 11

### ¿Qué es?

Es la capacidad del sistema de proteger sus datos y servicios frente a un ataque —un intento deliberado y no autorizado de ver, robar, modificar o borrar datos, o de degradar el servicio—, garantizando confidencialidad, integridad y disponibilidad frente a ese adversario. Se diferencia de Disponibilidad en que acá la causa del problema es intencional (un atacante), no accidental, y de Safety en que el riesgo es sobre datos/servicios, no sobre daño físico.

### Escenario General

| Parte | Descripción | Valores posibles |
|---|---|---|
| Fuente | El ataque puede venir de dentro o fuera de la organización; puede ser una persona u otro sistema; puede haber sido identificado antes (correcta o incorrectamente) o ser desconocido | Humano · Otro sistema — que está: dentro de la organización · fuera de la organización · previamente identificado · desconocido |
| Estímulo | El estímulo es un ataque: un intento no autorizado de | Mostrar datos · Capturar datos · Cambiar o borrar datos · Acceder a servicios del sistema · Cambiar el comportamiento del sistema · Reducir la disponibilidad |
| Artefacto | Cuál es el blanco del ataque | Servicios del sistema · Datos dentro del sistema · Un componente o recurso del sistema · Datos producidos o consumidos por el sistema |
| Ambiente | El estado del sistema cuando ocurre el ataque | El sistema está: online u offline · conectado o desconectado de una red · detrás de un firewall o abierto a una red · totalmente operativo · parcialmente operativo · no operativo |
| Respuesta | El sistema asegura confidencialidad, integridad y disponibilidad | Las transacciones se realizan de forma que: los datos/servicios están protegidos de acceso no autorizado · no se manipulan sin autorización · las partes de una transacción se identifican con certeza · las partes no pueden repudiar su participación · los datos/recursos/servicios estarán disponibles para uso legítimo. Además, el sistema registra actividades: acceso o modificación, intentos de acceso, y notifica a entidades apropiadas ante un aparente ataque |
| Medida de Respuesta | Relacionadas con la frecuencia de ataques exitosos, el tiempo/costo de resistir y reparar, y el daño consecuente | Cuánto de un recurso quedó comprometido o asegurado · Precisión de la detección del ataque · Tiempo transcurrido hasta detectar el ataque · Cuántos ataques se resistieron · Cuánto se tarda en recuperarse de un ataque exitoso · Cuánta información es vulnerable a un ataque particular |

**Ejemplo concreto (del libro):** un empleado descontento en una ubicación remota intenta modificar indebidamente la tabla de sueldos durante operación normal; el acceso no autorizado se detecta, el sistema mantiene un registro de auditoría y los datos correctos se restauran dentro de un día.

### Tácticas

Inspiradas en seguridad física (vallas, guardias, cerraduras, backups). Cuatro categorías: **detectar**, **resistir**, **reaccionar** y **recuperarse de** ataques.

**Detectar Ataques**
- **Detect intrusion**: compara tráfico/patrones de pedidos contra firmas conocidas de comportamiento malicioso.
- **Detect service denial**: compara el tráfico entrante contra perfiles históricos de ataques DoS conocidos.
- **Verify message integrity**: checksums o hashes para verificar la integridad de mensajes y archivos.
- **Detect message delivery anomalies**: detecta posibles ataques man-in-the-middle observando tiempos de entrega o patrones de conexión inusuales.

**Resistir Ataques**
- **Identify actors**: identificar la fuente de cualquier entrada externa (user IDs, IPs, protocolos, puertos).
- **Authenticate actors**: confirmar que un actor es quien dice ser (contraseñas, 2FA, biometría, certificados, CAPTCHA).
- **Authorize actors**: verificar que un actor autenticado tiene permisos sobre datos/servicios (control de acceso por actor, clase o rol).
- **Limit access**: restringir puntos de acceso y tipo de tráfico permitido (ej. DMZ con doble firewall).
- **Limit exposure**: minimizar el efecto del daño reduciendo qué datos/servicios son accesibles desde un mismo punto de acceso.
- **Encrypt data**: proteger confidencialidad de datos y comunicación (simétrica o asimétrica).
- **Separate entities**: aislar partes del sistema (servidores/redes distintos, VMs, air gap) para limitar el alcance de un ataque.
- **Validate input**: sanitizar/filtrar entradas para prevenir SQL injection, XSS, etc.
- **Change credential settings**: forzar el cambio de credenciales por defecto o periódicamente.

**Reaccionar a Ataques**
- **Revoke access**: limitar severamente el acceso ante un ataque en curso, incluso a usuarios normalmente legítimos.
- **Restrict login**: limitar/bloquear tras repetidos intentos fallidos de login (a veces duplicando el tiempo de bloqueo en cada intento).
- **Inform actors**: notificar a operadores, personal o sistemas cooperantes ante un ataque detectado.

**Recuperarse de Ataques**

Se reutilizan las tácticas de recuperación de Disponibilidad, más:
- **Audit**: mantener registro de acciones de usuarios/sistema para rastrear atacantes y mejorar defensas futuras.
- **Nonrepudiation**: garantizar que emisor y receptor de un mensaje no puedan negar haberlo enviado/recibido (firmas digitales + autenticación de terceros confiables).

### Cómo reconocerla en un escenario

Aparece cuando el estímulo es un **ataque o intento de acceso/manipulación no autorizado**, y lo que importa es confidencialidad, integridad, disponibilidad frente a un adversario (no una falla accidental, que sería Disponibilidad, ni un riesgo físico, que sería Safety). Palabras clave: *atacante, acceso no autorizado, autenticación, autorización, cifrado, auditoría, brecha*.

## 🧪 Testeabilidad (Testability) — Cap. 12

### ¿Qué es?

Es qué tan fácil y barato resulta hacer que el sistema "muestre" sus fallas mediante testing: llevarlo a un estado específico, ejecutar el test, y observar el resultado con poco esfuerzo. No se trata de las fallas en producción (eso es Disponibilidad/Safety), sino de qué tan fácil es encontrarlas *antes*, durante el desarrollo.

### Escenario General

| Parte | Descripción | Valores posibles |
|---|---|---|
| Fuente | Los casos de prueba pueden ejecutarlos una persona o una herramienta automatizada | Testers de unidad, de integración, de sistema, de aceptación, usuarios finales — corriendo tests manualmente o con herramientas automatizadas |
| Estímulo | Se inicia un test o conjunto de tests | Validar funciones del sistema · Validar atributos de calidad · Descubrir amenazas emergentes a la calidad |
| Ambiente | El testing ocurre en distintos eventos o hitos del ciclo de vida | El conjunto de tests se ejecuta por: la finalización de un incremento de código (clase, capa, servicio) · la integración completa de un subsistema · la implementación completa del sistema · el despliegue a producción · la entrega al cliente · un cronograma de testing |
| Artefactos | La porción del sistema que se testea y cualquier infraestructura de testing necesaria | Una unidad de código (módulo) · Componentes · Servicios · Subsistemas · El sistema entero · La infraestructura de testing |
| Respuesta | El sistema y su infraestructura de testing pueden controlarse para ejecutar los tests deseados, y los resultados pueden observarse | Ejecutar la suíte de tests y capturar resultados · Capturar la actividad que resultó en la falla · Controlar y monitorear el estado del sistema |
| Medida de Respuesta | Qué tan fácil el sistema bajo prueba "entrega" sus fallas o defectos | Esfuerzo para encontrar una falla o clase de fallas · Esfuerzo para alcanzar cierto % de cobertura del espacio de estados · Probabilidad de que el próximo test revele una falla · Tiempo para ejecutar los tests · Esfuerzo para detectar fallas · Tiempo para preparar la infraestructura de testing · Esfuerzo para llevar al sistema a un estado específico · Reducción de la exposición al riesgo: tamaño(pérdida) × probabilidad(pérdida) |

**Ejemplo concreto (del libro):** un desarrollador completa una unidad de código durante el desarrollo y ejecuta una secuencia de tests que da 85% de cobertura de caminos (path coverage) en 30 minutos.

### Tácticas

Dos categorías: **controlar y observar el estado del sistema** y **limitar la complejidad**.

**Controlar y Observar el Estado del Sistema**
- **Specialized interfaces**: interfaces de testing dedicadas (get/set de variables clave, método que devuelve el estado completo, reset a un estado interno específico, activar logging/instrumentación verbose). Deben mantenerse separadas de las interfaces funcionales.
- **Record/playback**: registrar el estado que cruza una interfaz para poder "reproducir" el sistema y recrear una falla.
- **Localize state storage**: centralizar el estado en un solo lugar (ej. una máquina de estados) para poder arrancar el sistema en un estado arbitrario fácilmente.
- **Abstract data sources**: abstraer las fuentes de datos para poder sustituirlas fácilmente por datos de prueba (ej. apuntar a una base de test en vez de la real).
- **Sandbox**: aislar una instancia del sistema del mundo real para experimentar sin consecuencias permanentes; incluye virtualizar recursos como el reloj, la red o la batería (stubs, mocks, dependency injection).
- **Executable assertions**: aserciones codificadas (pre/post-condiciones, invariantes de clase) que señalan cuándo y dónde el programa entra en un estado defectuoso.

También se mencionan **component replacement** (cambiar la implementación por una versión testeable), **preprocessor macros** y **aspects** para inyectar reportes de estado.

**Limitar Complejidad**
- **Limit structural complexity**: evitar dependencias cíclicas, aislar dependencias del entorno externo, reducir el acoplamiento en general (ej. limitar profundidad de herencia, cantidad de clases hijas, polimorfismo/llamadas dinámicas). Alta cohesión, bajo acoplamiento y separación de concerns (tácticas de Modificabilidad) también ayudan a la testeabilidad. Los patrones en capas facilitan testear capa por capa.
- **Limit nondeterminism**: reducir fuentes de comportamiento no determinista (ej. paralelismo no restringido), ya que un sistema no determinista es mucho más difícil de testear.

### Cómo reconocerla en un escenario

Aparece cuando el estímulo es la **ejecución de un test** (o la necesidad de descubrir/aislar una falla) y lo que importa es cuán fácil/barato es controlar el sistema hacia un estado específico y observar el resultado. Palabras clave: *cobertura, aserción, mock/stub, ambiente de testing, reproducir una falla*. No debe confundirse con Disponibilidad/Safety (que hablan de fallas en producción, no de encontrarlas antes).

## 👥 Usabilidad (Usability) — Cap. 13

### ¿Qué es?

Es qué tan fácil, eficiente y satisfactorio resulta para un usuario final usar el sistema: aprenderlo, operarlo, configurarlo, y recuperarse de sus propios errores con el menor costo posible. El estímulo siempre viene de un usuario interactuando con el sistema (o reaccionando a un evento), y lo que importa es esa experiencia —no el rendimiento técnico interno (Performance) ni la seguridad del acceso (Security).

### Escenario General

| Parte | Descripción | Valores posibles |
|---|---|---|
| Fuente | De dónde viene el estímulo | El usuario final (que puede tener un rol especializado, como administrador de sistema/red) es la fuente principal · También puede ser un evento externo que llega al sistema y al que el usuario reacciona |
| Estímulo | Qué quiere el usuario final | Usar el sistema eficientemente · Aprender a usar el sistema · Minimizar el impacto de errores · Adaptar el sistema · Configurar el sistema |
| Ambiente | Cuándo llega el estímulo al sistema | Siempre en runtime o en tiempo de configuración del sistema |
| Artefactos | Qué parte del sistema se estimula | Una GUI · Una interfaz de línea de comandos · Una interfaz de voz · Una pantalla táctil |
| Respuesta | Cómo debería responder el sistema | Proveer al usuario las funciones que necesita · Anticipar las necesidades del usuario · Dar feedback apropiado al usuario |
| Medida de Respuesta | Cómo se mide la respuesta | Tiempo de tarea · Número de errores · Tiempo de aprendizaje · Ratio tiempo de aprendizaje/tiempo de tarea · Número de tareas completadas · Satisfacción del usuario · Ganancia de conocimiento del usuario · Ratio de operaciones exitosas/totales · Cantidad de tiempo o datos perdidos cuando ocurre un error |

**Ejemplo concreto (del libro):** un usuario descarga una nueva aplicación y la usa productivamente después de solo 2 minutos de experimentación.

### La distinción clave: iniciativa del usuario vs. del sistema

Las tácticas de usabilidad se organizan según quién toma la iniciativa en la interacción. Hay además una conexión fuerte con **Modificabilidad**: el diseño de UI se itera constantemente (diseñar → testear → corregir), así que una arquitectura fácil de modificar hace ese ciclo menos doloroso.

### Tácticas

**Soportar la Iniciativa del Usuario**
- **Cancel**: el sistema debe estar "escuchando" el comando de cancelar, terminar la actividad, liberar recursos usados y avisar a los componentes colaboradores.
- **Undo**: mantener suficiente información de estado (snapshots/checkpoints u operaciones reversibles) para restaurar un estado anterior a pedido del usuario. No todas las operaciones son reversibles (ej. no se puede "des-enviar" un paquete).
- **Pause/resume**: pausar y reanudar una operación larga (ej. una descarga), liberando recursos temporalmente.
- **Aggregate**: agrupar objetos de bajo nivel para aplicarles una operación en conjunto, evitando repetición manual y errores (ej. cambiar la fuente de todos los objetos de una diapositiva a la vez).

**Soportar la Iniciativa del Sistema**

Requieren que el sistema mantenga un modelo:
- **Maintain task model**: modelo de qué está intentando hacer el usuario, para dar asistencia contextual (ej. autocompletado predictivo, corrector ortográfico).
- **Maintain user model**: modelo explícito del conocimiento/comportamiento del usuario o clase de usuarios (ej. apps de idiomas que detectan errores recurrentes y refuerzan esos temas); incluye la personalización explícita de UI.
- **Maintain system model**: modelo explícito del propio sistema, usado para dar feedback apropiado (ej. una barra de progreso que predice el tiempo restante).

### Cómo reconocerla en un escenario

Aparece cuando el estímulo viene de un **usuario final interactuando con el sistema** (o de un evento al que el usuario debe reaccionar), y lo que importa es la eficiencia, facilidad de aprendizaje, manejo de errores o satisfacción de esa interacción — no el rendimiento técnico interno (eso sería Performance) ni la seguridad del acceso (eso sería Security). Palabras clave: *usuario, feedback, cancelar/deshacer, curva de aprendizaje, satisfacción*.

## 🌐 Otros Atributos de Calidad (Cap. 14 SAP)

> Este capítulo no trae una tabla de escenario general propia (no es un atributo único), sino una discusión sobre otros atributos que el libro menciona sin dedicarles capítulo, más cómo trabajar con listas estándar de atributos de calidad. Se agrega acá como complemento, porque puede servir para reconocer un atributo en un escenario que no encaje en los 10 anteriores.

### Atributos de calidad de la arquitectura misma

A diferencia de los demás (que califican el comportamiento del sistema en ejecución), estos miden la **arquitectura como artefacto de desarrollo**:

- **Buildability**: qué tan bien se presta la arquitectura a un desarrollo rápido y eficiente; se mide en costo (dinero o tiempo) de convertir la arquitectura en un producto funcionando que cumpla sus requerimientos.
- **Integridad conceptual**: consistencia del diseño a lo largo de toda la arquitectura ("lo mismo se hace de la misma forma en todos lados"); mejora la comprensibilidad y reduce confusión. Ej.: todos los componentes deberían loguear errores, manejar excepciones y sanitizar datos de la misma manera.
- **Marketability**: la percepción/reputación que trae consigo una arquitectura (ej. la presión de usar cloud o microservicios "porque es lo que se usa", independientemente de si es la mejor opción técnica).

### Development Distributability

Qué tan bien soporta el sistema el desarrollo por **equipos distribuidos** (geográfica u organizacionalmente). Se logra diseñando subsistemas con bajo acoplamiento entre sí (tanto en código como en modelo de datos), para minimizar la coordinación necesaria entre equipos. La estructura arquitectónica y la estructura social/organizacional del proyecto deberían estar alineadas (Ley de Conway).

### Atributos de calidad del sistema físico

En sistemas embebidos (auto, avión, electrodoméstico), el software convive con requerimientos físicos: **peso, tamaño, consumo eléctrico, potencia de salida, emisiones, resistencia climática, duración de batería**, etc. La arquitectura del software puede tener un efecto directo sobre estos (ej. software ineficiente → requiere más memoria/procesador/batería → más peso/consumo/costo). Las mismas técnicas de escenarios sirven para especificar estos atributos de sistema, no solo los de software.

### Uso de listas estándar de atributos de calidad

Existen varias normas/listas de referencia (ninguna es "la correcta"; sirven como checklist). La más citada es **ISO/IEC 25010 (SQuaRE)**, que divide los atributos en un modelo de "calidad de producto" con estas categorías principales:

- Functional suitability (completitud, corrección, adecuación funcional)
- Performance efficiency (comportamiento temporal, utilización de recursos, capacidad)
- Compatibility (coexistencia, interoperabilidad)
- Usability (aprendizaje, operabilidad, estética de UI, accesibilidad)
- Reliability (madurez, disponibilidad, tolerancia a fallas, recuperabilidad)
- Security (confidencialidad, integridad, no repudio, responsabilidad, autenticidad)
- Maintainability (modularidad, reusabilidad, analizabilidad, modificabilidad, testeabilidad)
- Portability (adaptabilidad, instalabilidad, reemplazabilidad)

### Qué hacer cuando el atributo no tiene tabla propia (sección 14.3 del libro)

El libro no se queda solo en "acá hay otros atributos que no cubrimos" — también da un **procedimiento explícito** para cuando hay que trabajar con un QA que no tiene capítulo/tabla propia (lo llama *"Dealing with X-Ability: Bringing a New QA into the Fold"*). Esto resuelve el dilema práctico de qué hacer con atributos que la cátedra menciona (ej. Interoperabilidad) pero que no están entre los 10 con capítulo dedicado (Cap. 4–13: Availability, Deployability, Energy Efficiency, Integrability, Modifiability, Performance, Safety, Security, Testability, Usability).

> **Criterio práctico para armar un escenario (TP):**
> - **Si el atributo tiene capítulo con tabla en el libro** (Availability, Integrability, Modifiability, Performance, Security, Usability, etc.) → usar esa tabla de general scenario como base.
> - **Si no tiene capítulo dedicado** (ej. Interoperabilidad, que en este libro ni siquiera es capítulo aparte) → seguir el procedimiento de la sección 14.3 en vez de inventar una tabla o forzar el escenario en otro atributo.

**El procedimiento (14.3):**

1. **Levantar escenarios concretos primero, no una tabla genérica**: identificar la preocupación real de los stakeholders para ese atributo (entrevistas, riesgos conocidos, árbol de utilidad) y armar escenarios concretos y específicos a partir de eso — no partir de una tabla vacía que no existe.
2. **Generalizar después**: una vez que hay varios escenarios concretos, mirar qué tienen en común los estímulos, las respuestas, las medidas — y de ahí armar el propio "general scenario" ad-hoc, por analogía con cómo están armadas las tablas de los otros capítulos.
3. **Modelar el atributo si se puede**: si existe (o se puede construir) un modelo conceptual del QA — entender de qué parámetros depende y qué características arquitectónicas los afectan — eso ayuda a diseñar soluciones para él, aunque no haya un capítulo dedicado.

Un principio general que el libro remarca acá: **los nombres de los atributos por sí solos sirven poco** — lo que importa es el escenario concreto que se arma, no si se le puso la etiqueta "perfecta". Las listas estándar (como ISO 25010, arriba) son un checklist de referencia, no una camisa de fuerza.

### Cómo usar esta sección

Si un escenario no encaja claramente en Disponibilidad, Desplegabilidad, Eficiencia Energética, Integrabilidad, Modificabilidad, Rendimiento, Safety, Seguridad, Testeabilidad o Usabilidad, puede tratarse de uno de estos atributos "secundarios" (buildability, integridad conceptual, distribuibilidad del desarrollo) o de un atributo del sistema físico en el que corre el software — o simplemente de una combinación/tradeoff entre varios de los diez principales.

---

# Atributos "extra" mencionados en la filmina (sin capítulo propio en el libro)

Estos atributos aparecen en la filmina de la cátedra pero no tienen capítulo dedicado en el libro — el criterio del libro (ver sección 14.3 arriba) es armar el general scenario propio aplicando las 6 partes, en vez de forzar una tabla que no existe.

## 🔗 Interoperabilidad (Interoperability)

> No tiene capítulo propio en el libro de Bass/Clements/Kazman (4ta ed.) — pero sí tiene su propio ejercicio en la filmina de la cátedra (Clase 5, con medida de respuesta explícita).

**Qué es:** capacidad de dos o más sistemas de **intercambiar información** a través de sus interfaces y de **usar correctamente** esa información intercambiada, tanto a nivel **sintáctico** (el formato/estructura es válido) como **semántico** (el significado se interpreta igual de los dos lados). En ISO/IEC 25010 aparece como sub-característica de **Compatibility**, junto con coexistencia.

**Por qué no tiene capítulo en el libro:** Bass et al. reconocen explícitamente que su lista de 10 atributos no agota los que existen, y para casos como interoperabilidad dicen que si le importa a tu organización, corresponde armar el propio general scenario aplicando las 6 partes del framework.

**General scenario propio** (armado a partir de la definición + el ejercicio de la filmina):

- **Fuente**: otro sistema externo o servicio con el que hay que interoperar
- **Estímulo**: llega un mensaje/dato desde el sistema externo (o el propio sistema necesita enviarlo)
- **Artefacto**: la interfaz/adaptador de intercambio de datos
- **Ambiente**: operación normal, con uno o más sistemas externos ya integrados
- **Respuesta**: el sistema interpreta y procesa correctamente los datos recibidos (o el sistema externo interpreta correctamente los emitidos)
- **Medida de respuesta**: % de mensajes/intercambios interpretados sin error (la medida que da la propia filmina)

**Cómo se relaciona con otros atributos:** se confunde fácil con **Integrabilidad**: Integrabilidad es el costo puntual de *ensamblar* un componente nuevo al sistema (una vez, al incorporarlo); Interoperabilidad es que, ya conectados, los sistemas se sigan **entendiendo correctamente en cada intercambio**, de forma sostenida en el tiempo.

> Nota: hay además una sección extensa sobre Interoperabilidad (niveles sintáctico/semántico, sistemas propios vs. de terceros, ejemplo con pasarelas de pago) desarrollada en `template-6-partes-sei.md` dentro de la página de origen "Escenarios de Calidad" — se puede consultar ahí para más profundidad.

## 📦 Portabilidad (Portability)

> No tiene capítulo propio en el libro — el libro la trata como un "sabor" dentro de Modificabilidad (Cap. 8). Acá se profundiza un poco más porque en la filmina de la cátedra aparece como ítem propio.

**Qué es:** facilidad con la que el software puede **moverse/adaptarse a un entorno distinto** del que fue diseñado originalmente (otro hardware, sistema operativo, navegador, nube, etc.), minimizando el esfuerzo y sin requerir cambios significativos. En ISO/IEC 25010 es una característica propia con tres sub-características: **adaptabilidad** (qué tan bien se adapta a distintos entornos), **instalabilidad** (facilidad de instalar/desinstalar) y **reemplazabilidad** (puede reemplazar a otro producto con el mismo propósito en el mismo entorno).

**Por qué el libro no le dedica capítulo:** Bass et al. la capturan directamente como un **tipo de cambio dentro de Modificabilidad**: cambiar de plataforma es, para ellos, una modificación más (con Ambiente típicamente en tiempo de build/compilación). La clave de diseño es la misma que en modificabilidad: **aislar las dependencias de plataforma** en la menor cantidad de módulos posible, para que portar el sistema afecte poco código.

**Cómo armar un escenario de portabilidad:**

- **Fuente**: stakeholder/product owner que necesita soportar un nuevo entorno
- **Estímulo**: pedido de que el sistema corra en una plataforma/entorno nuevo (otro SO, navegador, nube)
- **Artefacto**: el código dependiente de plataforma (o toda la aplicación)
- **Ambiente**: tiempo de diseño/build (portar suele decidirse antes de ejecutar)
- **Respuesta**: el sistema corre correctamente en el nuevo entorno
- **Medida de respuesta**: cantidad de módulos/líneas de código que hubo que tocar para portarlo, o tiempo/costo de la migración

## 📈 Escalabilidad (Scalability)

> No tiene capítulo propio en el libro — el libro la trata explícitamente como un "sabor" dentro de Modificabilidad (Cap. 8), no de Performance. Acá se profundiza porque en la filmina de la cátedra aparece como ítem propio.

**Qué es:** capacidad del sistema de **manejar cargas de trabajo crecientes (o decrecientes)**, o de adaptar su capacidad para soportar esa variación. En ISO/IEC 25010 aparece como sub-característica de **Flexibility**. Se distinguen dos tipos:

- **Vertical (scale up)**: sumar más recursos a una misma unidad física (más CPU/RAM a un servidor).
- **Horizontal (scale out)**: sumar más nodos/instancias. En la nube, cuando esto se hace automáticamente según la demanda, se llama **elasticidad**.

**Por qué el libro la mete adentro de Modificabilidad y no de Performance:** esta es la confusión más común con este atributo. La distinción:

- **Performance**: mide cómo responde el sistema **ante una carga dada** (tiempo de respuesta, throughput) — el sistema no cambia, solo se lo somete a estrés.
- **Escalabilidad**: mide qué tan fácil/barato es **cambiar la capacidad del sistema** para soportar más (o menos) carga — acá sí hay un cambio real sobre el sistema (agregar servidores, reparticionar datos, etc.), por eso Bass la ubica como una faceta de Modificabilidad.

**Cómo armar un escenario de escalabilidad:**

- **Fuente**: administrador del sistema / la propia demanda de usuarios
- **Estímulo**: pedido (o necesidad automática) de aumentar la capacidad para soportar más carga
- **Artefacto**: la infraestructura/arquitectura de despliegue
- **Ambiente**: runtime (si es autoescalado) o tiempo de diseño/deployment (si es planificado)
- **Respuesta**: el sistema agrega/quita recursos y sigue funcionando dentro de los niveles de servicio esperados
- **Medida de respuesta**: costo de la expansión (dinero, tiempo, esfuerzo), o tiempo que tarda en adaptarse la capacidad

> Ver también el desarrollo extendido de un escenario de Escalabilidad completo en `template-6-partes-sei.md`.

## ⚖️ Estabilidad (Stability)

> No tiene capítulo propio en el libro ni en ISO/IEC 25010 como atributo independiente — aparece en la norma previa (ISO/IEC 9126) como sub-característica de Mantenibilidad.

**Qué es:** el **riesgo de que una modificación al sistema tenga efectos inesperados** sobre otras partes que no se querían tocar. Cuanto más estable es un sistema, menos "rompe" otras funcionalidades al cambiar algo puntual. En ISO/IEC 9126 es una de las cuatro sub-características de Mantenibilidad, junto con analizabilidad, capacidad de cambio (changeability) y testeabilidad.

**Relación directa con Modificabilidad:** la **Medida de Respuesta** del general scenario de Modificabilidad incluye explícitamente "grado en que la modificación afecta otras funciones/atributos de calidad" y "nuevos defectos introducidos" — eso es, en la práctica, **medir estabilidad**. Por eso el libro no necesita darle un capítulo propio: la absorbe como una de las medidas de respuesta posibles de Modificabilidad, en vez de tratarla como un atributo separado.

**Cómo se reconoce en un escenario:** si el enunciado pone el foco en "que un cambio no rompa nada más" (en vez de en el costo/tiempo de hacer el cambio en sí), es una pista de que la preocupación central es Estabilidad — pero se especifica igual dentro del template de Modificabilidad, ajustando la Medida de Respuesta a algo como "cantidad de tests que fallan tras el cambio" o "cantidad de módulos no relacionados afectados".

## 🌐 "Webifyability" (término informal)

> Término informal, no está en ninguna norma (ISO 25010, Mitre, IEEE 1061) ni tiene capítulo en el libro. Aparece mencionado en la filmina de la cátedra como ejemplo de la enorme cantidad de "-ilities" que circulan en la literatura.

**Qué es:** un término de humor/jerga que aparece en algunas listas académicas informales de atributos de calidad (junto a otros igual de infrecuentes como "Calibrateability", "Subsetability" o "Configurability"). Se refiere, informalmente, a **qué tan fácil es exponer o adaptar un sistema existente como aplicación web**.

**Por qué no es un atributo "real" con tratamiento propio:** no tiene definición formal, ni general scenario, ni tácticas asociadas en ninguna fuente seria — es básicamente un caso particular de **Portabilidad** (migrar/exponer el sistema a un nuevo "ambiente": la web) mezclado con **Modificabilidad** (los cambios de código que hacen falta para lograrlo).

**Por qué vale la pena tenerlo anotado igual:** es el mejor ejemplo dentro de esta clase para el principio que dice el propio libro: **el nombre del atributo importa poco** — lo que importa es poder armar un escenario concreto y medible. Si apareciera un enunciado sobre esto, conviene tratarlo directamente como Portabilidad (o Modificabilidad, según el foco), sin buscarle una tabla propia que no existe.

## 🌱 Sustentabilidad (Sustainability)

> No es parte de las clasificaciones clásicas que aparecen en la filmina de la cátedra (ISO 9126, Mitre, IEEE 1061) ni tiene capítulo en el libro (aunque se relaciona fuerte con Eficiencia Energética, Cap. 6). Es un atributo relativamente reciente, todavía en discusión activa en la literatura de arquitectura de software.

**Qué es:** según investigación reciente en arquitectura de software (Venters et al.), es la **capacidad de un sistema de perdurar ("endure") en entornos cambiantes**, sin acumular deuda técnica insostenible ni degradarse con el tiempo. Se suele descomponer en cuatro dimensiones:

- **Técnica**: que el sistema se pueda mantener y evolucionar de forma costo-eficiente durante todo su ciclo de vida.
- **Económica**: preservación y creación de valor/capital a lo largo del tiempo.
- **Ambiental**: minimizar el impacto en recursos naturales (energía, emisiones, hardware descartado).
- **Social**: continuidad de las comunidades/usuarios que dependen del sistema.

**Relación con lo que ya vimos del libro:** la dimensión **ambiental** se superpone bastante con **Eficiencia Energética** — de hecho, varios autores tratan la eficiencia energética como el aspecto de sustentabilidad más fácil de medir con un general scenario clásico (fuente, estímulo de "conservar energía", medida en kWh). Pero sustentabilidad como concepto es más amplio: incluye también cuánto le cuesta a la organización mantener el sistema vivo en el tiempo (dimensión técnica/económica), que se superpone con **Modificabilidad**.

**Cómo tratarlo si aparece en un escenario:** dado que no hay una tabla estándar consolidada (a diferencia de Eficiencia Energética), conviene decidir primero **qué dimensión de sustentabilidad plantea el enunciado** (ambiental → tirar hacia Eficiencia Energética; técnica → tirar hacia Modificabilidad) y armar el escenario con esa tabla como base, en vez de inventar una tabla nueva de cero.

## 🔍 Observabilidad / Trazabilidad (Observability)

> No tiene capítulo propio en el libro ni aparece en la filmina de la cátedra — surgió al analizar el sistema de monopatines del TP3, buscando un ancla distinta a Disponibilidad.

**Qué es:** capacidad del sistema de **exponer información sobre su estado interno y su historial de comportamiento**, de forma que un operador humano (o una herramienta de monitoreo) pueda **diagnosticar, auditar o reconstruir** qué pasó, sin necesidad de intervenir directamente en cada componente. Es especialmente relevante en sistemas distribuidos e IoT, donde hay muchos dispositivos físicos difíciles de inspeccionar uno por uno.

**Cómo se distingue de Disponibilidad (para no confundirlas):** es fácil pisarlas porque ambas usan datos de sensores/estado, pero preguntan cosas distintas:

- **Disponibilidad** responde "¿dónde/cómo está el sistema **ahora**?" (ej.: ¿el monopatín está ubicable en este momento?).
- **Observabilidad/Trazabilidad** responde "¿puedo **reconstruir/entender** qué pasó a lo largo del tiempo?" (ej.: ¿puedo saber cuántos km recorrió un monopatín y con qué desgaste, para decidir si necesita mantenimiento?).

Por eso conviene anclar Observabilidad en el registro histórico/reportes, no en el GPS en tiempo real (eso ya lo cubre Disponibilidad).

**General scenario propio** (aplicando las 6 partes, ejemplo del sistema de monopatines de TP3):

- **Fuente**: Encargado de Mantenimiento (o el Administrador)
- **Estímulo**: necesita determinar si un monopatín requiere mantenimiento
- **Artefacto**: módulo de registro de uso / generación de reportes
- **Ambiente**: operación normal
- **Respuesta**: el sistema expone el historial de kilómetros y tiempo de uso (con y sin pausas) de forma consultable
- **Medida de respuesta**: el encargado obtiene el dato completo en menos de X segundos, con 100% de precisión respecto al uso real registrado

**Stakeholder interesado:** Encargado de Mantenimiento / Administrador de Monopatines — les importa poder auditar el estado y uso pasado de la flota para tomar decisiones (mantenimiento, bajas, reposición), no solo saber la ubicación actual.

---

# Cómo se identifican y priorizan los atributos con los stakeholders (QAW)

## 〰️ QAW – Quality Attribute Workshop

El **QAW (Quality Attribute Workshop)** es el método que responde a la pregunta de fondo: ¿cómo identifico cuáles son los atributos de calidad importantes para MI proyecto, **antes** de empezar a diseñar la arquitectura?

> Un método facilitado que involucra a los stakeholders del sistema desde etapas tempranas en el ciclo de vida, a fin de descubrir los atributos de calidad clave para un sistema.

### Puntos clave del método

- **Centrado en el sistema**
- **Focalizado en los stakeholders**
- Se utiliza normalmente **antes de que se construya la arquitectura**

### ¿Cuándo se realiza un QAW?

- En la **fase inicial** de un enfoque centrado en arquitectura → llamada **Inception**.
- Es **compatible con métodos ágiles** como Scrum, típicamente como el "Sprint 0" (antes de arrancar a codear).
- También aplica a **sistemas legados (brownfield)**, donde ya existe una arquitectura previa — ahí el QAW sirve para comparar el sistema **as-is** (como está hoy) contra el **to-be** (cómo debería quedar), y planear la evolución.

### Configuración típica del workshop

El QAW **no es solo una entrevista informal** — es un evento estructurado, con roles definidos y captura de información en tiempo real, visible para todos los participantes.

**Roles presentes:**
- **Customer Stakeholders**: el grupo de stakeholders del cliente
- **Facilitator**: quien conduce la sesión, hace las preguntas, mantiene el foco
- **Remaining Architecture Design Team**: el resto del equipo de arquitectura, que participa/escucha
- **Electronic Scribe**: registra digitalmente lo que se discute
- **Flip Chart Scribe**: anota a mano en un rotafolio/pizarra

**Materiales visuales expuestos en la sala** (carteles donde se documenta en vivo): Operational Descriptions, Quality Attributes, Constraints, Agenda, Parking Lot (espacio para "estacionar" temas que surgen pero no son el foco del momento).

### Los 3 outputs que produce un QAW

1. **Árbol de utilidad** — los atributos identificados, priorizados jerárquicamente (ver `metodo-arbol-utilidad.md`).
2. **Escenarios y casos de uso** — tanto escenarios de calidad como requerimientos funcionales se relevan en paralelo, no por separado.
3. **Captura de restricciones (constraints)** — condiciones no negociables que limitan el espacio de diseño.

**Ejemplo real de restricciones capturadas en un QAW:**

| ID | Restricción (Constraint) |
|---|---|
| CON-1 | Debe soportarse un mínimo de 50 usuarios simultáneos. |
| CON-2 | El sistema debe accederse vía navegador web (Chrome V3.0+, Firefox V4+, IE8+) en Windows, OSX y Linux. |
| CON-3 | Debe usarse un servidor de base de datos relacional existente, exclusivo para esa base. |
| CON-4 | La conexión de red a las estaciones de usuario puede tener bajo ancho de banda, pero es generalmente confiable. |
| CON-5 | Los datos de performance deben recolectarse en intervalos no mayores a 5 minutos. |

Las restricciones son **distintas** de los atributos de calidad: no son negociables ni "se optimizan" — son condiciones fijas del contexto que el arquitecto debe respetar sí o sí al diseñar.

## 🤝 Dinámicas Ágiles para QAW

El QAW "clásico" del SEI puede tomar varios días y requiere muchos stakeholders — apropiado para sistemas de alto riesgo. Michael Keeling propone una versión más **liviana y ágil**, con cuatro técnicas concretas.

### 1. Entrevistar Stakeholders

Entrevistas **semi-estructuradas**, una por una, con cada stakeholder. Se parte de información general, se validan **escenarios iniciales de atributos de calidad**, y luego se resume todo en una **visión global de preocupaciones (concerns) y posibles riesgos**.

### 2. Mini QAW

Una versión "liviana" del QAW completo del SEI, basada en:

1. **Brainstorming** de escenarios "crudos" (sin formalizar todavía en las 6 partes del template)
2. **Priorización** de esos escenarios (por ejemplo con *dot voting*: cada participante pone puntitos/votos sobre los que más le importan)
3. **Refinamiento** de los que salieron con mayor prioridad, recién ahí llevándolos al template de 6 partes

**Agenda típica de un Mini-QAW:**

| Actividad | Tiempo | Nota |
|---|---|---|
| Introducción al Mini-QAW | 10 min | Preparar a los participantes |
| Enseñar sobre Atributos de Calidad | 15 min | Nivelar conocimiento |
| Recorrer las propiedades del sistema | — | — |
| Brainstorming de escenarios crudos | 30 min – 2+ horas | — |
| Priorizar escenarios crudos | 5 min | Usando dot voting |
| Refinar escenarios | Hasta que se acabe el tiempo | El resto queda como tarea (homework) |
| Revisar resultados | 1 hora | En una reunión separada, futura |

**Tips importantes para esta dinámica:**

- No preocuparse por la **formalidad** durante el brainstorming — la prioridad es que fluyan ideas.
- Hacer **preguntas indagatorias** sobre estímulo, respuesta y ambiente (esto va llenando el template mentalmente mientras se conversa).
- **Prestar atención cuando los stakeholders suenan preocupados por algo** — esas preocupaciones suelen ser la fuente de un escenario importante (ver el ejemplo del Cyber Monday en `template-6-partes-sei.md`: nace de una preocupación expresada, no de un template vacío).
- Cuidado con confundir **features/requerimientos funcionales** con atributos de calidad (el mismo problema visto en el Ejercicio de Seguridad de `template-6-partes-sei.md`, con los "dos momentos" — auditoría permanente vs. respuesta ante ataque).
- **No saltearse el trabajo de "tarea" (homework)** — es la parte más importante, donde se termina de refinar todo con calma.

Después del mini-QAW se hace una **reunión de seguimiento** para revisar los escenarios refinados, chequear la precisión de los números "straw man", discutir información faltante y volver a confirmar las prioridades. Cualquier escenario "crudo" que no se llegó a refinar se considera automáticamente de **baja prioridad**.

### 3. Response Measure Straw Man

Ver sección dedicada más abajo.

### 4. Stakeholder Map

Un diagrama tipo **red** que conecta a los distintos actores del sistema entre sí, en términos de sus necesidades, relaciones y objetivos. Se hace **mayormente entre el equipo del proyecto** (no necesariamente con todos los stakeholders presentes), y sirve para **visualizar de un vistazo quién le importa a quién**, y de dónde pueden surgir tensiones o atributos de calidad en conflicto.

### El flujo completo de QAW con estas dinámicas

```
QAW (evento facilitado, con roles y logística definida)
        ↓
Técnicas usadas dentro del QAW:
  • Entrevistas a stakeholders → recolectar concerns/riesgos
  • Stakeholder Map → visualizar relaciones entre actores
  • Mini-QAW (brainstorming → priorización → refinamiento) → escenarios crudos
  • Straw Man → primera estimación de medidas de respuesta
        ↓
Resultados documentados:
  • Árbol de utilidad (atributos priorizados)
  • Escenarios de calidad (con el template de 6 partes)
  • Restricciones (constraints)
        ↓
Todo esto es el INPUT para empezar recién ahí a diseñar la arquitectura
```

## 🪨 Straw Man y Estimación de Medidas de Respuesta

Cuando recién se está definiendo un escenario (por ejemplo en un QAW, al principio del proyecto), todavía no se construyó nada — no hay cómo saber con certeza si se va a poder cumplir un número como "500ms" o "no más de 10% de degradación". En ese momento, el número que se pone es, efectivamente, una **hipótesis de trabajo**. El **Response Measure Straw Man** es el mecanismo formal para manejar esa incertidumbre inicial.

### Qué es el Straw Man

> Mecanismo para aproximar medidas de respuesta de escenarios.

El primer número que se pone es intencionalmente un **"borrador descartable"**, no un compromiso final. Es una forma de **arrancar la conversación** con los stakeholders, no de cerrarla.

### El sesgo de Anchoring

> Keep an eye out for anchoring. Anchoring is a cognitive bias where people let the first information they hear drive their decision making. The straw man should be a reasonable estimate or so outrageous it will be rejected outright. Exercise caution if your outrageous estimate is accepted.

**Anchoring** es un sesgo cognitivo por el cual la primera cifra que se escucha condiciona desproporcionadamente la decisión final, aunque no tenga fundamento real. Por eso el straw man debe ser una estimación **razonable**, o tan **exagerada** que se vaya a rechazar de inmediato. Si una estimación exagerada termina siendo aceptada sin cuestionamiento, es una señal de alerta de que el grupo está anclando en el número en vez de razonarlo.

**¿Se redondea más amplio o más acotado?** No hay una dirección fija:

1. **Empezar amplio/laxo (fácil de cumplir)** → se espera que alguien diga *"no, necesitamos más que eso"* → esto revela **ambición/urgencia real del negocio**.
2. **Empezar acotado/exigente (difícil de cumplir)** → se espera que alguien diga *"eso es técnicamente imposible/carísimo"* → esto revela **restricciones técnicas o de costo reales**.

Lo constante es la intención: **el straw man tiene que estar lo suficientemente lejos del valor final como para que alguien reaccione y lo corrija con fundamento**, no que se acepte sin cuestionar.

### Ejemplo real (sistema de información en la nube)

| Atributo de Calidad | Medida de Respuesta | Straw Man | Medida Aceptada |
|---|---|---|---|
| Changeability | Tiempo para agregar un algoritmo | 6 meses | 2 iteraciones |
| Portability | Esfuerzo para migrar a otro proveedor cloud | 3 persona-meses | 4 persona-días |
| Performance | Tiempo de respuesta promedio bajo carga típica | 1 minuto | 3 segundos máx |
| Scalability | Carga de usuarios que el sistema debe soportar | 10 req/seg | 140 req/seg |

Nótese cómo **cambian drásticamente** los valores entre el straw man y el aceptado — a veces el straw man queda muy por debajo (10 vs 140 req/seg) y a veces muy por arriba (6 meses vs 2 iteraciones). Esto demuestra que el número inicial casi nunca es el correcto: es solo el punto de partida para generar discusión y ajuste.

### ¿Es solo una promesa, o hay análisis detrás?

Al inicio sí es, en efecto, una estimación — pero existe un proceso para no dejarla así, y ajustarla progresivamente con evidencia:

1. **Datos históricos** (si existe un sistema previo): si se está rediseñando o migrando un sistema (caso *brownfield*), se mide el comportamiento actual y se usa como base real.
2. **Spikes / prototipos técnicos**: antes de comprometerse con un número, se hace una prueba de concepto acotada, se la somete a carga simulada, para tener evidencia empírica.
3. **Análisis arquitectural con tácticas conocidas**: un arquitecto con experiencia puede estimar con más fundamento si una meta es alcanzable, basándose en qué tácticas planea aplicar.
4. **Benchmarks de la industria / literatura técnica**: para ciertos atributos hay valores de referencia conocidos.
5. **Negociación iterativa con el stakeholder**: el straw man se corrige en diálogo — no es una imposición unilateral.

**El proceso completo, iterativo:**

```
Straw man inicial (estimación gruesa)
        ↓
Análisis arquitectural: ¿qué tácticas/patrones permitirían cumplir esto?
        ↓
Prototipo/spike técnico si hay mucha incertidumbre
        ↓
Ajuste del número con evidencia real
        ↓
Medida de respuesta final (más confiable, aunque nunca 100% garantizada hasta tenerlo en producción)
```

> **La verdad final, sin vueltas:** nunca hay **certeza absoluta** de que se va a cumplir el número antes de construir el sistema. Lo que SÍ da este proceso (straw man → análisis → ajuste) es pasar de "una promesa a ciegas" a "una estimación informada y negociada", que después se **valida y refina continuamente** con testing de carga y monitoreo en producción.

> Nota adicional dejada en la página de origen: si no se encuentra un valor luego de varias iteraciones, puede ser indicador de que no se tiene muy en claro el dominio del problema, y conviene dejarlo archivado para analizarlo y re-preguntar más adelante.
