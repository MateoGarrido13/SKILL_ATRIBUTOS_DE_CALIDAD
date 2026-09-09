# Descripción de la prueba — Chatbot y pagos

**Carpeta:** `docs/test/PRUEBA_DOS_CHATBOT_PAGOS/`  
**Skills bajo prueba:** i (Generador SEI) y ii (Validador SEI)

## Utilidad

Prueba **inicial** de **capacidad correctiva** y de respeto de templates, con inputs sucios a la vez (ver `DESC_PRUEBA_DOS.txt`):

1. Un `.md` en borradores **fuera de formato e incompleto** (nuevas funciones / pasarela de pago).
2. Por chat, un atributo **mal mapeado**: “Escalabilidad” para un chatbot disponible 24 hs.

El objetivo es ver si reclasifica, si rellena `template_escenarios.md` + `template_preguntas.md` / `template_sugerencias.md`, y si el primer relato sesga al segundo. Los `responsev1.md` y `responsev3.md` registran Generador y Validador; `requestv2.txt` cierra las marcas con datos del usuario.

## Resultados

| Input | Esperable | Lo que hizo |
|---|---|---|
| Chatbot 24 hs etiquetado como Escalabilidad | Disponibilidad (estar en pie), no crecimiento de carga | Reclasificó a **Disponibilidad**; no selló hasta fijar fuente, un solo ambiente y estadístico de los 5 min |
| Nuevas funciones + Mercado Pago | Integrabilidad (sumar pasarela) o Modificabilidad (agregar función) | Adoptó **Modificabilidad**; el Validador frenó la medida “8 módulos y/o artefactos” hasta unificar la unidad |

Sellados finales: `escenario_chatbot_24hs.md` (caída del proceso, mantenimiento, modo acotado &lt; 5 min al máximo) y `escenario_nuevas_funciones.md` (≤ 8 módulos de código existentes).

## Comentarios

**Positivo**
- Corrige el mapeo falso de Escalabilidad y explica por qué el estímulo no es crecimiento de carga.
- No sella con marcas: el Validador usa tres opciones por parte fallida (`responsev3.md`) y no adelanta la tabla final.
- Distingue “24 hs” (ventana de servicio) de un umbral de downtime.

**A mejorar**
- En el borrador de pago eligió Modificabilidad en vez de Integrabilidad; `DESC_PRUEBA_DOS.txt` lo atribuye a posible **sesgo del primer relato**.
- El chatbot quedó en un solo modo (mantenimiento); el enunciado mezclaba degradado, hora pico y 24 hs: la corrección es sana, pero se pierde cobertura de esos otros modos salvo que se abran escenarios aparte.
