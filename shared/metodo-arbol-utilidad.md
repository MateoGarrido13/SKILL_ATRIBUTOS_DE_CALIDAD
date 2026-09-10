# Árbol de Utilidad (Utility Tree)

> Fuente: página de Notion "Árbol de Utilidad" (subpágina de "Atributos de Calidad"), migrada casi íntegra. Basado en Clase 5 — Diseño de Sistemas de Software (J. Andrés Díaz Pace) y en Kazman y Cervantes, 2016.

El **árbol de utilidad** (*utility tree*) es una herramienta de **priorización jerárquica** para organizar los atributos de calidad de un sistema, yendo de lo más general a lo más específico y concreto. Es la técnica que responde a la pregunta "¿cuáles atributos importan y cuánto, en MI proyecto puntual?", **antes** de especificar cada uno con el template de 6 partes (ver `template-6-partes-sei.md`).

(Diagrama tomado de Kazman y Cervantes, 2016.)

## Cómo se estructura

Tiene 4 niveles, de izquierda a derecha:

1. **Utility** (raíz): la "utilidad" general del sistema — el nodo de partida.
2. **Atributos de calidad** (nivel 2): Performance, Usability, Availability, Security, etc. — las categorías generales, las mismas que aparecen en las clasificaciones estándar (ver sección "Clasificaciones de Atributos de Calidad" en `glosario-atributos-calidad.md`).
3. **Sub-atributos o refinamientos** (nivel 3): cada atributo se desglosa en aspectos más concretos. Por ejemplo:
   - Performance → Latency, Peak load
   - Usability → Learnability, Feedback
   - Availability → SW failure, Network failure
   - Security → Authentication, Audit trail
4. **Escenarios concretos** (nivel 4, las hojas del árbol): cada sub-atributo se traduce en uno o más escenarios específicos y medibles — acá es exactamente donde se conecta con el template de 6 partes. Por ejemplo, bajo "Latency": *"User displays time server event history. The list of events from the last 24 hours is displayed within 1 second."*

## Las etiquetas de priorización (H, M, L)

Cada hoja del árbol tiene un par de letras entre paréntesis, por ejemplo **(H, H)** o **(M, L)**:

- La **primera letra** = importancia / impacto en el negocio (*business importance*)
- La **segunda letra** = dificultad técnica de implementación (*technical risk/difficulty*)
- Valores posibles: **H (High), M (Medium), L (Low)**

> **Ejemplos del árbol de la filmina**
> - *"A failure occurs in the management system. The management system resumes operation in less than 30 seconds"* → **(H, H)**: muy importante para el negocio Y muy difícil de lograr técnicamente — candidato prioritario de atención arquitectural.
> - *"A new user can configure their account in less than 8 hours of training"* → **(L, L)**: poco importante y poco riesgoso — se puede posponer en el diseño arquitectural inicial.

## Para qué sirve en la práctica

1. Partir de lo genérico (Utility) y bajar sistemáticamente hasta escenarios concretos y medibles.
2. **Priorizar** dónde poner el esfuerzo de diseño/arquitectura: los escenarios (H,H) son los que más atención necesitan; los (L,L) casi se pueden ignorar en el diseño arquitectural inicial.
3. Servir de **input directo** para los escenarios formales del template de 6 partes — cada hoja del árbol ya es, en esencia, un mini-escenario que después se expande en las 6 partes completas.

## La relación exacta entre árbol de utilidad y template de 6 partes

Esta distinción es clave y suele confundirse:

| | **Árbol de utilidad** | **Template de 6 partes** |
|---|---|---|
| Qué me dice | **CUÁLES** son mis atributos de calidad relevantes, y cuánto importa cada uno | **QUÉ SIGNIFICA CONCRETAMENTE** un atributo que ya identifiqué como importante |
| Tipo de trabajo | **Priorización** (con QAW, entrevistas a stakeholders) | **Especificación** (una vez elegido el escenario relevante) |

El orden real del proceso es:

1. **Primero** se identifican/priorizan los atributos relevantes para el proyecto (con QAW, entrevistas, árbol de utilidad) → responde "¿cuáles son mis atributos de calidad relevantes?"
2. **Después**, para cada atributo ya priorizado, se usa el template de 6 partes (adaptado a ese atributo) para especificarlo con precisión → responde "¿qué significa exactamente ESTE atributo en MI sistema, y cómo lo verifico?"
3. El resultado final es el **escenario concreto**, ya testeable, con una medida de respuesta cuantificable.

El árbol de utilidad **no reemplaza** el template ni descubre por sí solo qué atributos existen (eso ya se sabe de antemano por las clasificaciones estándar) — su función es **priorizar** entre lo que ya se sabe que existe, para no gastar esfuerzo especificando con el mismo detalle algo (L,L) que algo (H,H).
