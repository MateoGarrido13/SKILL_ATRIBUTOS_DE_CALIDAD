# 🔒 Seguridad (Security)

> Fuente: *Software Architecture in Practice* (4th ed.), Bass/Clements/Kazman, Cap. 11 — Security.
> 

# Escenario General de Seguridad

| **Parte** | **Descripción** | **Valores posibles** |
| --- | --- | --- |
| Fuente | El ataque puede venir de dentro o fuera de la organización; puede ser una persona u otro sistema; puede haber sido identificado antes (correcta o incorrectamente) o ser desconocido | Humano · Otro sistema — que está: dentro de la organización · fuera de la organización · previamente identificado · desconocido |
| Estímulo | El estímulo es un ataque: un intento no autorizado de | Mostrar datos · Capturar datos · Cambiar o borrar datos · Acceder a servicios del sistema · Cambiar el comportamiento del sistema · Reducir la disponibilidad |
| Artefacto | Cuál es el blanco del ataque | Servicios del sistema · Datos dentro del sistema · Un componente o recurso del sistema · Datos producidos o consumidos por el sistema |
| Ambiente | El estado del sistema cuando ocurre el ataque | El sistema está: online u offline · conectado o desconectado de una red · detrás de un firewall o abierto a una red · totalmente operativo · parcialmente operativo · no operativo |
| Respuesta | El sistema asegura confidencialidad, integridad y disponibilidad | Las transacciones se realizan de forma que: los datos/servicios están protegidos de acceso no autorizado · no se manipulan sin autorización · las partes de una transacción se identifican con certeza · las partes no pueden repudiar su participación · los datos/recursos/servicios estarán disponibles para uso legítimo. Además, el sistema registra actividades: acceso o modificación, intentos de acceso, y notifica a entidades apropiadas ante un aparente ataque |
| Medida de Respuesta | Relacionadas con la frecuencia de ataques exitosos, el tiempo/costo de resistir y reparar, y el daño consecuente | Cuánto de un recurso quedó comprometido o asegurado · Precisión de la detección del ataque · Tiempo transcurrido hasta detectar el ataque · Cuántos ataques se resistieron · Cuánto se tarda en recuperarse de un ataque exitoso · Cuánta información es vulnerable a un ataque particular |

**Ejemplo concreto (del libro):** un empleado descontento en una ubicación remota intenta modificar indebidamente la tabla de sueldos durante operación normal; el acceso no autorizado se detecta, el sistema mantiene un registro de auditoría y los datos correctos se restauran dentro de un día.

# Tácticas para Seguridad

Inspiradas en seguridad física (vallas, guardias, cerraduras, backups). Cuatro categorías: **detectar**, **resistir**, **reaccionar** y **recuperarse de** ataques.

## Detectar Ataques

- **Detect intrusion**: compara tráfico/patrones de pedidos contra firmas conocidas de comportamiento malicioso.
- **Detect service denial**: compara el tráfico entrante contra perfiles históricos de ataques DoS conocidos.
- **Verify message integrity**: checksums o hashes para verificar la integridad de mensajes y archivos.
- **Detect message delivery anomalies**: detecta posibles ataques man-in-the-middle observando tiempos de entrega o patrones de conexión inusuales.

## Resistir Ataques

- **Identify actors**: identificar la fuente de cualquier entrada externa (user IDs, IPs, protocolos, puertos).
- **Authenticate actors**: confirmar que un actor es quien dice ser (contraseñas, 2FA, biometría, certificados, CAPTCHA).
- **Authorize actors**: verificar que un actor autenticado tiene permisos sobre datos/servicios (control de acceso por actor, clase o rol).
- **Limit access**: restringir puntos de acceso y tipo de tráfico permitido (ej. DMZ con doble firewall).
- **Limit exposure**: minimizar el efecto del daño reduciendo qué datos/servicios son accesibles desde un mismo punto de acceso.
- **Encrypt data**: proteger confidencialidad de datos y comunicación (simétrica o asimétrica).
- **Separate entities**: aislar partes del sistema (servidores/redes distintos, VMs, air gap) para limitar el alcance de un ataque.
- **Validate input**: sanitizar/filtrar entradas para prevenir SQL injection, XSS, etc.
- **Change credential settings**: forzar el cambio de credenciales por defecto o periódicamente.

## Reaccionar a Ataques

- **Revoke access**: limitar severamente el acceso ante un ataque en curso, incluso a usuarios normalmente legítimos.
- **Restrict login**: limitar/bloquear tras repetidos intentos fallidos de login (a veces duplicando el tiempo de bloqueo en cada intento).
- **Inform actors**: notificar a operadores, personal o sistemas cooperantes ante un ataque detectado.

## Recuperarse de Ataques

Se reutilizan las tácticas de recuperación de Disponibilidad (Cap. 4), más:

- **Audit**: mantener registro de acciones de usuarios/sistema para rastrear atacantes y mejorar defensas futuras.
- **Nonrepudiation**: garantizar que emisor y receptor de un mensaje no puedan negar haberlo enviado/recibido (firmas digitales + autenticación de terceros confiables).

# Cómo reconocer Seguridad en un escenario

Aparece cuando el estímulo es un **ataque o intento de acceso/manipulación no autorizado**, y lo que importa es confidencialidad, integridad, disponibilidad frente a un adversario (no una falla accidental, que sería Disponibilidad, ni un riesgo físico, que sería Safety). Palabras clave: *atacante, acceso no autorizado, autenticación, autorización, cifrado, auditoría, brecha*.