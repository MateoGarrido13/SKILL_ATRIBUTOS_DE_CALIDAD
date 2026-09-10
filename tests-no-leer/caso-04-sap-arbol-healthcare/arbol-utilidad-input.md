# Caso 04 (SAP — Árbol de Utilidad, Healthcare) — input para skill-arbol-utilidad

> Fuente: Software Architecture in Practice (Bass/Clements/Kazman), Tabla 19.1, "Tabular Form of the Utility Tree for a System in the Healthcare Space" (cap. 19, sección 19.4). Escenarios traducidos al español, organizados por atributo y refinamiento, **sin las etiquetas H/M/L** — este es el input que recibe la skill para que ella misma las asigne.

## Rendimiento (Performance)

- **Tiempo de respuesta de transacciones**: el sistema debe responder a las transacciones de los usuarios dentro de un tiempo aceptable en condiciones de operación normal.
- **Throughput**: el sistema debe procesar un volumen suficiente de transacciones por unidad de tiempo en condiciones de operación normal.

## Usabilidad (Usability)

- **Entrenamiento de competencia (proficiency training)**: un nuevo usuario del sistema debe poder alcanzar un nivel de competencia operativa aceptable después de un período de entrenamiento razonable.
- **Eficiencia de las operaciones**: un usuario habitual del sistema debe poder completar las operaciones cotidianas con un esfuerzo mínimo.

## Configurabilidad (Configurability)

- **Configurabilidad de datos**: el sistema debe permitir configurar qué datos se capturan y cómo se presentan, adaptándose a las necesidades particulares de cada institución médica, sin requerir cambios de código.

## Mantenibilidad (Maintainability)

- **Cambios de rutina — escenario 1**: un desarrollador debe poder realizar un cambio de rutina (ej. corregir un defecto menor) en un componente del sistema con un esfuerzo acotado y sin introducir efectos secundarios.
- **Cambios de rutina — escenario 2**: un desarrollador debe poder realizar un cambio de rutina distinto (ej. ajustar una regla de validación) en otro componente del sistema, también con esfuerzo acotado.
- **Actualización de componentes comerciales (upgrades)**: el sistema debe poder incorporar una nueva versión de un componente de software comercial (COTS) del que depende, sin comprometer la funcionalidad existente.
- **Agregar una nueva funcionalidad**: el sistema debe poder incorporar una nueva funcionalidad solicitada por el negocio sin requerir un rediseño mayor de la arquitectura existente.

## Seguridad (Security)

- **Confidencialidad**: el sistema debe proteger la confidencialidad de los datos de los pacientes frente a accesos no autorizados.
- **Resistencia a ataques**: el sistema debe resistir intentos de ataque (acceso no autorizado, manipulación de datos) sobre la información médica que administra.

## Disponibilidad (Availability)

- **Sin tiempo de inactividad**: el sistema debe seguir operando sin interrupciones ante fallas de sus componentes, dado el carácter crítico de la información médica que gestiona.
- **Acceso web 24/7/365**: el sistema debe estar accesible vía web de forma continua, todos los días del año.
