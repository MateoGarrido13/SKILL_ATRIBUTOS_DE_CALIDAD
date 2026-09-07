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
