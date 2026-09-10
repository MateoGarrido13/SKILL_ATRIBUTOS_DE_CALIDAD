# Caso 02 (SAP — Disponibilidad) — output esperado de skill-generar-escenarios-calidad

> Fuente: Software Architecture in Practice (Bass/Clements/Kazman), Figura 4.1, "Sample concrete availability scenario". Escenario ya validado por los autores — no hay valores asumidos en este caso.

## Atributo de calidad identificado

**Disponibilidad (Availability)** — falla de servidor en operación normal.

## Escenario (template de 6 partes)

| Parte | Valor |
|---|---|
| Fuente del estímulo | Un servidor (dentro de una granja de servidores / server farm) |
| Estímulo | El servidor falla |
| Artefacto | El servidor (Server) |
| Entorno | Operación normal |
| Respuesta | El sistema informa al operador y continúa operando |
| Medida de la respuesta | Sin tiempo de inactividad (*no downtime*) |

## Escenario final (oración completa)

"Un servidor de una granja de servidores falla durante la operación normal, y el sistema informa al operador y continúa operando sin ningún tiempo de inactividad."
