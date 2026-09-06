# Caso 01 — monopatines — output esperado de skill-generar-escenarios-calidad

> Fuente: ejercicio 3-B, escenario 2.1 de la resolución de Notion ("TP Atributos Calidad").

## Atributo de calidad identificado

**Interoperabilidad** — integración con Mercado Pago.

## Escenario (template de 6 partes)

| Parte | Valor |
|---|---|
| Fuente del estímulo | La API de Mercado Pago (sistema externo) |
| Estímulo | Se envía una solicitud de descuento de saldo o carga de crédito |
| Artefacto | Módulo/adaptador de integración con Mercado Pago |
| Entorno | Operación normal, con la cuenta de MP ya vinculada |
| Respuesta | El sistema envía y recibe correctamente la confirmación de la transacción, actualizando el saldo de la cuenta |
| Medida de la respuesta | % de transacciones procesadas sin error de comunicación (ej. 99.9%), en menos de 3 segundos *(valor asumido para este test)* |

## Escenario final (oración completa)

"Cuando la API de Mercado Pago envía una solicitud de descuento de saldo o carga de crédito, con la cuenta ya vinculada y en operación normal, el módulo de integración con Mercado Pago debe enviar y recibir correctamente la confirmación de la transacción, actualizando el saldo de la cuenta, con al menos 99.9% de las transacciones procesadas sin error de comunicación y en menos de 3 segundos *(valor asumido para este test)*."
