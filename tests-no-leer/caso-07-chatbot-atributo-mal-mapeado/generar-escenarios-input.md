# Caso 07 — Chatbot, atributo mal mapeado por el usuario — input para `generar-escenarios-calidad`

Caso adversarial tomado de la rama `mateo` (`docs/test/PRUEBA_DOS_CHATBOT_PAGOS/`). Objetivo del caso: verificar que la skill **no acepta pasivamente** la etiqueta de atributo que da el usuario cuando el propio texto describe otra cosa, y que además **pregunta en vez de inventar** cuando falta el evento concreto (Estímulo) — el caso más directo para ejercitar la regla nueva de `heuristica-inferencia.md` (Fuente/Estímulo/Respuesta no se infieren).

## Input (tal cual, dos requerimientos en el mismo mensaje)

1. Un borrador incompleto y fuera de formato:

   "Para el escenario de las nuevas funciones: incluye pasarelas externas como 'Mercado Pago'. Se agrega un módulo o artefacto que debe modificarse bajo el umbral de 8 de estos módulos, en tiempo de diseño antes de producción."

2. Por chat, con el atributo ya (mal) etiquetado por el usuario:

   "Escalabilidad: El sistema debe tener un asistente virtual (chatbot) disponible las 24hs."
