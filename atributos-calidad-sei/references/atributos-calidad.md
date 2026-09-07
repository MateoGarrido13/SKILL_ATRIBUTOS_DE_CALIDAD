# Clasificaciones de Atributos de Calidad

Fuente: Díaz Pace, "Atributos de Calidad" (diapositiva 9); Keeling, Cap. 5
"Define the Quality Attributes" (pág. 51).

## Taxonomías clásicas

| ISO Std 9126 | IEEE Std 1061 | Mitre |
|---|---|---|
| Efficiency | Efficiency | Efficiency |
| Functionality | Functionality | Reliability |
| Maintainability | Maintainability | Usability |
| Reliability | Portability | Maintainability |
| Portability | Reliability | Expandability |
| Usability | Usability | Interoperability |
| | | Reusability |
| | | Integrity |
| | | Survivability |
| | | Correctness |
| | | Verifiability |
| | | Flexibility |
| | | Portability |

## Clasificación de Keeling (Software Architecture in Practice)

| Propiedades de tiempo de diseño | Propiedades de runtime | Propiedades conceptuales |
|---|---|---|
| Modifiability | Availability | Manageability |
| Maintainability | Reliability | Supportability |
| Reusability | Performance | Simplicity |
| Testability | Scalability | Teachability |
| Buildability / Time-to-Market | Security | |

## Notas por atributo (bibliografía provista)

- **Performance**: latencia y throughput bajo condiciones normales de
  operación.
- **Escalabilidad** (diapositiva 18-19): impacto de agregar/remover
  recursos de TI, en 3 aspectos: Capacidad (volumen de datos que maneja),
  Tiempo de respuesta (qué pasa si el sistema escala, a diferencia de
  Performance que mide el tiempo de respuesta en sí), y Throughput
  (unidades de trabajo por período de tiempo).
- **Interoperabilidad** (diapositiva 20): capacidad de que 2 sistemas
  intercambien información mediante sus interfaces, en niveles sintáctico,
  de APIs y semántico. Los mecanismos pueden conocerse de antemano o no.
- **Disponibilidad, Seguridad, Usabilidad**: ver ejemplos concretos en
  `plantilla-6-partes.md`.

## Cómo se usan los atributos de calidad (diapositiva 7)

En licitaciones, liderando un grupo, al analizar un requerimiento,
evaluando/eligiendo arquitecturas o herramientas, complementando
metodologías ágiles, y para comunicar decisiones entre stakeholders y
equipo.
