# Caso 06 — Hospital — expected para `generar-escenarios-calidad`

**Comportamiento esperado:** la skill detecta que el input mezcla dos preocupaciones sin relación causal entre sí (seguridad de acceso vs. rendimiento bajo carga) y genera **dos escenarios separados**, cada uno con su propio atributo — no uno solo, y no fusionados en un mismo Estímulo/Respuesta.

Referencia de los dos escenarios esperados, adaptados de la corrida validada de `mateo` sobre este mismo input (traducidos a la nomenclatura de esta skill: "Ambiente" → "Entorno"):

## Escenario A — Rendimiento

| Parte | Valor |
|---|---|
| Fuente del estímulo | Usuarios externos (pacientes/público) que solicitan turnos |
| Estímulo | Llegada concurrente de pedidos de reserva de turnos |
| Artefacto | Módulo de reserva de turnos del sistema de gestión de turnos del hospital |
| Entorno | Pico de carga: concurrencia alta de solicitudes de turnos |
| Respuesta | El sistema procesa las solicitudes de reserva y devuelve una respuesta; no deja de atender |
| Medida de la respuesta | Latencia promedio de la reserva de turno ≤ 20 s en pico de carga *(Asunción: umbral estimado — el enunciado solo dice "que ande bien" y "si se cuelga nos matan", sin número; a diferencia del caso 05, acá la Respuesta y el Estímulo sí están claros, así que solo la Medida requiere asunción y puede inferirse — no aplica la restricción de "preguntar en vez de inventar" de Fuente/Estímulo/Respuesta)* |

## Escenario B — Seguridad

| Parte | Valor |
|---|---|
| Fuente del estímulo | Intruso humano, externo al hospital y no confiable |
| Estímulo | Intento de acceso no autorizado a los servicios del sistema de gestión de turnos |
| Artefacto | Módulo de autenticación y control de acceso del sistema de gestión de turnos |
| Entorno | Sistema abierto a una red |
| Respuesta | El sistema rechaza el acceso no autorizado y, tras intentos fallidos reiterados, bloquea nuevos intentos y notifica al administrador |
| Medida de la respuesta | *Asunción: bloqueo activado al 3.er intento fallido, con espera creciente de 20 min por ciclo — el enunciado solo pide "que no entren intrusos", sin cuantificar; se propone un valor a validar con el stakeholder.* |

## Qué falla si la skill NO separa el requerimiento

Si el resultado es un único escenario que mezcla "no debe colgarse" y "no deben entrar intrusos" bajo un solo atributo (típicamente forzado a Seguridad o a Disponibilidad), el escenario cae en el anti-patrón de `docs/condiciones/condiciones.md` de mezclar dos preocupaciones sin diferenciarlas — y además hace imposible priorizar cada una por separado en `arbol-utilidad` (tienen perfiles de Negocio/Técnica probablemente distintos).
