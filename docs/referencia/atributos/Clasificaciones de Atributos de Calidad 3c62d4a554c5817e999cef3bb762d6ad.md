# Clasificaciones de Atributos de Calidad

Los atributos de calidad no viven sueltos: distintos organismos y normas los agruparon bajo **taxonomías** —esencialmente, "nombres" o categorías estandarizadas— para poder manejarlos y comunicarlos como entidades reconocibles.

# ¿Para qué sirve clasificarlos?

1. **Que distintas personas/organizaciones hablen el mismo idioma**: si dos equipos dicen "eficiencia", "confiabilidad" o "usabilidad", se refieren al mismo concepto aunque trabajen en proyectos distintos.
2. **Comparar sistemas entre sí** usando el mismo marco de referencia.
3. **Evaluar y auditar sistemas de forma sistemática** (por eso son *normas*, provienen de organismos de estandarización).
4. **Servir como checklist** al analizar requerimientos, para no olvidarse de considerar ciertos aspectos de calidad.

# Las tres clasificaciones de la filmina

| **ISO 9126** | **Mitre** | **IEEE 1061** |
| --- | --- | --- |
| Efficiency | Efficiency | Efficiency |
| Functionality | Reliability | Functionality |
| Maintainability | Usability | Maintainability |
| Reliability | Maintainability | Portability |
| Portability | Expandability | Reliability |
| Usability | Interoperability | Usability |
| (+ distintos sub-factores) | Reusability |  |
|  | Integrity |  |
|  | Survivability |  |
|  | Correctness |  |
|  | Verifiability |  |
|  | Flexibility |  |
|  | Portability |  |
- **ISO 9126**: norma internacional (ISO) con 6 características principales, cada una con "sub-factores".
- **Mitre**: organización/contratista de investigación (ligada a proyectos de defensa y gobierno de EE. UU.) que propuso una lista más extensa.
- **IEEE 1061**: estándar del IEEE (organismo de ingeniería eléctrica/electrónica y de software).

# La idea clave: no hay una única clasificación "correcta"

Fijáte que **hay superposición entre las tres** (todas incluyen Efficiency, Reliability, Usability, Maintainability, Portability), pero **no son idénticas** — cada organismo agregó o quitó categorías según su propio enfoque y propósito.

<aside>
🧭

Esto es similar a cómo distintas disciplinas científicas a veces usan taxonomías levemente distintas para clasificar el mismo fenómeno: la realidad (que el software puede fallar, ser lento, ser difícil de cambiar, etc.) es la misma, pero el "nombre y la caja" en la que se mete cada problema puede variar según quién hizo la clasificación.

</aside>

# Ejemplos sueltos mencionados en la filmina

Además de las tres clasificaciones formales, la filmina tira una lista abierta de ejemplos de atributos de calidad que no siempre encajan prolijamente en una única norma:

- Performance
- Interoperabilidad
- Modificabilidad
- Desplegabilidad
- Seguridad
- Escalabilidad
- Estabilidad
- "Webifyability"
- Sustentabilidad
- …

Esto refuerza que las clasificaciones son herramientas de organización, no un catálogo cerrado ni definitivo.