# Caso 05 — MarketHub — expected para `arbol-utilidad`

Este caso es el que motivó la excepción de "dependencias de terceros no controladas por el equipo" agregada a `docs/conocimiento/criterio-priorizacion.md`. Documentado en `informe-skill-yaco-recroa.md`, secciones 2 (Problema 2), 3 (Limitación ④) y 4.4.

## Referencia del grupo (ground truth acordado)

| Escenario | (Negocio, Técnica) referencia |
|---|---|
| 1. Pago/logística (proveedor cambia SDK/protocolo) | **(M, H)** |
| 2. Renderizado cross-browser/dispositivo | **(M, M)** |
| 3. Cifrado de datos financieros | **(H, M)** |
| 4. Fuerza bruta / auditoría | **(H, L)** |

## Resultado ANTES de la corrección de esta rama (documentado, no repetir)

| Escenario | Skill (antes de la excepción de terceros) | Coincide con la referencia |
|---|---|---|
| 1. Pago/logística | (H, M) | ❌ — invertido en los dos ejes |
| 2. Renderizado | (H, M) | ❌ — negocio sobreestimado |
| 3. Cifrado de datos | (H, M) | ✅ |
| 4. Fuerza bruta | (H, M) | ⚠️ — negocio correcto, técnica sobreestimada |

Causa raíz documentada: el "principio rector" original de `criterio-priorizacion.md` hacía que la medida de esfuerzo cuantificada del propio escenario (ej. "5 días-persona") pesara más que el riesgo de depender de un proveedor externo no controlado por el equipo — confirmado también contra el benchmark del libro SAP (sistema de salud, Tabla 19.1), donde el mismo patrón apareció en el caso "Upgrade de componente COTS": (M, H) en el libro vs. (M, L)/(H, M) según la corrida.

## Comportamiento esperado DESPUÉS de la excepción agregada

Para el escenario 1 (dependencia de un proveedor de pago/logística externo), la excepción de `criterio-priorizacion.md` debe llevar el eje Técnico a **H** (o como mínimo M, nunca inferior), acercándolo a la referencia (M, H), en vez de bajarlo a M/L solo porque el escenario ya trae una medida de esfuerzo propia ("5 días-persona").

## Nota de cobertura (relacionada, ver `arbol-utilidad/SKILL.md`, paso 6)

En la corrida documentada de la rama `mateo` sobre este mismo caso, el escenario de cifrado de datos financieros (equivalente al #3 de esta tabla) quedó **fuera del árbol final**, señalado solo como alerta de cobertura sin llegar a sellarse. El chequeo de cobertura obligatorio agregado a `arbol-utilidad/SKILL.md` existe específicamente para que ese tipo de omisión no se repita en esta rama: si una preocupación del enunciado original no tiene escenario asociado, el árbol no puede presentarse como terminado.
