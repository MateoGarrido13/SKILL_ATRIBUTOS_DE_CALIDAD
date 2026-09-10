# Caso 04 (SAP — Árbol de Utilidad, Healthcare) — output esperado de skill-arbol-utilidad

> **Aclaración importante:** estas etiquetas H/M/L son las **originales del libro** (Software Architecture in Practice, Bass/Clements/Kazman, Tabla 19.1), **no fueron derivadas por nuestra skill** — se usan para comparar si el criterio de la skill (`docs/conocimiento/criterio-priorizacion.md`) coincide con el de los autores, a diferencia de `caso-01-monopatines`, donde la priorización sí es nueva y propia.

## Árbol de utilidad

```
Utility (sistema del ámbito de salud / healthcare)
│
├── Rendimiento (Performance)
│   ├── Tiempo de respuesta de transacciones (H, H)
│   └── Throughput (M, M)
│
├── Usabilidad (Usability)
│   ├── Entrenamiento de competencia (M, L)
│   └── Eficiencia de las operaciones (M, M)
│
├── Configurabilidad (Configurability)
│   └── Configurabilidad de datos (H, L)
│
├── Mantenibilidad (Maintainability)
│   ├── Cambios de rutina — escenario 1 (H, M)
│   ├── Cambios de rutina — escenario 2 (M, L)
│   ├── Actualización de componentes comerciales (H, M)
│   └── Agregar una nueva funcionalidad (M, M)
│
├── Seguridad (Security)
│   ├── Confidencialidad (H, M)
│   └── Resistencia a ataques (H, M)
│
└── Disponibilidad (Availability)
    ├── Sin tiempo de inactividad (H, L)
    └── Acceso web 24/7/365 (M, M)
```

## Tabla completa (fuente: Tabla 19.1 del libro)

| Atributo | Refinamiento | Importancia negocio | Dificultad técnica |
|---|---|---|---|
| Rendimiento | Tiempo de respuesta de transacciones | H | H |
| Rendimiento | Throughput | M | M |
| Usabilidad | Entrenamiento de competencia | M | L |
| Usabilidad | Eficiencia de las operaciones | M | M |
| Configurabilidad | Configurabilidad de datos | H | L |
| Mantenibilidad | Cambios de rutina — escenario 1 | H | M |
| Mantenibilidad | Cambios de rutina — escenario 2 | M | L |
| Mantenibilidad | Actualización de componentes comerciales | H | M |
| Mantenibilidad | Agregar una nueva funcionalidad | M | M |
| Seguridad | Confidencialidad | H | M |
| Seguridad | Resistencia a ataques | H | M |
| Disponibilidad | Sin tiempo de inactividad | H | L |
| Disponibilidad | Acceso web 24/7/365 | M | M |

## Uso de este caso

Este caso sirve como **benchmark externo**: si la skill, al recibir `arbol-utilidad-input.md` (sin etiquetas) y aplicar su propio `criterio-priorizacion.md`, llega a etiquetas distintas de las de esta tabla, no es necesariamente un error — pero vale la pena revisar la justificación en esos casos puntuales para entender si la skill está interpretando el dominio de salud de forma razonable o si su criterio necesita ajuste.
