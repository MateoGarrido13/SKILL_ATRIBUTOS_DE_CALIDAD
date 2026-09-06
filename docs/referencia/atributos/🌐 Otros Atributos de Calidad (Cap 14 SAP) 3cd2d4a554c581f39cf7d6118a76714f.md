# 🌐 Otros Atributos de Calidad (Cap. 14 SAP)

> Fuente: *Software Architecture in Practice* (4th ed.), Bass/Clements/Kazman, Cap. 14 — Working with Other Quality Attributes. Este capítulo no trae una tabla de escenario general propia (no es un atributo único), sino una discusión sobre otros atributos que el libro menciona sin dedicarles capítulo, más cómo trabajar con listas estándar de atributos de calidad. Se agrega acá como complemento, porque puede servir para reconocer un atributo en un escenario que no encaje en los 10 anteriores.
> 

# Atributos de calidad de la arquitectura misma

A diferencia de los demás (que califican el comportamiento del sistema en ejecución), estos miden la **arquitectura como artefacto de desarrollo**:

- **Buildability**: qué tan bien se presta la arquitectura a un desarrollo rápido y eficiente; se mide en costo (dinero o tiempo) de convertir la arquitectura en un producto funcionando que cumpla sus requerimientos.
- **Integridad conceptual**: consistencia del diseño a lo largo de toda la arquitectura ("lo mismo se hace de la misma forma en todos lados"); mejora la comprensibilidad y reduce confusión. Ej.: todos los componentes deberían loguear errores, manejar excepciones y sanitizar datos de la misma manera.
- **Marketability**: la percepción/reputación que trae consigo una arquitectura (ej. la presión de usar cloud o microservicios "porque es lo que se usa", independientemente de si es la mejor opción técnica).

# Development Distributability

Qué tan bien soporta el sistema el desarrollo por **equipos distribuidos** (geográfica u organizacionalmente). Se logra diseñando subsistemas con bajo acoplamiento entre sí (tanto en código como en modelo de datos), para minimizar la coordinación necesaria entre equipos. La estructura arquitectónica y la estructura social/organizacional del proyecto deberían estar alineadas (Ley de Conway).

# Atributos de calidad del sistema físico

En sistemas embebidos (auto, avión, electrodoméstico), el software convive con requerimientos físicos: **peso, tamaño, consumo eléctrico, potencia de salida, emisiones, resistencia climática, duración de batería**, etc. La arquitectura del software puede tener un efecto directo sobre estos (ej. software ineficiente → requiere más memoria/procesador/batería → más peso/consumo/costo). Las mismas técnicas de escenarios sirven para especificar estos atributos de sistema, no solo los de software.

# Uso de listas estándar de atributos de calidad

Existen varias normas/listas de referencia (ninguna es "la correcta"; sirven como checklist). La más citada es **ISO/IEC 25010 (SQuaRE)**, que divide los atributos en un modelo de "calidad de producto" con estas categorías principales:

- Functional suitability (completitud, corrección, adecuación funcional)
- Performance efficiency (comportamiento temporal, utilización de recursos, capacidad)
- Compatibility (coexistencia, interoperabilidad)
- Usability (aprendizaje, operabilidad, estética de UI, accesibilidad)
- Reliability (madurez, disponibilidad, tolerancia a fallas, recuperabilidad)
- Security (confidencialidad, integridad, no repudio, responsabilidad, autenticidad)
- Maintainability (modularidad, reusabilidad, analizabilidad, modificabilidad, testeabilidad)
- Portability (adaptabilidad, instalabilidad, reemplazabilidad)

# Cómo usar esta página

Si un escenario no encaja claramente en Disponibilidad, Desplegabilidad, Eficiencia Energética, Integrabilidad, Modificabilidad, Rendimiento, Safety, Seguridad, Testeabilidad o Usabilidad, puede tratarse de uno de estos atributos "secundarios" (buildability, integridad conceptual, distribuibilidad del desarrollo) o de un atributo del sistema físico en el que corre el software — o simplemente de una combinación/tradeoff entre varios de los diez principales.