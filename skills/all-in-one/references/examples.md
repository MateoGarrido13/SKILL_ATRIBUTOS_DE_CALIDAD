# Ejemplos trabajados

Estos ejemplos son variaciones de casos clásicos usados en la literatura del SEI (p. ej. "Software Architecture in Practice", Bass/Clements/Kazman) adaptados como referencia de estilo. Úsalos para calibrar nivel de detalle y formato, no los copies literalmente cuando el usuario tenga su propio sistema.

## Ejemplo 1: Disponibilidad — sistema de comercio electrónico

**Escenario concreto completo:**

| Parte | Contenido |
|---|---|
| Fuente del estímulo | El nodo de base de datos primario |
| Estímulo | Deja de responder por falla de hardware |
| Artefacto | El servicio de catálogo de productos |
| Entorno | Operación normal, horario de tráfico medio |
| Respuesta | El sistema conmuta automáticamente a la réplica secundaria sin intervención manual |
| Medida de la respuesta | Conmutación completa en ≤ 30 segundos, sin pérdida de transacciones confirmadas, disponibilidad mensual ≥ 99.95% |

## Ejemplo 2: Auditoría de un escenario incompleto — sistema de simulador de vuelo

**Escenario tal como lo dio el usuario**: "Cuando el sistema recibe muchos datos de sensores, debe seguir funcionando bien."

**Tabla de auditoría:**

| Parte | Estado | Comentario |
|---|---|---|
| Fuente del estímulo | ⚠️ Vaga | "Se recibe" no dice de dónde — ¿sensores de vuelo, red externa, otro subsistema? |
| Estímulo | ⚠️ Vaga | "Muchos datos" no es un estímulo medible — falta volumen/tasa |
| Artefacto | ❌ Falta | No se identifica qué componente recibe los datos (¿el módulo de procesamiento de telemetría?) |
| Entorno | ❌ Falta | No se dice si esto ocurre en vuelo normal, en una maniobra crítica, etc. |
| Respuesta | ⚠️ Vaga | "Funcionando bien" no es una respuesta verificable |
| Medida de la respuesta | ❌ Falta | No hay número ni criterio de éxito |

**Escenario completado (asumiendo el contexto de un simulador de vuelo militar, a validar con el usuario):**

| Parte | Contenido |
|---|---|
| Fuente del estímulo | Los sensores de la aeronave *(supuesto — confirmar si son sensores físicos o simulados)* |
| Estímulo | Envían datos de telemetría a una tasa de 1000 mensajes/segundo |
| Artefacto | El módulo de procesamiento de telemetría |
| Entorno | Durante una maniobra de vuelo crítica (alta carga de sensores) |
| Respuesta | El sistema procesa todos los mensajes sin descartarlos y actualiza la pantalla del piloto |
| Medida de la respuesta | Latencia de procesamiento por mensaje < 50ms, 0% de mensajes descartados |

## Ejemplo 3: Fragmento de árbol de utilidad

```
Utilidad
├── Disponibilidad
│   └── Falla del nodo de BD primario → conmutación automática (Ejemplo 1)  (A, M)
├── Rendimiento
│   └── Procesamiento de telemetría en maniobra crítica (Ejemplo 2)  (A, A)
├── Modificabilidad
│   └── Agregar nuevo método de pago sin tocar el flujo de checkout  (M, B)
└── Seguridad
    └── Bloqueo de intentos de acceso no autorizado a datos de pago  (A, M)
```

Los escenarios (A, A) — Alta importancia, Alta dificultad — son los prioritarios para el análisis de arquitectura, porque combinan mayor impacto en el negocio con mayor riesgo técnico.

## Ejemplo 4: Caso completo — sistema de alquiler de monopatines eléctricos

Este ejemplo cubre las tres tareas de la skill (generar escenarios, auditar uno incompleto, y armar un árbol de utilidad) sobre un mismo sistema, para mostrar cómo se relacionan entre sí en un caso real de punta a punta.

**Contexto del sistema**: app móvil para usuarios que alquilan monopatines eléctricos desde paradas fijas (activación por QR, cobro por tiempo vía Mercado Pago, pausas de hasta 15 minutos, geolocalización por GPS) más una app Web de gestión para el Administrador de Monopatines y el Encargado de Mantenimiento.

### 4.1 Escenario concreto — Disponibilidad: finalización de viaje sujeta a validación GPS

| Parte | Contenido |
|---|---|
| Fuente del estímulo | El usuario del servicio, desde la app móvil |
| Estímulo | Selecciona "Finalizar viaje" al llegar a una parada |
| Artefacto | El módulo de finalización de viajes, que consulta el servicio de geolocalización del monopatín |
| Entorno | Operación normal, en zona urbana con cobertura GPS variable |
| Respuesta | El sistema valida la posición del monopatín contra el listado de paradas habilitadas; si está dentro del radio permitido, cierra el viaje y registra fecha, hora y kilómetros recorridos; si no, informa al usuario y mantiene el viaje activo |
| Medida de la respuesta | Validación de ubicación resuelta en ≤ 3 segundos, margen de error de detección de parada ≤ 10 metros, disponibilidad del servicio de geolocalización ≥ 99.5% mensual *(supuesto — ajustar según SLA real del proveedor de GPS)* |

### 4.2 Escenario concreto — Rendimiento: activación del monopatín por QR

| Parte | Contenido |
|---|---|
| Fuente del estímulo | Un usuario autenticado con crédito cargado en su cuenta |
| Estímulo | Escanea el código QR de un monopatín disponible en una parada |
| Artefacto | El servicio de activación de viajes (backend) |
| Entorno | Hora pico, con alta concurrencia de activaciones simultáneas *(supuesto: ~500 activaciones/minuto en el centro de la ciudad)* |
| Respuesta | El sistema valida el crédito disponible de la cuenta, genera el viaje con fecha y hora de inicio, y envía la señal de encendido al monopatín |
| Medida de la respuesta | Tiempo entre el escaneo y el encendido efectivo ≤ 2 segundos en el 95% de los casos, tasa de error de activación < 1% |

### 4.3 Auditoría de un escenario incompleto — pausas del viaje

**Escenario tal como lo daría el usuario**: "Cuando el usuario pausa el viaje, el sistema debe manejarlo bien y cobrar lo que corresponde."

**Tabla de auditoría:**

| Parte | Estado | Comentario |
|---|---|---|
| Fuente del estímulo | ✅ | El usuario del servicio, desde la app, está implícito |
| Estímulo | ⚠️ Vaga | "Pausa el viaje" no distingue si la pausa dura menos o más de los 15 minutos permitidos, que es la condición que cambia todo el comportamiento |
| Artefacto | ❌ Falta | No se identifica qué componente gestiona la pausa (¿el módulo de viajes? ¿el firmware del monopatín que se apaga?) |
| Entorno | ❌ Falta | No se dice si la pausa ocurre dentro o fuera de la ventana de 15 minutos |
| Respuesta | ⚠️ Vaga | "Manejarlo bien" no es verificable; el negocio define dos respuestas distintas (apagar sin desasignar vs. reanudar automáticamente con tarifa extra) |
| Medida de la respuesta | ❌ Falta | No hay número ni criterio de éxito para ninguno de los dos casos |

**Escenario completado** (tomando el caso de pausa extensa, que es el de mayor riesgo de negocio porque involucra un cambio de tarifa):

| Parte | Contenido |
|---|---|
| Fuente del estímulo | El usuario del servicio, que pausó el viaje y no lo reanudó a tiempo |
| Estímulo | Transcurren más de 15 minutos desde el inicio de la pausa sin que el usuario la finalice |
| Artefacto | El módulo de gestión de viajes/pausas |
| Entorno | Viaje en curso, monopatín apagado por la pausa pero aún asignado a la cuenta |
| Respuesta | El sistema reanuda automáticamente el viaje como "en uso", y comienza a aplicar la tarifa extra por reinicio de pausa extensa definida por el Administrador de Monopatines |
| Medida de la respuesta | El cambio de tarifa se aplica exactamente a los 15:00 minutos ± 1 segundo desde el inicio de la pausa, y queda reflejado en el reporte de uso con y sin pausas del viaje |

### 4.4 Escenario concreto — Seguridad: anulación de cuentas

| Parte | Contenido |
|---|---|
| Fuente del estímulo | El Administrador de Monopatines |
| Estímulo | Detecta actividad fraudulenta (o recibe un pedido válido) y solicita anular una cuenta |
| Artefacto | El módulo de gestión de cuentas de la app Web |
| Entorno | Operación normal, posiblemente con un viaje en curso asociado a esa cuenta |
| Respuesta | El sistema anula la cuenta, bloquea toda activación futura de monopatines asociada a ella, y registra la acción con motivo, fecha y usuario administrador responsable |
| Medida de la respuesta | Bloqueo de nuevas activaciones efectivo en el 100% de los casos desde el momento de la anulación, registro de auditoría disponible de inmediato en los reportes *(supuesto: no se anulan viajes ya en curso, a validar con negocio)* |

### 4.5 Escenario concreto — Modificabilidad: agregar una nueva tarifa extra

| Parte | Contenido |
|---|---|
| Fuente del estímulo | El Administrador de Monopatines |
| Estímulo | Solicita definir una nueva tarifa extra (por ejemplo, por daño del monopatín) |
| Artefacto | El módulo de configuración de tarifas de la app Web |
| Entorno | En tiempo de operación normal, sin afectar viajes en curso |
| Respuesta | El administrador crea la tarifa desde la interfaz, sin requerir intervención de desarrollo, y esta queda disponible para aplicarse a partir de ese momento |
| Medida de la respuesta | Tarifa nueva visible y aplicable en ≤ 5 minutos, sin necesidad de re-despliegue de código ni tiempo de inactividad del servicio |

### 4.6 Fragmento de árbol de utilidad — sistema de monopatines

```
Utilidad
├── Disponibilidad
│   └── Finalización de viaje con validación GPS (Ejemplo 4.1)  (A, A)
├── Rendimiento
│   └── Activación de monopatín por QR en hora pico (Ejemplo 4.2)  (A, M)
├── Confiabilidad de datos de cobro
│   └── Reinicio automático de tarifa tras pausa extensa (Ejemplo 4.3)  (A, M)
├── Seguridad
│   └── Anulación de cuentas fraudulentas (Ejemplo 4.4)  (M, B)
└── Modificabilidad
    └── Alta de nuevas tarifas extra sin re-despliegue (Ejemplo 4.5)  (M, B)
```

Los escenarios de Disponibilidad (4.1) y Rendimiento (4.2) quedan como (A, A)/(A, M) porque el negocio depende directamente de ellos (un monopatín que no puede devolverse, o que tarda en encenderse, frena todo el flujo de ingresos), mientras que Seguridad y Modificabilidad, aunque importantes, son de menor dificultad técnica relativa en este dominio.
