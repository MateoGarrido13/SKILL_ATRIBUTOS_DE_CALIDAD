# MarketHub — prueba integral del pipeline

Esta es la corrida que **posiciona el flujo frente al resto del grupo**: el mismo sistema (MarketHub), los mismos cinco enunciados vagos, las tres skills en orden. La diferencia no es “llegar a un árbol”, sino **quién decide qué**. Acá el stakeholder afirma medidas y H/M/L; la skill estructura, veta y presenta evidencia. No inventa umbrales ni dictamina ASR.

Conversación completa: [[cursor_use_case_scenario_audit]]. Artefactos: `Escenarios Refinados/`, `Copia de arbol_base.md`. Construcción de las reglas: `informe-garrido.md`.

---

## Fase 1 — Skill i (Generador)

**Input.** Contexto de la plataforma (PWA, panel en la nube, Mercado Pago / Stripe / PayPal, Andreani / DHL) más cinco frases de stakeholder, sin 6 partes ni números.

**Interacción.** El Generador no redacta un escenario “listo para arquitectura”. Entrega `[ESTADO: BORRADOR]`: tabla SEI, `[INFERIDO]` solo en Ambiente y Artefacto, `[INCOMPLETO]` en toda Medida no cuantificable, y pregunta (tres candidatas con unidad, umbral a cargo del usuario).

El primer enunciado (“conectarse a las APIs… si una cambia algo no se rompa el núcleo”) **no cabe en una sola hoja**: se parte en Integrabilidad (sumar/actualizar el proveedor) y Modificabilidad (acomodar el cambio de SDK). El de tarjetas en la BD y el de fuerza bruta al panel quedan como **dos Seguridades** (artefacto distinto). Salen **seis** borradores, no cinco.

El usuario cierra las marcas en serie: capa de integración y cota de 8 componentes; adaptadores y 2 semanas; interfaz web + Brave con la misma cota de tiempo; circuit breaker al 5.º intento / 30 min en la venta; doble factor facial + email en el panel; ramp-up del partner extra con cota de 5 módulos.

**Qué demuestra frente a un generador que “completa solo”.** El borrador es usable desde el primer turno *y* sigue siendo falso hasta que el dueño afirma. Lo que no está en el texto no entra como hecho.

**Fricción de esta fase.** El renderizado arrancó como Usabilidad; Portabilidad la afirmó el usuario. Un cambio de rumbo (25 s de pintura, circuit breaker en lugar de confidencialidad de la BD) se absorbió para dejar las seis casillas llenas: la crítica de coherencia no es el rol del Generador.

---

## Fase 2 — Skill ii (Validador)

**Input.** Los seis `.md` ya sin marcas, todavía en borradores; el usuario pide auditar de a uno.

**Interacción.** El Validador es veto, no redactor. No muestra “cómo quedaría” el escenario. O tres opciones concretas, o sello `[ESTADO: VALIDADO]` y el archivo vive una sola vez (aquí, copiado a `Escenarios Refinados/`).

| Hoja | Qué objetó | Qué eligió el usuario |
|---|---|---|
| Integrabilidad logística | Fuente y estímulo con `o` (stakeholder **o** proveedor; cobro **o** envío) | Opción 2: proveedor logístico, nueva versión de API de etiquetas |
| Modificabilidad SDK | Misma familia de `o` (pago **o** logística; protocolo **o** SDK) | Opción 2: Mercado Pago, cambio de protocolo, un adaptador |
| Portabilidad / render | Atributo vs medida (Portabilidad declarada, latencia de 25 s) | Opción 2: Directiva de incorporar Brave en diseño, ≤ 2 semanas |
| Seguridad venta | Estímulo vago, artefacto “la aplicación”, ambiente de negocio | Opción 2: fuerza bruta de sesión en el checkout; corte 5 / 30 min |
| Seguridad panel | “Acceso tras 2FA” sin unidad numérica | Opción 1: 100 % de rechazos si falta facial o email |
| Integrabilidad partner | “Ramp-up” no es modo operacional; “capa de adaptadores” demasiado ancha | Opción 2: Despliegue; adaptadores de catálogo e inventario; ≤ 5 módulos |

**Qué demuestra frente a un chequeo que solo puntúa “completo / incompleto”.** La ambigüedad no se resuelve eligiendo por el usuario: se enumera y se espera la letra. El sello documenta *qué opción se aplicó*.

**Fricción de esta fase.** El veto llega *después* de que el Generador ya cerró las seis partes con un cambio no previsto. La confidencialidad de tarjetas en reposo, desplazada por el circuit breaker, no se reconstruye sola: queda como hueco para el árbol.

---

## Fase 3 — Skill iii (Árbol de utilidad)

**Input.** Seis escenarios `[ESTADO: VALIDADO]`. Ninguno trae H/M/L.

**Interacción, compuerta cerrada.** Pedir el árbol **no lo construye**. Sale un bloque por hoja: qué implicaría H, M y L *en ese artefacto y esa medida*, las tres letras al mismo peso. El usuario etiqueta de a uno (o de a dos); mientras falte una celda, no hay tabla.

**Interacción, compuerta abierta.** Con las seis pares dichas, un único árbol (ISO 9126): refinamiento por fila, link al escenario, cobertura y evidencia de ASR **solo** en hojas con Prioridad H o M. El arquitecto dictamina el impacto; la skill no escribe “esto es ASR”.

| Refinamiento | (Negocio, Técnica) |
|---|---|
| Nueva versión de API logística | (M, H) |
| Cuarto partner de catálogo | (H, M) |
| Cambio de protocolo Mercado Pago | (M, L) |
| Incorporar Brave | (M, M) |
| Fuerza bruta en checkout | (L, M) |
| 2FA en el panel | (M, M) |

No hay `(H, H)`. El árbol marca cobertura faltante: cifrado en reposo, usabilidad de compra, disponibilidad/rendimiento del checkout.

**Qué demuestra frente a un árbol que auto-etiqueta.** La priorización es del stakeholder (la H técnica de logística la justificó él: discrepancias entre proveedores; la L de Mercado Pago, la documentación de la API). Un árbol grupal que asigne (H, M) por “dato financiero” o “checkout crítico” no es comparable con este: miden cosas distintas.

---

## Notas de analisis general

El valor de esta prueba no es la cantidad de hojas, es el **contrato de decisión**: Generador pregunta, Validador veta con tres caminos, Árbol espera las prioridades,quien compare outpus  debería contrastar (1) si el primer requerimiento se partió, (2) si hubo números no dichos por el usuario, (3) quién puso las H/M/L. Esos tres ejes separan este flujo de una corrida que llega al mismo dibujo de Utility con premisas distintas.


