# QAW – Quality Attribute Workshop

El **QAW (Quality Attribute Workshop)** es el método que responde a la pregunta de fondo: ¿cómo identifico cuáles son los atributos de calidad importantes para MI proyecto, **antes** de empezar a diseñar la arquitectura?

> Un método facilitado que involucra a los stakeholders del sistema desde etapas tempranas en el ciclo de vida, a fin de descubrir los atributos de calidad clave para un sistema.
> 

# Puntos clave del método

- **Centrado en el sistema**
- **Focalizado en los stakeholders**
- Se utiliza normalmente **antes de que se construya la arquitectura**

# ¿Cuándo se realiza un QAW?

- En la **fase inicial** de un enfoque centrado en arquitectura → llamada **Inception**.
- Es **compatible con métodos ágiles** como Scrum, típicamente como el "Sprint 0" (antes de arrancar a codear).
- También aplica a **sistemas legados (brownfield)**, donde ya existe una arquitectura previa — ahí el QAW sirve para comparar el sistema **as-is** (como está hoy) contra el **to-be** (cómo debería quedar), y planear la evolución.

# Configuración típica del workshop

*(Diagrama tomado y traducido de A. Lattanze, CMU, 2013.)*

El QAW **no es solo una entrevista informal** — es un evento estructurado, con roles definidos y captura de información en tiempo real, visible para todos los participantes.

**Roles presentes:**

- **Customer Stakeholders**: el grupo de stakeholders del cliente
- **Facilitator**: quien conduce la sesión, hace las preguntas, mantiene el foco
- **Remaining Architecture Design Team**: el resto del equipo de arquitectura, que participa/escucha
- **Electronic Scribe**: registra digitalmente lo que se discute
- **Flip Chart Scribe**: anota a mano en un rotafolio/pizarra

**Materiales visuales expuestos en la sala** (carteles donde se documenta en vivo):

- **Operational Descriptions**: descripciones de cómo opera el sistema
- **Quality Attributes**: los atributos de calidad que van surgiendo
- **Constraints**: las restricciones del sistema
- **Agenda**: la agenda de la sesión
- **Parking Lot**: espacio para "estacionar" temas que surgen pero no son el foco del momento, para no perder el hilo de la discusión

También se usa un **proyector/beamer** para mostrar contenido en pantalla.

# Los 3 outputs que produce un QAW

1. **Árbol de utilidad** — los atributos identificados, priorizados jerárquicamente (ver subpágina dedicada).
2. **Escenarios y casos de uso** — tanto escenarios de calidad como requerimientos funcionales se relevan en paralelo, no por separado (ver subpágina *Escenarios de Calidad*).
3. **Captura de restricciones (constraints)** — condiciones no negociables que limitan el espacio de diseño.

## Ejemplo real de restricciones capturadas en un QAW

| **ID** | **Restricción (Constraint)** |
| --- | --- |
| CON-1 | Debe soportarse un mínimo de 50 usuarios simultáneos. |
| CON-2 | El sistema debe accederse vía navegador web (Chrome V3.0+, Firefox V4+, IE8+) en Windows, OSX y Linux. |
| CON-3 | Debe usarse un servidor de base de datos relacional existente, exclusivo para esa base. |
| CON-4 | La conexión de red a las estaciones de usuario puede tener bajo ancho de banda, pero es generalmente confiable. |
| CON-5 | Los datos de performance deben recolectarse en intervalos no mayores a 5 minutos. |

Las restricciones son **distintas** de los atributos de calidad: no son negociables ni "se optimizan" — son condiciones fijas del contexto que el arquitecto debe respetar sí o sí al diseñar. Junto con los requerimientos funcionales y de atributos de calidad, forman el conjunto completo de insumos para diseñar (ver página raíz *Atributos de Calidad*).

# En qué se profundiza en las subpáginas

<aside>
🗂️

- **Dinámicas Ágiles para QAW** — versión liviana del método (entrevistas a stakeholders, Mini-QAW, Stakeholder Map), según Michael Keeling.
- **Straw Man y Estimación de Medidas de Respuesta** — cómo se estima con fundamento (y no "a ciegas") el número que va en la Medida de Respuesta de un escenario.
</aside>

[🤝 Dinámicas Ágiles para QAW](🤝%20Dinámicas%20Ágiles%20para%20QAW%203c62d4a554c581cca616e4b3b3176d83.md)

[🪨 Straw Man y Estimación de Medidas de Respuesta](🪨%20Straw%20Man%20y%20Estimación%20de%20Medidas%20de%20Respuesta%203c62d4a554c581da8bc5ca08502b1ecf.md)