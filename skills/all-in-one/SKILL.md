---
name: sei-quality-attribute-scenarios
description: Genera escenarios de atributos de calidad siguiendo la plantilla de 6 partes del SEI (Software Engineering Institute) / ATAM (fuente del estímulo, estímulo, artefacto, entorno, respuesta y medida de la respuesta), evalúa si un escenario dado está completo y propone cómo completarlo, y construye árboles de utilidad (utility trees) para priorizar requisitos de arquitectura. Usa esta skill siempre que el usuario mencione "atributo de calidad", "escenario de calidad", "ATAM", "QAW" (Quality Attribute Workshop), "árbol de utilidad"/"utility tree", "requisitos no funcionales", o describa un sistema y pida identificar, revisar, completar o priorizar sus atributos de calidad (rendimiento, disponibilidad, seguridad, modificabilidad, usabilidad, testabilidad, interoperabilidad, etc.), aunque no lo pida en esos términos exactos.
---

# Escenarios de Atributos de Calidad (SEI / ATAM)

Esta skill cubre tres tareas relacionadas del método SEI para requisitos de arquitectura:

1. **Generar** escenarios de atributos de calidad nuevos para un sistema.
2. **Auditar** un escenario existente: ¿están las 6 partes presentes y son concretas? Si no, ¿cómo completarlo?
3. **Construir un árbol de utilidad** que organice y priorice los escenarios.

Antes de generar contenido, entiende el sistema: su propósito, usuarios, restricciones técnicas y de negocio, y qué preocupa a los stakeholders. Si el usuario ya describió el sistema en la conversación, úsalo; si falta contexto esencial para producir escenarios concretos (no genéricos), pregunta — pero con una sola ronda de preguntas específicas, no un interrogatorio.

## La plantilla de 6 partes

Todo escenario de atributo de calidad bien formado tiene estas partes. Un escenario incompleto casi siempre falla en una o dos de ellas, típicamente la medida de la respuesta (queda vaga) o el estímulo (queda sin especificar el disparador concreto).

| # | Parte | Pregunta que responde | Ejemplo concreto (evitar vaguedad) |
|---|-------|------------------------|--------------------------------------|
| 1 | **Fuente del estímulo** (source of stimulus) | ¿Quién o qué genera el estímulo? | "Un usuario autenticado", "Un pico de tráfico de campaña de marketing", "Un desarrollador del equipo de pagos" |
| 2 | **Estímulo** (stimulus) | ¿Qué condición o evento dispara la respuesta? | "Envía 1000 solicitudes/seg de checkout", "Solicita agregar un nuevo método de pago", no solo "hay mucha carga" |
| 3 | **Artefacto** (artifact) | ¿Qué parte del sistema recibe el estímulo? | "El servicio de checkout", "El módulo de autenticación", "La API de pagos" — no "el sistema" en general si se puede acotar |
| 4 | **Entorno** (environment) | ¿En qué condiciones ocurre? | "En operación normal", "Durante un failover", "En horario pico (Black Friday)", "En modo degradado con 1 de 3 réplicas caídas" |
| 5 | **Respuesta** (response) | ¿Qué debe hacer el sistema? | "El sistema procesa las solicitudes sin degradar el checkout de otros usuarios", "El sistema registra el intento y notifica al equipo de seguridad" |
| 6 | **Medida de la respuesta** (response measure) | ¿Cómo se sabe, con un número, que la respuesta fue satisfactoria? | "Latencia p95 < 300ms", "Disponibilidad ≥ 99.95% mensual", "El cambio se implementa en ≤ 2 días-persona sin afectar otros módulos" |

La regla práctica: si no puedes poner un número, unidad o criterio verificable en la parte 6, el escenario todavía no está completo, sin importar cuán bien redactadas estén las otras cinco partes.

### Escenarios generales vs. concretos

El SEI distingue dos niveles:

- **Escenario general**: independiente del sistema, sirve para cualquier sistema del mismo dominio de atributo (ej. "El sistema detecta una falla de componente y se recupera sin pérdida de datos").
- **Escenario concreto**: instancia específica del sistema en cuestión, con las 6 partes llenas con detalles reales del contexto (nombres de componentes, números, SLAs).

Cuando el usuario pida "generar escenarios", produce primero el escenario general (para anclar el atributo de calidad) y luego el concreto (aplicado a su sistema). Cuando pida "revisar/completar" un escenario, trabaja directamente sobre el concreto que te dieron.

## Tarea 1: Generar escenarios nuevos

1. Identifica con el usuario (o infiere del contexto) qué atributos de calidad son relevantes para el sistema: rendimiento, disponibilidad, seguridad, modificabilidad, usabilidad, testabilidad, interoperabilidad, u otros específicos del dominio.
2. Para cada atributo, consulta `references/quality-attributes-catalog.md` — tiene, por atributo, las preguntas típicas de elicitación y ejemplos de las 6 partes ya adaptados a ese atributo, para no partir de cero ni sonar genérico.
3. Redacta cada escenario como una tabla de 6 filas (usa la tabla de arriba como formato) o como una oración estructurada seguida de la tabla — pregunta al usuario si no lo especificó, aunque la tabla es el formato por defecto porque hace explícito qué falta.
4. No inventes números de negocio específicos (SLAs, presupuestos) que el usuario no haya dado; en su lugar, propdomn un valor razonable y márcalo explícitamente como supuesto a validar (ej. "Latencia p95 < 300ms *(supuesto: ajustar según SLA real)*").

## Tarea 2: Auditar si un escenario está completo

Cuando el usuario dé un escenario (en prosa, en tabla, o parcial), sigue este proceso:

1. **Mapea** lo que dieron a las 6 partes. Muchas veces el usuario mezcla 2-3 partes en una frase ("cuando hay mucho tráfico el sistema debe responder rápido") — sepáralas explícitamente en vez de asumir que ya está completo.
2. **Evalúa cada parte** con dos criterios, no solo "¿está presente?":
   - **Presencia**: ¿la parte fue mencionada, aunque sea implícitamente?
   - **Concreción**: ¿es específica y no ambigua? ("rápido" no es una medida de respuesta; "p95 < 300ms" sí lo es. "el sistema" no es un artefacto acotado si el sistema tiene múltiples componentes.)
3. Presenta el resultado en una tabla de auditoría:

   | Parte | Estado | Comentario |
   |---|---|---|
   | Fuente del estímulo | ✅ / ⚠️ Vaga / ❌ Falta | ... |
   | Estímulo | ... | ... |
   | Artefacto | ... | ... |
   | Entorno | ... | ... |
   | Respuesta | ... | ... |
   | Medida de la respuesta | ... | ... |

4. **Para cada parte marcada ⚠️ o ❌**, propone cómo completarla:
   - Si la información se puede inferir razonablemente del contexto ya dado (otros escenarios, descripción del sistema), complétala tú y dilo explícitamente ("Asumiendo que el artefacto es el servicio de checkout, según lo descrito arriba...").
   - Si de verdad falta información que solo el usuario tiene (un SLA de negocio, un umbral de riesgo aceptable), no inventes el número — pregunta puntualmente por ese dato, o entrega 2-3 opciones plausibles con su justificación para que el usuario elija.
5. Entrega el escenario completado al final como una sola tabla limpia de 6 filas, para que quede listo para usar, además de la tabla de auditoría.

## Tarea 3: Construir un árbol de utilidad

El árbol de utilidad organiza los escenarios para priorizarlos frente a los stakeholders. Estructura:

```
Utilidad (raíz)
├── Rendimiento
│   ├── Tiempo de respuesta bajo carga normal
│   │   └── Escenario: ... (Importancia: Alta, Dificultad: Media) → (A, M)
│   └── Throughput en picos
│       └── Escenario: ... (A, A)
├── Disponibilidad
│   └── Recuperación ante falla de nodo
│       └── Escenario: ... (M, B)
├── Seguridad
│   └── ...
└── Modificabilidad
    └── ...
```

Pasos para construirlo:

1. **Ramas de primer nivel** = atributos de calidad relevantes (no más de 6-8; si hay más, agrupa).
2. **Sub-ramas** = refinamientos o preocupaciones específicas dentro de cada atributo (ej. dentro de Rendimiento: "latencia de checkout", "throughput de búsqueda").
3. **Hojas** = escenarios concretos (las 6 partes, resumidas en una frase para el árbol; el detalle completo va aparte si se necesita).
4. **Cada hoja se etiqueta con (Importancia, Dificultad)**, típicamente en escala Alta/Media/Baja (o H/M/L), asignada por los stakeholders — si el usuario no dio esos valores, pregúntale o propón una asignación razonada y márcala como propuesta a validar.
5. Los escenarios (Alta, Alta) son los candidatos prioritarios para el análisis de arquitectura (son los que más impactan y más cuesta lograr).

### Formato de salida del árbol

- Por defecto en el chat, usa una lista anidada en markdown como el ejemplo de arriba (rápida de leer, no requiere herramientas).
- Si el usuario pide un diagrama visual, ofrece generarlo como diagrama (árbol/mindmap) usando las herramientas de visualización disponibles en la conversación en vez de dibujarlo a mano en texto.
- Si el usuario pide una tabla resumen para priorizar, complementa el árbol con una tabla ordenada por prioridad:

  | Escenario | Atributo | Importancia | Dificultad | Prioridad |
  |---|---|---|---|---|
  | ... | ... | A | A | 1 |

## Entrega en Word

Si el usuario pide el resultado (escenarios, auditoría o árbol de utilidad) como documento descargable (.docx), usa la skill de docx para producirlo — no reconstruyas el manejo de Word manualmente. Mantén las mismas tablas y estructura que usarías en el chat.

## Referencias

- `references/quality-attributes-catalog.md`: catálogo de atributos de calidad comunes (rendimiento, disponibilidad, seguridad, modificabilidad, usabilidad, testabilidad, interoperabilidad) con preguntas de elicitación y escenarios generales típicos de cada uno. Consúltalo al generar escenarios nuevos para no partir de cero.
- `references/examples.md`: ejemplos completos y trabajados (escenario, auditoría de completitud, y fragmento de árbol de utilidad) tomados de casos clásicos de la literatura del SEI, útiles como calibración de estilo y nivel de detalle esperado.
