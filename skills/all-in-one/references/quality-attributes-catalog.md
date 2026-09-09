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

## Escalabilidad (Scalability)

**Preguntas de elicitación**: ¿Qué dimensión debe escalar (usuarios concurrentes, volumen de datos, número de transacciones)? ¿Se espera escalar de forma horizontal, vertical o ambas? ¿Cuál es el crecimiento proyectado y en qué plazo? ¿Debe escalar automáticamente o es aceptable una intervención manual?

**Escenario general**:
| Parte | Contenido |
|---|---|
| Fuente | Aumento sostenido o repentino de carga (usuarios, datos, transacciones) |
| Estímulo | La demanda crece por encima de la capacidad actual del sistema |
| Artefacto | Componente(s) o capa que debe escalar (servicio, base de datos, cola) |
| Entorno | Crecimiento gradual de operación normal / evento de pico repentino |
| Respuesta | El sistema añade capacidad (nodos, réplicas, recursos) sin degradar el servicio ni requerir rediseño |
| Medida | Tiempo para escalar (aprovisionar nueva capacidad), costo marginal por unidad de capacidad añadida, degradación de rendimiento tolerada durante el escalado (ej. < 10%) |

## Portabilidad (Portability)

**Preguntas de elicitación**: ¿A qué otras plataformas, sistemas operativos, nubes o dispositivos podría necesitar moverse el sistema? ¿Qué dependencias del entorno actual (SO, hardware, proveedor cloud) son las más riesgosas de mantener? ¿Con qué frecuencia se anticipa un cambio de entorno?

**Escenario general**:
| Parte | Contenido |
|---|---|
| Fuente | Equipo de desarrollo / negocio (decisión de cambiar de plataforma o proveedor) |
| Estímulo | Se requiere ejecutar el sistema (o un componente) en un nuevo entorno (SO, hardware, proveedor cloud, dispositivo) |
| Artefacto | El sistema o el componente dependiente del entorno original |
| Entorno | Tiempo de migración / despliegue en el nuevo entorno |
| Respuesta | El sistema se ejecuta correctamente en el nuevo entorno con cambios mínimos al código fuente |
| Medida | % de código reutilizado sin modificación, esfuerzo (días-persona) para completar la migración, número de componentes que requirieron cambios |

## Desplegabilidad (Deployability)

**Preguntas de elicitación**: ¿Con qué frecuencia se despliegan cambios a producción? ¿El despliegue es manual o automatizado (CI/CD)? ¿Qué debe pasar si un despliegue falla — hay rollback? ¿Se requiere desplegar sin tiempo de inactividad?

**Escenario general**:
| Parte | Contenido |
|---|---|
| Fuente | Desarrollador / pipeline de integración continua |
| Estímulo | Se dispara un despliegue de una nueva versión (cambio de código, configuración o infraestructura) |
| Artefacto | El componente o servicio a desplegar y su pipeline de despliegue |
| Entorno | Producción / staging, en horario normal o en ventana de mantenimiento |
| Respuesta | El sistema se despliega exitosamente sin interrupción del servicio, o revierte automáticamente si falla una validación |
| Medida | Tiempo total de despliegue, tiempo de inactividad (idealmente 0), tiempo de rollback ante fallo, % de despliegues exitosos sin intervención manual |

## Recuperabilidad (Recoverability)

**Preguntas de elicitación**: ¿Qué tipos de desastre o pérdida de datos hay que contemplar (corrupción, borrado accidental, caída de datacenter)? ¿Cuál es la pérdida de datos máxima tolerable (RPO)? ¿Cuál es el tiempo máximo de restauración tolerable (RTO)?

**Escenario general**:
| Parte | Contenido |
|---|---|
| Fuente | Falla catastrófica (corrupción de datos, caída de infraestructura, error humano) |
| Estímulo | Se pierde o corrompe información, o el sistema queda inoperable |
| Artefacto | El almacén de datos o servicio afectado, y el mecanismo de respaldo/restauración |
| Entorno | Después de un incidente, durante el proceso de recuperación |
| Respuesta | El sistema restaura los datos y el servicio a un estado consistente conocido |
| Medida | Objetivo de punto de recuperación — RPO (máxima pérdida de datos tolerada, ej. ≤ 5 min) y objetivo de tiempo de recuperación — RTO (ej. ≤ 1 hora) |

## Operabilidad (Operability)

**Preguntas de elicitación**: ¿Qué necesita el equipo de operaciones para monitorear la salud del sistema? ¿Qué tareas rutinarias de administración deben poder hacerse sin downtime (configuración, actualización, escalado manual)? ¿Cómo se detectan y diagnostican problemas en producción?

**Escenario general**:
| Parte | Contenido |
|---|---|
| Fuente | Equipo de operaciones / SRE |
| Estímulo | Necesita monitorear, diagnosticar o administrar el sistema en producción (ej. investigar una anomalía, aplicar una configuración) |
| Artefacto | El componente relevante y sus interfaces de observabilidad/administración (logs, métricas, paneles) |
| Entorno | Operación normal o durante un incidente |
| Respuesta | El sistema expone la información necesaria y permite realizar la acción administrativa sin afectar el servicio |
| Medida | Tiempo para detectar una anomalía, tiempo para diagnosticar la causa raíz, % de tareas administrativas que no requieren downtime |

## Eficiencia energética / Sostenibilidad (Energy Efficiency)

**Preguntas de elicitación**: ¿Hay restricciones de consumo energético o costo de infraestructura por eficiencia? ¿El sistema corre en dispositivos con batería limitada (móvil, IoT)? ¿Existen metas de sostenibilidad o huella de carbono que el sistema deba respetar?

**Escenario general**:
| Parte | Contenido |
|---|---|
| Fuente | Carga de trabajo normal del sistema (procesos, usuarios, dispositivos) |
| Estímulo | El sistema ejecuta una operación o permanece en espera durante un período prolongado |
| Artefacto | El componente o dispositivo cuyo consumo se mide (servidor, app móvil, sensor IoT) |
| Entorno | Operación normal / modo inactivo (idle) |
| Respuesta | El sistema completa la operación o permanece en espera minimizando el consumo de recursos (CPU, energía, red) |
| Medida | Consumo energético por operación o por unidad de tiempo (ej. Wh por transacción), impacto en duración de batería, costo de infraestructura asociado al consumo |

---

Este catálogo no es exhaustivo — si el sistema del usuario tiene atributos específicos del dominio (ej. "seguridad del paciente" en salud, "cumplimiento regulatorio" en finanzas), constrúyelos con la misma estructura de 6 partes en vez de forzarlos en una de estas categorías genéricas.
