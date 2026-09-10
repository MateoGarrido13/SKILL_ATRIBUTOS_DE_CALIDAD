# Caso 01 — monopatines — output esperado de skill-arbol-utilidad

> **Aclaración importante:** las etiquetas H/M/L de este árbol son una priorización **nueva**, elaborada para este test aplicando `docs/conocimiento/criterio-priorizacion.md`. La resolución original en Notion (ejercicio 3) no incluye un árbol de utilidad priorizado con H/M/L — solo la identificación de los 8 escenarios candidatos y sus stakeholders. No tomar estas etiquetas como parte de la resolución histórica del TP.

## Árbol de utilidad

```
Utility (sistema de alquiler de monopatines eléctricos)
│
├── Seguridad
│   └── Uso de crédito de cuenta ajena (H, M)
│
├── Interoperabilidad
│   └── Integración con Mercado Pago (H, M)
│
├── Modificabilidad
│   └── Tarifa extra por reinicio de pausa (M, L)
│
├── Observabilidad/Trazabilidad
│   └── Historial de uso para mantenimiento (M, M)
│
├── Eficiencia Energética
│   └── Apagado manual en pausa (L, L)
│
└── Disponibilidad
    └── Ubicación consultable en todo momento (H, M)
```

## Justificación de cada etiqueta

| Escenario | Importancia negocio | Por qué | Dificultad técnica | Por qué |
|---|---|---|---|---|
| Seguridad — crédito ajena | **H** | Involucra dinero de un usuario; un fallo aquí genera pérdida directa y de confianza en la plataforma (criterio: dinero de por medio → nivel alto). | **M** | Se resuelve con tácticas conocidas y acotadas (autorización, registro de auditoría) sobre el módulo de cuentas — no requiere cambios transversales. |
| Interoperabilidad — Mercado Pago | **H** | Es el mecanismo que permite cargar crédito: si falla, bloquea el uso pago del servicio; involucra dinero y un tercero externo. | **M** | Depende de una API externa (riesgo de cambios/errores no controlados), pero es una integración puntual con patrones conocidos (adapter/wrapper). |
| Modificabilidad — tarifa extra | **M** | Afecta la flexibilidad operativa del Administrador, pero no bloquea el uso del servicio si se pospone (se puede cambiar el precio manualmente en el corto plazo). | **L** | Se resuelve con una táctica simple y localizada (parámetro de configuración), sin tocar múltiples componentes. |
| Observabilidad — historial mantenimiento | **M** | Afecta la calidad de las decisiones de mantenimiento, pero no bloquea el uso del servicio por parte de los usuarios finales. | **M** | Requiere construir un módulo de reportes propio con datos históricos — esfuerzo moderado, no trivial pero con patrones conocidos (logging + consulta). |
| Eficiencia Energética — apagado en pausa | **L** | Es una mejora de costo operativo deseable, pero no bloquea ni degrada la experiencia del usuario si se pospone. | **L** | Acción simple y localizada sobre el módulo de control del motor, sin dependencias complejas. |
| Disponibilidad — ubicación en todo momento | **H** | Es el corazón del caso de uso principal (encontrar un monopatín): si el dato de ubicación no es confiable, el servicio deja de ser usable. | **M** | Requiere actualización de posición razonablemente frecuente sobre dispositivos distribuidos (IoT/GPS), con riesgo de fallas de conectividad — no trivial, pero con tácticas conocidas (heartbeat, tolerancia a datos desactualizados). |

## Prioridad de atención sugerida

1. **(H, M)** — Seguridad (crédito ajena), Interoperabilidad (Mercado Pago) y Disponibilidad (ubicación) son los tres candidatos prioritarios: alto impacto de negocio con dificultad técnica moderada, ameritan diseño arquitectónico cuidadoso desde el inicio.
2. **(M, M)** y **(M, L)** — Observabilidad y Modificabilidad pueden abordarse en una segunda etapa de diseño.
3. **(L, L)** — Eficiencia Energética es la de menor urgencia; puede posponerse sin riesgo para el diseño arquitectónico inicial.
