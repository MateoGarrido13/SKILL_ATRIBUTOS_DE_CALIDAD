# Atributos de Calidad

> Clase 5 — Diseño de Sistemas de Software (J. Andrés Díaz Pace). Esta página y sus subpáginas desarrollan en profundidad los conceptos de la filmina de **Atributos de Calidad**: qué son, cómo se capturan, cómo se priorizan y qué métodos existen para relevarlos con los stakeholders.
> 

# ¿Qué son los Atributos de Calidad?

Los **atributos de calidad** son propiedades sistémicas de un producto de software, a través de las cuales los stakeholders juzgan la calidad de ese producto. No alcanza con que el sistema implemente la funcionalidad correcta — un sistema puede hacer exactamente lo que tiene que hacer y aun así ser un mal sistema si:

- "Anda lento"
- Permite que atacantes roben datos
- Está caído la mayor parte del tiempo
- No escala cuando crece la demanda
- Es difícil de modificar o integrar con otros sistemas

Cuando el sistema sí tiene bien definidos sus atributos de calidad, estos:

- Hablan sobre la **calidad esperada** del sistema
- Están definidos desde el **punto de vista de los stakeholders**
- Se definen de manera **precisa** (no ambigua)
- Permiten **"testear"** si el requerimiento de calidad se satisface o no

# No son operacionales: la clave para entenderlos

Esta es la diferencia central entre un requerimiento funcional y un atributo de calidad, y por eso conviene detenerse en ella.

Un **requerimiento funcional** es *operacional*: se puede traducir casi directamente en una operación o función del sistema. Por ejemplo, "el sistema debe permitir cancelar un pedido" se traduce en un método como `cancelarPedido()`. Podés señalar una línea de código, una pantalla, un botón, y decir "esto implementa ese requerimiento".

Un **atributo de calidad**, en cambio, *no se puede señalar en una sola parte del sistema* ni traducir en una función concreta. "El sistema debe ser seguro" o "el sistema debe ser rápido" no son cosas que se implementen en un módulo aislado — son **propiedades que emergen de cómo está construido el sistema como un todo**: de la arquitectura, de decisiones de diseño distribuidas en múltiples componentes, de la infraestructura, de cómo interactúan las partes entre sí.

<aside>
💡

**Ejemplo comparativo**

- "El sistema debe autenticar usuarios" → **operacional**: es una función concreta, `login(usuario, password)`.
- "El sistema debe ser seguro" → **no operacional**: no es una función, es una cualidad que depende de cómo se implementó la autenticación, el cifrado de datos, los logs de auditoría, el manejo de sesiones, la validación de inputs, etc. — todo distribuido por el sistema.
</aside>

Por no ser operacionales:

1. **No se pueden "codear" directamente** — no existe un método `serRapido()`. La performance surge de decisiones combinadas: el algoritmo elegido, la arquitectura de capas, el uso de cache, la base de datos, etc.
2. **Son transversales (cross-cutting)** — atraviesan todo el sistema en vez de vivir en un solo lugar. Por eso su alcance debe considerarse a lo largo del diseño, implementación y deployment (ver Resumen al final de esta página).
3. **Necesitan ser interpretados en contexto** — decir "el sistema debe ser rápido" no significa nada por sí solo. ¿Rápido comparado con qué? ¿En qué condición? ¿Cuánto es "rápido"? Por eso se necesita capturarlos como **escenarios** con una estructura de 6 partes (ver subpágina *Escenarios de Calidad*), que es la forma de tomar algo abstracto y no operacional y volverlo concreto, medible y testeable.

# El panorama de los Requerimientos de Software

Para diseñar un sistema se necesitan cuatro insumos:

- El **contexto** del sistema
- Los **requerimientos funcionales** → capturados como casos de uso o historias de usuario
- Los **requerimientos de atributos de calidad** → capturados como escenarios de atributos de calidad
- Las **restricciones** impuestas por el contexto o por los stakeholders (condiciones no negociables: tecnología obligatoria, infraestructura existente, plataformas soportadas, etc. — ver subpágina QAW)

Los atributos de calidad son no operacionales, y por lo tanto deben ser interpretados en contexto.

# ¿Cómo se usan los Atributos de Calidad en la práctica?

- En licitaciones
- Liderando un grupo o equipo
- Cuando se analiza un requerimiento
- Evaluando o eligiendo arquitecturas (o herramientas)
- Complementando metodologías ágiles
- Comunicando decisiones de diseño

# Tradeoffs (puntos de balance)

Los atributos de calidad pueden entrar en conflicto unos con otros. Algunos ejemplos típicos:

- Performance **versus** Seguridad
- Seguridad **versus** Disponibilidad
- Performance **versus** Modificabilidad

El objetivo del diseño **no** es maximizar cada atributo por separado — eso normalmente es imposible, porque mejorar uno empeora otro. El objetivo real es **evaluar múltiples atributos de calidad y diseñar un sistema que sea "good enough" (suficientemente bueno) para los stakeholders**, balanceando las tensiones entre ellos.

# Cómo se organiza esta clase (mapa de subpáginas)

<aside>
🗂️

Esta página es el punto de entrada. Las subpáginas profundizan cada tema:

- **Clasificaciones de Atributos de Calidad** — los distintos "nombres" y normas que categorizan los atributos (ISO 9126, Mitre, IEEE 1061).
- **Árbol de Utilidad** — la herramienta para priorizar qué atributos importan más en un proyecto concreto.
- **Escenarios de Calidad: el Template SEI de 6 partes** — cómo especificar con precisión un atributo ya priorizado, con sus propias subpáginas de ejercicios resueltos y de escalabilidad.
- **QAW – Quality Attribute Workshop** — el método facilitado que reúne todas estas técnicas para relevar atributos de calidad con los stakeholders, con sus propias subpáginas de dinámicas ágiles y de estimación de medidas de respuesta (straw man).
</aside>

# Resumen de la clase

- Funcionalidad y atributos de calidad son **ortogonales** (independientes entre sí).
- El alcance de los atributos de calidad debe ser considerado a lo largo del **diseño, implementación y deployment**.
- Los atributos de calidad normalmente **llevan a tradeoffs**.
- Existen los **escenarios** para relevar atributos de calidad.
- Los resultados satisfactorios dependen tanto de la **arquitectura** (big picture) como de la **correcta implementación** (los detalles).

[Clasificaciones de Atributos de Calidad](Clasificaciones%20de%20Atributos%20de%20Calidad%203c62d4a554c5817e999cef3bb762d6ad.md)

[Árbol de Utilidad](Árbol%20de%20Utilidad%203c62d4a554c5817cbb44ee62602cb06a.md)


[QAW – Quality Attribute Workshop](QAW%20–%20Quality%20Attribute%20Workshop%203c62d4a554c5816bb895fe558601795f.md)

[🌍 De la Facultad a la Realidad](🌍%20De%20la%20Facultad%20a%20la%20Realidad%203c62d4a554c5815bb5d3f0902eaf474c.md)

## Profundizacion de cada atributo

[🟢 Disponibilidad (Availability)](🟢%20Disponibilidad%20(Availability)%203cd2d4a554c581c7b286f2762597c849.md)

[🚀 Desplegabilidad (Deployability)](🚀%20Desplegabilidad%20(Deployability)%203cd2d4a554c58191a381c8cfdd7912b3.md)

[🔋 Eficiencia Energética (Energy Efficiency)](🔋%20Eficiencia%20Energética%20(Energy%20Efficiency)%203cd2d4a554c5818bacdde5b19e02be14.md)

[🔌 Integrabilidad (Integrability)](🔌%20Integrabilidad%20(Integrability)%203cd2d4a554c581c8ac39e8deecc26f24.md)

[🧩 Modificabilidad (Modifiability)](🧩%20Modificabilidad%20(Modifiability)%203cd2d4a554c581299ffff22ff5b8e181.md)

[⚡ Rendimiento (Performance)](⚡%20Rendimiento%20(Performance)%203cd2d4a554c5816696ddec7ac7d67ec5.md)

[⛑️ Safety (Seguridad Operacional)](⛑️%20Safety%20(Seguridad%20Operacional)%203cd2d4a554c581b6b46df286b5edbbaa.md)

[🔒 Seguridad (Security)](🔒%20Seguridad%20(Security)%203cd2d4a554c581ee86fce7d3e52e863f.md)

[🧪 Testeabilidad (Testability)](🧪%20Testeabilidad%20(Testability)%203cd2d4a554c5810a8d04d6b0a5637832.md)

[👥 Usabilidad (Usability)](👥%20Usabilidad%20(Usability)%203cd2d4a554c581c39ef9ee40a3fca9b5.md)

[🌐 Otros Atributos de Calidad (Cap. 14 SAP)](🌐%20Otros%20Atributos%20de%20Calidad%20(Cap%2014%20SAP)%203cd2d4a554c581f39cf7d6118a76714f.md)
