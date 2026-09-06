# Caso 03 (SAP — Performance) — output esperado de skill-generar-escenarios-calidad

> Fuente: Software Architecture in Practice (Bass/Clements/Kazman), Figura 9.1, "example concrete performance scenario". Escenario ya validado por los autores — no hay valores asumidos en este caso.

## Atributo de calidad identificado

**Rendimiento (Performance)** — pico de solicitudes concurrentes en operación normal.

## Escenario (template de 6 partes)

| Parte | Valor |
|---|---|
| Fuente del estímulo | 500 usuarios |
| Estímulo | Inician 2000 solicitudes en un intervalo de 30 segundos |
| Artefacto | El sistema (System) |
| Entorno | Operación normal |
| Respuesta | El sistema procesa todas las solicitudes |
| Medida de la respuesta | Latencia promedio de 2 segundos |

## Escenario final (oración completa)

"Quinientos usuarios inician 2000 solicitudes en un intervalo de 30 segundos, bajo operación normal, y el sistema procesa todas las solicitudes con una latencia promedio de dos segundos."
