# Catálogo de atributos de calidad

Para cada atributo: qué preguntar al usuario para elicitar escenarios reales, y un escenario general de referencia (las 6 partes) que sirve de plantilla al generar el escenario concreto del sistema del usuario.

## Rendimiento (Performance)

**Preguntas de elicitación**: ¿Qué operaciones son sensibles a la latencia? ¿Cuál es la carga esperada en operación normal y en picos? ¿Hay un throughput mínimo requerido?

**Escenario general**:
| Parte | Contenido |
|---|---|
| Fuente | Usuarios / procesos externos |
| Estímulo | Llegan N solicitudes por segundo |
| Artefacto | Componente(s) que procesan la solicitud |
| Entorno | Operación normal / pico de carga |
| Respuesta | El sistema procesa las solicitudes dentro del tiempo esperado |
| Medida | Latencia (p50/p95/p99) y/o throughput con umbral numérico |

## Disponibilidad (Availability)

**Preguntas**: ¿Qué componentes son de misión crítica? ¿Qué falla se puede tolerar (nodo, red, dependencia externa)? ¿Cuál es el SLA/uptime objetivo?

**Escenario general**:
| Parte | Contenido |
|---|---|
| Fuente | Falla interna (hardware, proceso) o externa (dependencia caída) |
| Estímulo | El componente falla o deja de responder |
| Artefacto | El componente afectado y el que debe compensarlo |
| Entorno | Operación normal |
| Respuesta | El sistema detecta la falla y continúa operando (failover, degradación controlada) |
| Medida | Tiempo de detección + tiempo de recuperación (MTTR) y/o % de disponibilidad objetivo |

## Seguridad (Security)

**Preguntas**: ¿Qué activos hay que proteger? ¿Qué tipo de ataque o acceso no autorizado es la principal preocupación? ¿Qué debe pasar si se detecta un intento?

**Escenario general**:
| Parte | Contenido |
|---|---|
| Fuente | Usuario no autorizado / atacante externo o interno |
| Estímulo | Intenta acceder, modificar o exfiltrar datos/funcionalidad protegida |
| Artefacto | El recurso protegido (dato, servicio, API) |
| Entorno | Operación normal o durante un ataque activo |
| Respuesta | El sistema bloquea el acceso, registra el evento y notifica |
| Medida | % de intentos bloqueados, tiempo hasta la notificación, sin acceso no autorizado exitoso |

## Modificabilidad (Modifiability)

**Preguntas**: ¿Qué tipos de cambio son frecuentes o anticipados (nuevo requisito, nueva integración, cambio de UI)? ¿Quién hace el cambio? ¿Cuánto debería costar en tiempo/esfuerzo?

**Escenario general**:
| Parte | Contenido |
|---|---|
| Fuente | Desarrollador / equipo de producto |
| Estímulo | Solicita un cambio específico (nueva función, nueva integración, cambio de regla de negocio) |
| Artefacto | Componente(s) que deben modificarse |
| Entorno | Tiempo de diseño / desarrollo (no en producción) |
| Respuesta | El cambio se implementa sin afectar otros componentes no relacionados |
| Medida | Esfuerzo (días-persona), número de componentes tocados, sin regresiones |

## Usabilidad (Usability)

**Preguntas**: ¿Quiénes son los usuarios (expertos, novatos)? ¿Qué tareas deben poder completar fácilmente? ¿Cómo se mide "fácil" (tiempo, errores, necesidad de ayuda)?

**Escenario general**:
| Parte | Contenido |
|---|---|
| Fuente | Usuario final (nuevo o experimentado) |
| Estímulo | Intenta completar una tarea por primera vez / recuperarse de un error |
| Artefacto | La interfaz o flujo relevante |
| Entorno | Uso normal |
| Respuesta | El usuario completa la tarea con ayuda mínima o nula |
| Medida | Tiempo para completar la tarea, tasa de éxito, número de errores, satisfacción reportada |

## Testabilidad (Testability)

**Preguntas**: ¿Qué tan rápido se puede detectar una falla al ejecutar pruebas? ¿Qué % de código/rutas debe ser cubierto? ¿Es posible probar componentes de forma aislada?

**Escenario general**:
| Parte | Contenido |
|---|---|
| Fuente | Equipo de QA / desarrollador |
| Estímulo | Ejecuta una suite de pruebas tras un cambio |
| Artefacto | El componente modificado y sus dependencias |
| Entorno | Tiempo de desarrollo / integración continua |
| Respuesta | Las pruebas se ejecutan e identifican fallas sin necesitar el sistema completo |
| Medida | Tiempo de ejecución de la suite, % de cobertura, esfuerzo para aislar el componente |

## Interoperabilidad (Interoperability)

**Preguntas**: ¿Con qué sistemas externos se integra? ¿Qué formatos/protocolos deben soportarse? ¿Qué pasa si el sistema externo cambia su contrato?

**Escenario general**:
| Parte | Contenido |
|---|---|
| Fuente | Sistema externo |
| Estímulo | Envía o solicita datos en un formato/protocolo acordado |
| Artefacto | El componente de integración (adaptador, API gateway) |
| Entorno | Operación normal |
| Respuesta | El sistema intercambia los datos correctamente / detecta incompatibilidad y la reporta |
| Medida | % de intercambios exitosos, tiempo de detección de incompatibilidades |

---

Este catálogo no es exhaustivo — si el sistema del usuario tiene atributos específicos del dominio (ej. "seguridad del paciente" en salud, "cumplimiento regulatorio" en finanzas), constrúyelos con la misma estructura de 6 partes en vez de forzarlos en una de estas categorías genéricas.
