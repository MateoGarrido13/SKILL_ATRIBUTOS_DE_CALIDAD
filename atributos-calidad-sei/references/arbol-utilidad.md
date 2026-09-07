# Árbol de Utilidad (Utility Tree)

Fuente: Díaz Pace, "Atributos de Calidad" (diapositiva 22, tomado de Kazman
y Cervantes, 2016); Keeling, Activity 6 "Quality Attribute Web" (pág. 207)
y Activity 7 "Mini-Quality Attribute Workshop" (pág. 210).

## Estructura

El árbol parte de la raíz "Utility" y se ramifica en:

```
Utility
│
└── <Atributo de Calidad> (ej. Performance, Usability, Security, Availability)
      │
      └── <Refinamiento / sub-característica> (ej. Latency, Peak load,
           Learnability, Feedback, SW failure, Authentication, Audit trail)
            │
            └── Escenario concreto → Calificación (Importancia, Dificultad)
```

En el ejemplo de la diapositiva 22, cada escenario se anota con un par
`(Importancia, Dificultad)` usando valores High/Medium/Low, por ejemplo:
`Latency (M, M)`, `Peak load (H, H)`, `Authentication (H, M)`.

## Tabla de escenarios y casos de uso asociados

Alternativamente (diapositiva 23, Kazman y Cervantes), se puede presentar
como tabla plana:

| ID | Atributo de Calidad | Escenario | Caso de uso asociado |
|----|---|---|---|
| QA-1 | Performance | ... | UC-2 |
| QA-2 | Modifiability | ... | UC-5 |
| QA-3 | Availability | ... | All |

## Cómo calificar Importancia / Dificultad

- Si el usuario no da las calificaciones, sugerir valores basados en el
  impacto típico descripto en la bibliografía (por ejemplo, escenarios de
  manejo de errores y confianza del usuario suelen calificarse con
  Importancia Alta) y **aclarar siempre que son sugerencias a validar**.
- Para priorizar entre varios escenarios en grupo, la bibliografía sugiere
  técnicas complementarias:
  - **Dot voting** (Keeling, pág. 208, 211): cada participante reparte
    votos entre los escenarios/atributos.
  - **Choose One Thing** (Activity 1, pág. 192): forzar una elección
    binaria entre dos atributos en tensión para detectar prioridades reales.
  - **Quality Attribute Web** (Activity 6, pág. 207): radar chart con los
    atributos de calidad en los ejes y los escenarios como notas
    adhesivas ubicadas cerca del atributo al que corresponden.

## Notas sobre tradeoffs

Los atributos de calidad pueden entrar en conflicto entre sí (Díaz Pace,
diapositiva 10): Performance vs. Seguridad, Seguridad vs. Disponibilidad,
Performance vs. Modificabilidad, etc. El objetivo no es maximizar todos,
sino lograr un sistema "good enough" para los stakeholders. Al ubicar un
escenario en el árbol, vale la pena señalar si compite con otro atributo ya
registrado.
