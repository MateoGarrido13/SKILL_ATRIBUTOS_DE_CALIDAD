# Caso 05 — MarketHub — expected para `generar-escenarios-calidad`

Referencia de clasificación acordada por el grupo (usada también en `otaño/informe_skill.md` y contrastada contra `mateo` y contra el propio informe de esta rama, `informe-skill-yaco-recroa.md`, sección 4.4).

| # | Requerimiento | Atributo esperado | Nota |
|---|---|---|---|
| 1 | Pago/logística — "si una cambia algo, no se rompa el núcleo" | **Integrabilidad** (visto como "poder conectarse con APIs de terceros" — sumar/combinar componentes externos) | ⚠️ Ambigüedad legítima y documentada: tanto `mateo` (Integrabilidad) como esta skill en su propia corrida (Modificabilidad) y `otaño` (Modificabilidad) leyeron el mismo texto con anclajes distintos. Ninguna de las dos lecturas inventa un hecho no mencionado — es un caso real de dos atributos con evidencia textual válida ("conectarse con APIs" ancla a Integrabilidad; "si una cambia algo, no se rompa" ancla a Modificabilidad). El paso 1.3 del `SKILL.md` exige declarar esta ambigüedad explícitamente en vez de elegir en silencio. |
| 2 | Renderizado multi-navegador/dispositivo | **Portabilidad** (sabor de Modificabilidad: aislar dependencias de plataforma de ejecución) | La skill debe explicitar por qué no es solo "Usabilidad" — el síntoma que percibe el usuario es de usabilidad, pero la causa arquitectónica raíz es portabilidad (mismo código en motores de renderizado distintos). |
| 3 | Cifrado de tarjetas/contraseñas | **Seguridad** | Coincide en las 3 corridas del grupo (`mateo`, `otaño`, y esta skill). Es el punto de mayor consenso del caso. **Atención:** en la corrida documentada de `mateo`, este escenario quedó fuera del árbol final (solo anotado como alerta de cobertura, nunca sellado) — es justamente el tipo de omisión que el chequeo de cobertura obligatorio de `arbol-utilidad/SKILL.md` (paso 6) está diseñado para no dejar pasar. |
| 4 | Fuerza bruta / auditoría (panel + cuentas) | **Seguridad** | Coincide en las 3 corridas. Evaluar si conviene separar en dos escenarios (panel de administración vs. cuentas de clientes) — `otaño` los separó explícitamente por tener artefactos distintos aunque compartan patrón de respuesta; es el criterio más riguroso y se recomienda replicarlo. |
| 5 | Sincronización de catálogo de partner | **Integrabilidad** | Coincide en las 3 corridas. |

## Medidas de respuesta — chequeo contra la regla de no-inventar

Bajo la regla nueva de `heuristica-inferencia.md` (Fuente/Estímulo/Respuesta no se infieren), el requerimiento 1 en particular **no debería resolverse con una Medida de la respuesta inventada de una sola tirada** (ej. "menos de 5 días-persona") sin antes preguntar qué tipo de cambio hace el proveedor (nueva versión de API vs. cambio de formato vs. baja de servicio) — ver `docs/conocimiento/heuristica-inferencia.md`, Ejemplo 3, que usa este mismo requerimiento como caso de referencia.
