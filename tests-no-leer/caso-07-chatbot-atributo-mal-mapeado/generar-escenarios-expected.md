# Caso 07 — Chatbot — expected para `generar-escenarios-calidad`

## Requerimiento 2 (chatbot 24hs) — reclasificación obligatoria

**Comportamiento esperado:** la skill **no** acepta "Escalabilidad" tal como lo etiquetó el usuario. El texto no menciona crecimiento de carga (eso dispararía Escalabilidad) — pide que el asistente esté en pie en todo momento, lo cual ancla a **Disponibilidad**. La skill debe decirlo explícitamente y explicar la diferencia (ver la regla de "justificar el descarte del atributo confundible" agregada al paso 1.2 del `SKILL.md`): *"No es Escalabilidad — ese atributo se dispara cuando crece la carga del sistema. Lo que pedís es que el asistente esté disponible en cualquier momento, que es Disponibilidad."*

**Comportamiento esperado sobre el Estímulo (regla nueva, no inventar):** el enunciado "disponible las 24 hs" no describe ningún evento concreto que ponga a prueba al sistema (no dice si el problema es una caída, una consulta a una hora rara, una sobrecarga puntual). Bajo la regla nueva de `heuristica-inferencia.md`, el Estímulo **no se puede inferir en silencio** — la skill debe preguntar, ofreciendo lecturas concretas, por ejemplo:

> ⚠️ Antes de fijar el Estímulo: ¿qué evento pone a prueba al asistente — (a) una falla que lo tira abajo, (b) una consulta de un usuario a cualquier hora sin que haya fallado nada, o (c) un pico de consultas simultáneas? Y para la Medida de la respuesta: ¿preferís (1) % de disponibilidad en un período, (2) tiempo de reparación tras una caída en minutos, o (3) duración máxima de una caída continua?

**No es aceptable** que la skill complete directamente algo como "Fuente: falla del sistema. Estímulo: el asistente se cae. Medida: <5 min" sin haber preguntado — eso es inventar el evento central del escenario, exactamente lo que la regla nueva prohíbe.

## Requerimiento 1 (nuevas funciones de pago) — clasificación esperada

**Atributo esperado:** Modificabilidad (agregar una función de pago es un cambio sobre el sistema existente, no sumar un componente externo nuevo — a diferencia del caso 05, acá no hay ambigüedad real con Integrabilidad porque el texto no habla de conectar con un sistema de terceros en sí, sino de modificar el propio flujo de pago para soportar Mercado Pago).

**Sobre la Medida de la respuesta:** el input ya trae un número ("8 módulos"), pero la unidad es ambigua ("módulos y/o artefactos" mezcla código, configuración y pruebas bajo el mismo conteo). Esto es un caso para `chequear-completitud-escenario`, no para inventar una unidad al momento de generar: la skill debe dejar explícito que el conteo necesita una sola unidad definida (por ejemplo, solo módulos de código fuente existentes) antes de poder verificarse, ofreciendo 2-3 alternativas concretas de qué contar, en vez de asumir una en silencio.
