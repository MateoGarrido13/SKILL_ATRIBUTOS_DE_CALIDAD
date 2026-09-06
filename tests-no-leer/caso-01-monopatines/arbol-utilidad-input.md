# Caso 01 — monopatines — input para skill-arbol-utilidad

> Los 6 escenarios completos identificados para el sistema de monopatines (ejercicio 3-B de Notion), con sus stakeholders (ejercicio 3-A). Todas las medidas de respuesta con "X"/"Y" fueron completadas con valores asumidos para este test, marcados como tales.

## 1. Seguridad — uso de crédito de una cuenta ajena

| Parte | Valor |
|---|---|
| Fuente | Un usuario del sistema |
| Estímulo | Intento de consumir crédito de una cuenta ajena |
| Artefacto | Módulo de gestión de cuentas y créditos |
| Entorno | Operación normal |
| Respuesta | El sistema rechaza la operación y no descuenta crédito de la cuenta ajena |
| Medida | 0% de transacciones exitosas de este tipo; el intento queda registrado en el log de auditoría |

**Stakeholder:** Usuario (dueño del dinero de la cuenta).

## 2. Interoperabilidad — integración con Mercado Pago

| Parte | Valor |
|---|---|
| Fuente | La API de Mercado Pago |
| Estímulo | Solicitud de descuento/carga de saldo |
| Artefacto | Módulo/adaptador de integración con MP |
| Entorno | Operación normal, cuenta ya vinculada |
| Respuesta | El sistema envía y recibe correctamente la confirmación, actualizando el saldo |
| Medida | 99.9% de transacciones sin error de comunicación, en menos de 3 segundos *(valor asumido)* |

**Stakeholder:** Developer (implementa la integración) y Owner (le importa que la plata se cobre bien).

## 3. Modificabilidad — tarifa extra por pausa

| Parte | Valor |
|---|---|
| Fuente | El Administrador de Monopatines |
| Estímulo | Solicita modificar la fórmula/monto de la tarifa extra por reinicio de pausas extensas |
| Artefacto | Módulo de cálculo de tarifas |
| Entorno | Tiempo de diseño/configuración |
| Respuesta | El cambio se aplica sin modificar código fuente ni redeployar |
| Medida | El cambio se realiza en menos de 30 minutos *(valor asumido)*, sin afectar otras funcionalidades, con 0 líneas de código modificadas *(valor asumido)* |

**Stakeholder:** Administrador.

## 4. Observabilidad/Trazabilidad — historial para mantenimiento

| Parte | Valor |
|---|---|
| Fuente | Encargado de Mantenimiento |
| Estímulo | Necesita determinar si un monopatín requiere mantenimiento |
| Artefacto | Módulo de registro de uso / generación de reportes |
| Entorno | Operación normal |
| Respuesta | El sistema expone el historial de km y tiempo de uso (con y sin pausas) de forma consultable |
| Medida | El encargado obtiene el dato completo en menos de 5 segundos *(valor asumido)*, con 100% de precisión respecto al uso real |

**Stakeholder:** Encargado de Mantenimiento / Administrador.

## 5. Eficiencia Energética — apagado manual en pausa

| Parte | Valor |
|---|---|
| Fuente | El usuario del servicio |
| Estímulo | Activa la opción "Pausar" durante un viaje |
| Artefacto | Módulo de control del motor/encendido |
| Entorno | Viaje en curso, pausa de hasta 15 minutos |
| Respuesta | El sistema apaga el motor sin desasignarlo de la cuenta |
| Medida | El apagado se ejecuta en menos de 2 segundos *(valor asumido)* desde que se activa la pausa |

**Stakeholder:** Administrador de Monopatines (responsable del mantenimiento/costo operativo de la flota).

## 6. Disponibilidad — ubicación consultable en todo momento

| Parte | Valor |
|---|---|
| Fuente | El usuario del servicio (o Administrador) |
| Estímulo | Solicita ver monopatines disponibles en el mapa de su zona |
| Artefacto | Módulo de localización/mapa |
| Entorno | Operación normal |
| Respuesta | El sistema muestra la posición actualizada de monopatines cercanos |
| Medida | La posición mostrada tiene una antigüedad menor a 30 segundos *(valor asumido)* en el 99% de las consultas |

**Stakeholder:** Usuario (para encontrar el monopatín más cercano) y Administrador (para localizarlo ante un problema).
