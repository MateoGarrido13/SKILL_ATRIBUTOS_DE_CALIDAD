### 🎯 Falta Priorizar: el Árbol de Utilidad Queda Pendiente

Tus escenarios ya están cerrados y verificables, pero el árbol ordena el esfuerzo de diseño según **cuánto vale cada escenario para los stakeholders** y **cuánto riesgo técnico tiene lograrlo**. Esas dos decisiones son tuyas: sin ellas no hay priorización, así que todavía no construyo el árbol.

Para cada escenario necesito dos letras. Te explico qué implicaría cada nivel **a la hora de diseñar**, no solo qué quiere decir "High" o "Low".

---

**Escenario [N] — [Atributo de Calidad]:** [Resumen en una línea: qué componente se estimula, en qué condición de operación y cuál es la medida comprometida].

**1. Prioridad para los stakeholders (importancia de negocio):** ¿cuánto vale para el negocio que *este* escenario se cumpla con esa medida?

- **H (High)** — Es indispensable, un *must-have*: si el sistema se entrega sin cumplir esa medida, el proyecto está en riesgo de fracasar. En el diseño esto habilita gastar esfuerzo de arquitectura y tácticas dedicadas en [artefacto involucrado], y lo vuelve candidato a requisito arquitectónicamente significativo.
- **M (Medium)** — Es importante y se quiere lograr, pero omitirlo no hace fracasar al proyecto. Entra igual al análisis de arquitectura, aunque compite por esfuerzo con los escenarios H.
- **L (Low)** — Es deseable: no justifica esfuerzo significativo. No se especifica con el mismo detalle que un escenario prioritario y puede resolverse más adelante.

**2. Complejidad técnica (riesgo de implementación):** ¿cuánta confianza tenés de que el equipo alcance esa medida en [artefacto involucrado]?

Más allá de "es difícil" o "es fácil", la letra que elijas fija **cuánto condiciona este escenario al diseño**:

- **H (High)** — Riesgo alto de no lograrlo: **alerta activa para el arquitecto**. En el diseño significa que el escenario no se acomoda a lo ya decidido: va a condicionar decisiones estructurales, exige tácticas específicas de [atributo] (no un mecanismo genérico) y obliga a negociar *tradeoffs* — reforzar [atributo] acá típicamente degrada otro atributo, y esa tensión pasa a ser parte del diseño. Si además la prioridad es H, el escenario queda como máxima prioridad de análisis arquitectónico y diseño de tácticas; varias hojas así son señal de alerta de viabilidad o alcance.
- **M (Medium)** — Preocupa, pero el riesgo no es alto. Se aborda con tácticas conocidas de [atributo]; conviene dejar el punto vigilado porque un cambio de contexto puede subirlo a H.
- **L (Low)** — Alta confianza de logro. En el diseño significa que el escenario **no arrastra decisiones de arquitectura**: se cumple con los mecanismos ya previstos, no obliga a rediseñar [artefacto involucrado] ni a negociar contra otros atributos, y puede postergarse en el diseño arquitectónico inicial.

---

*Respondeme las dos letras por escenario —por ejemplo "Escenario 1: prioridad M, complejidad H"— y con eso construyo el árbol de utilidad. Si dudás entre dos niveles, decime la duda y la dejamos anotada; no elijo por vos.*
