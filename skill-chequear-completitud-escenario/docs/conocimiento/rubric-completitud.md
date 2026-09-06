# Rúbrica de completitud — las 6 partes del template

Criterio **positivo** de aceptación para cada parte del template de 6 partes (ver `../../../shared/template-6-partes-sei.md`): qué la hace COMPLETA vs INCOMPLETA, de forma objetiva y verificable. Esto es distinto de `docs/condiciones/condiciones.md` (que lista errores a detectar) — acá el foco es el checklist positivo que una parte tiene que cumplir para aprobar.

Para cada parte: si no cumple el criterio de "completa", marcarla como incompleta y usar la sugerencia de esa fila para indicar cómo completarla.

## 1. Fuente del estímulo

| | Criterio |
|---|---|
| **Completa si** | Identifica un actor o entidad concreta y nombrable (un rol de usuario, un componente interno específico, un sistema externo específico, un atacante). |
| **Incompleta si** | Queda genérica ("el sistema", "alguien", "un evento") sin decir quién o qué específicamente lo origina. |
| **Sugerencia si falta** | Preguntar: ¿la fuente es interna o externa al sistema? ¿es una persona, un componente, otro sistema? Nombrarla con la mayor precisión que el contexto permita. |

## 2. Estímulo

| | Criterio |
|---|---|
| **Completa si** | Especifica una condición concreta y desafiante (un evento, carga, fallo, pedido o ataque puntual) que exige una respuesta observable del sistema. |
| **Incompleta si** | Describe una interacción vaga o el caso de uso base sin ninguna condición que ponga a prueba al sistema (ej. "interactuar con el sistema", "un usuario usa la app"). |
| **Sugerencia si falta** | Agregar la variable que estresa la funcionalidad: ¿y si hay muchos a la vez? ¿y si falla algo? ¿y si es un adversario? (ver `docs/condiciones/condiciones.md`, tip 3 y 6). |

## 3. Artefacto

| | Criterio |
|---|---|
| **Completa si** | Señala qué parte del sistema recibe o es afectada por el estímulo (todo el sistema, un componente, un módulo, una interfaz), aunque sea con precisión razonable para el nivel de detalle del enunciado. |
| **Incompleta si** | No hay ninguna mención, ni siquiera implícita, de qué parte del sistema está en juego. |
| **Sugerencia si falta** | Inferir el artefacto más probable según el estímulo (ver `heuristica-inferencia.md` de `skill-generar-escenarios-calidad`) y marcarlo como asunción si no está explícito. |

## 4. Ambiente (entorno)

| | Criterio |
|---|---|
| **Completa si** | Indica el estado del sistema en el momento del estímulo (operación normal, arranque, sobrecarga, modo degradado, tiempo de diseño/build/runtime, etc.), explícito o claramente inferible del contexto. |
| **Incompleta si** | No hay ninguna referencia al estado o momento en que ocurre el estímulo. |
| **Sugerencia si falta** | Si el enunciado no lo aclara, asumir "operación normal" solo si es razonable para el atributo en cuestión, y marcarlo como asunción. |

## 5. Respuesta

| | Criterio |
|---|---|
| **Completa si** | Describe una acción **observable desde afuera del sistema** que reacciona específicamente al problema planteado por el estímulo (no la operación funcional que el sistema haría de todas formas, y no un mecanismo interno de arquitectura). |
| **Incompleta si** | Es una acción puramente funcional sin conexión con el atributo (ver `docs/condiciones/condiciones.md`, tip 7), o describe una táctica técnica interna en vez de un resultado observable (tip 8). |
| **Sugerencia si falta** | Reformular en términos de lo que percibe quien generó el estímulo (usuario, operador, sistema externo), no de cómo el sistema lo resuelve por dentro. |

## 6. Medida de la respuesta

| | Criterio |
|---|---|
| **Completa si** | Es **cuantificable**: incluye un número, porcentaje, unidad de tiempo, tasa o umbral verificable (ej. "≤ 2 segundos", "99.9% de disponibilidad", "0% de pérdida de datos"). |
| **Incompleta si** | Usa solo un adjetivo o adverbio cualitativo sin número asociado (ej. "rápido", "sin degradar", "de forma segura", "en poco tiempo"). Un adjetivo cualitativo **nunca** es suficiente por sí solo, aunque el resto del escenario esté bien armado. |
| **Sugerencia si falta** | Pedir el número al usuario; si no está disponible, remitir a `heuristica-inferencia.md` de `skill-generar-escenarios-calidad` para proponer un valor razonable marcado explícitamente como asunción — nunca dejar la medida en un adjetivo suelto. |

## Veredicto final

- Un escenario es **completo** solo si las 6 partes individualmente cumplen su criterio de "completa".
- Si una o más partes son incompletas, el escenario es **incompleto**, y el output debe listar específicamente cuáles partes fallaron y aplicar la sugerencia correspondiente de cada una.
- La parte 6 (Medida de la respuesta) es la más común de encontrar incompleta en la práctica — priorizar su verificación.
