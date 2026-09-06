
## 1. ESCENARIO DE ATRIBUTO DE CALIDAD (QAS) — 6 PARTES OBLIGATORIAS

Orden canónico de referencia: Fuente → Estímulo → Artefacto → Entorno → Respuesta → Medida de Respuesta.
Regla general: es aceptable omitir partes en fases tempranas de elicitación, pero el arquitecto DEBE verificar explícitamente la relevancia de las 6 antes de omitir alguna.

- **Fuente del Estímulo** (Source of Stimulus / Stimulus Source)
  - Definición: entidad (humana, sistema de software, hardware, infraestructura física, entorno físico) que genera el estímulo.
  - Regla: clasificar como interna/externa y confiable/no confiable cuando sea relevante, ya que esto puede alterar el tratamiento del estímulo.
  - Sinónimos válidos: "Fuente", "Origen", "Actor generador", "Source".

- **Estímulo** (Stimulus)
  - Definición: evento o condición que llega al sistema o al proyecto y dispara la necesidad de una respuesta.
  - Naturaleza según tipo de atributo: evento discreto en atributos de runtime (performance, disponibilidad, seguridad); acción motivadora en atributos de desarrollo (ej. solicitud de modificación, finalización de una unidad de código).
  - Sinónimos válidos: "Trigger", "Evento disparador", "Condición".

- **Artefacto** (Artifact)
  - Definición: parte del sistema/proyecto sobre la que incide el estímulo (sistema completo, subsistema, componente, colección de sistemas).
  - Regla: debe especificarse con la mayor granularidad posible; artefactos distintos pueden exigir respuestas/medidas distintas.
  - Sinónimos válidos: "Target", "Elemento afectado", "Objetivo del estímulo".

- **Entorno** (Environment)
  - Definición: estado o circunstancia operacional del sistema al momento del estímulo (operación normal, sobrecarga, arranque, apagado, modo degradado, en desarrollo, en pruebas, recarga de batería).
  - Regla: debe indicar un modo/estado concreto; la respuesta esperada puede variar según el entorno (ej. una falla antes vs. después de un code freeze).
  - Sinónimos válidos: "Ambiente", "Contexto operacional", "Estado del sistema", "Modo de operación".

- **Respuesta** (Response)
  - Definición: actividad(es) que el sistema (atributos de runtime) o los desarrolladores (atributos de desarrollo) deben ejecutar tras el estímulo. Es el compromiso de satisfacción del arquitecto.
  - Sinónimos válidos: "Comportamiento esperado", "Reacción del sistema".

- **Medida de Respuesta** (Response Measure) — CRÍTICO PARA VALIDACIÓN
  - Definición: criterio que permite medir la respuesta para determinar objetivamente si el escenario fue satisfecho (testeable).
  - **Criterios de validez (obligatorios, no negociables):**
    - Debe ser CUANTIFICABLE: unidad numérica explícita (tiempo, %, tasa, probabilidad, conteo, esfuerzo en persona-horas/días).
    - Debe permitir un veredicto binario pasa/no-pasa contra un umbral.
    - INVÁLIDO: adjetivos subjetivos sin umbral numérico ("rápido", "robusto", "fácil", "seguro", "óptimo").
  - Ejemplos de unidades válidas: latencia (ms/s), throughput (tx/s), % de disponibilidad, tiempo de detección/reparación de fallo, % de cobertura de código, persona-horas/días de esfuerzo, tasa de error, probabilidad de fuga de fallo.

## 2. DEFINICIÓN DE ASR (Architecturally Significant Requirement)
- Un requisito es ASR si y solo si cumple AMBAS condiciones:
  1. Tiene impacto profundo en la arquitectura (su inclusión cambiaría la arquitectura resultante).
     - **No es evaluable automáticamente por una skill: es un juicio del arquitecto.** La skill NO resuelve esta condición; su rol es REUNIR Y PRESENTAR EVIDENCIA para que el arquitecto decida.
     - Evidencia que la skill debe adjuntar: (a) qué componentes/artefactos del sistema quedan afectados; (b) qué tácticas arquitectónicas exige el escenario según la ficha del atributo correspondiente en `docs/referencia/atributos/`; (c) qué tradeoffs con otros atributos de calidad están involucrados (concepto definido en el índice maestro `Atributos de Calidad 3c62d4a554c581f5875de3690ef35a05.md`: mejorar un atributo típicamente empeora otro, ej. Performance vs. Seguridad).
     - La skill nunca emite por sí sola un veredicto sobre esta condición: entrega la evidencia, el arquitecto dictamina.
  2. Tiene alto valor de negocio/misión para stakeholders relevantes.
     - "Alto valor de negocio/misión" es exactamente la **Prioridad de Negocio / Business Value** de la hoja del Árbol de Utilidad (§3): no existe una segunda variable de valor de negocio con escala propia.
     - La condición se cumple cuando esa Prioridad es **H o M** (no solo H).
     - Esa etiqueta la asigna EXCLUSIVAMENTE el stakeholder, nunca la skill.
     - Consecuencia: el veredicto de ASR sólo puede emitirse DESPUÉS de que el stakeholder haya etiquetado el escenario, es decir en la etapa de priorización, no durante la generación del borrador.
- **Regla de omisión (no hay tercer estado):** mientras no exista la etiqueta de Prioridad del stakeholder, la skill OMITE el bloque de ASR por completo. No se emite vacío, no se emite como "no determinable" y no existe ninguna marca de indeterminación.
- El rótulo **ASR** se reserva para las hojas del árbol que efectivamente pasan AMBAS condiciones; no todo nodo hoja es un ASR (§3).

## 3. ÁRBOL DE UTILIDAD (Utility Tree) (DUPLICADO PARA ASEGURAR LA PUERTA DE ENTRADA)
- Construcción: la realiza el arquitecto cuando NO hay acceso directo a fuentes primarias de requisitos (stakeholders). Es un sustituto, no un reemplazo, de la elicitación directa.
- Nodo raíz fijo: **"Utility"** (expresión de la "bondad" global del sistema).

### Jerarquía ESTRICTA (top-down):
1. **Utility** (raíz, fija).
2. **Atributo de Calidad (QA)** — ej. Performance, Seguridad, Disponibilidad, Usabilidad, Modificabilidad, Configurabilidad, Testability. Es solo un placeholder intermedio (el nombre del QA aislado no es testeable).
3. **Refinamiento / Sub-atributo** — descomposición específica y relevante al sistema (ej. Performance → "Transaction response time" / "Throughput"; Disponibilidad → "No down time"; Seguridad → "Confidentiality" / "Resisting attacks"; Mantenibilidad → "Routine changes" / "Upgrades to commercial components").
4. **Escenario** (nodo hoja) — QAS completo, expresado narrativamente pero derivable a las 6 partes. El rótulo **ASR** NO es sinónimo de nodo hoja: queda reservado para las hojas que pasan el criterio de §2; hay hojas legítimas del árbol, por ejemplo etiquetadas `(L, L)`, que no son ASR.
5. **Prioridad de Negocio / Business Value** — escala {H, M, L}, asignada a la hoja.
   - H = requisito indispensable ("must-have"); omisión = riesgo de fracaso del proyecto.
   - M = importante; omisión no implica fracaso.
   - L = deseable; no justifica esfuerzo significativo.
6. **Dificultad Técnica / Technical Risk** — escala {H, M, L}, asignada a la hoja.
   - H = alto riesgo de no lograrlo (fuente de alerta activa para el arquitecto).
   - M = preocupante, riesgo no alto.
   - L = alta confianza de logro.
- Notación de hoja: `[Escenario] (Prioridad, Dificultad)` — ej. `(H, H)`, `(M, L)`.

### Reglas de inferencia post-construcción (determinísticas):
- QA o Refinamiento sin ningún Escenario asociado → NO es error automático; marcar como "revisar cobertura: posible escenario no capturado".
- Toda hoja `(H, H)` → clasificar como MÁXIMA PRIORIDAD de análisis arquitectónico y diseño de tácticas.
- Volumen alto de hojas `(H, H)` → señal de alerta de viabilidad/alcance del sistema.



