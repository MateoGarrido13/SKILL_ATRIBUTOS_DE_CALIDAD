# 🚀 Desplegabilidad (Deployability)

> Fuente: *Software Architecture in Practice* (4th ed.), Bass/Clements/Kazman, Cap. 5 — Deployability.
> 

# Escenario General de Desplegabilidad

| **Parte** | **Descripción** | **Valores posibles** |
| --- | --- | --- |
| Fuente | El disparador del despliegue | Usuario final, desarrollador, administrador del sistema, personal de operaciones, marketplace de componentes, dueño del producto |
| Estímulo | Qué provoca el disparador | Hay un nuevo elemento disponible para desplegar (típicamente reemplazar un elemento de software por una nueva versión: corregir un defecto, aplicar un parche de seguridad, actualizar a la última versión de un componente/framework) · Un nuevo elemento fue aprobado para incorporación · Un elemento/conjunto de elementos existente necesita revertirse (rollback) |
| Artefactos | Qué se va a cambiar | Componentes o módulos específicos, la plataforma del sistema, su interfaz de usuario, su entorno, u otro sistema con el que interopera; puede ser un único elemento, varios, o todo el sistema |
| Ambiente | Staging, producción (o un subconjunto específico de cualquiera de los dos) | Despliegue completo · Despliegue a un subconjunto específico de usuarios, VMs, contenedores, servidores, plataformas |
| Respuesta | Qué debería pasar | Incorporar los nuevos componentes · Desplegar los nuevos componentes · Monitorear los nuevos componentes · Revertir un despliegue anterior |
| Medida de Respuesta | Medida de costo, tiempo o efectividad del proceso, para un despliegue o para una serie de despliegues | Costo en términos de: número/tamaño/complejidad de artefactos afectados, esfuerzo promedio/peor caso, tiempo transcurrido, dinero, nuevos defectos introducidos · Grado en que el despliegue/rollback afecta otras funciones o atributos de calidad · Número de despliegues fallidos · Repetibilidad del proceso · Trazabilidad del proceso · Tiempo de ciclo del proceso |

**Ejemplo concreto (del libro):** una nueva versión de un servicio de autenticación/autorización aparece en el marketplace de componentes y el dueño del producto decide incorporarla; se prueba y despliega a producción en 40 horas y no más de 120 horas-persona, sin introducir defectos ni violar ningún SLA.

# Conceptos clave previos

- **Pipeline de despliegue**: secuencia de herramientas y actividades desde que el código se sube a control de versiones hasta que la app queda desplegada. Ambientes: **desarrollo** (unit tests) → **integración** (build + tests de integración) → **staging** (performance, seguridad, licencias, tests de usuario) → **producción** (monitoreo continuo).
- **Despliegue continuo** (sin intervención humana) vs. **entrega continua** (con intervención humana en el paso final).
- Tres medidas de calidad del pipeline: **cycle time** (velocidad de avance por el pipeline), **trazabilidad** (poder recuperar todos los artefactos/versiones que llevaron a un problema) y **repetibilidad** (obtener el mismo resultado con las mismas entradas).
- **DevOps**: conjunto de prácticas para reducir el tiempo entre un commit y su paso a producción manteniendo alta calidad; el despliegue continuo es su núcleo conceptual. **DevSecOps** incorpora seguridad a todo el proceso.

# Tácticas para Desplegabilidad

## Gestionar el Pipeline de Despliegue

- **Scale rollouts**: desplegar gradualmente a subconjuntos controlados de usuarios en vez de a todos a la vez, para monitorear y poder revertir si algo falla.
- **Rollback**: revertir un despliegue defectuoso a su estado previo (idealmente de forma automatizada, incluso con múltiples servicios/datos coordinados).
- **Script deployment commands**: automatizar y orquestar los pasos del despliegue mediante scripts versionados y testeados.

## Gestionar el Sistema Desplegado

- **Manage service interactions**: permitir que convivan múltiples versiones de un servicio simultáneamente, mediando las interacciones para evitar incompatibilidades.
- **Package dependencies**: empaquetar un elemento junto con sus dependencias (librerías, versión de SO, contenedores utilitarios) usando contenedores, pods o VMs.
- **Feature toggle**: "interruptor" para deshabilitar una funcionalidad en runtime sin necesidad de un nuevo despliegue.

# Patrones para Desplegabilidad

**Para estructurar servicios:**

- **Microservice architecture**: servicios pequeños, independientemente desplegables, que solo se comunican por mensajes vía interfaces. Beneficios: reduce el time-to-market, cada equipo elige su tecnología, fácil de escalar. Costos: overhead de red, poco apta para transacciones complejas, requiere catálogos para mantener control intelectual.

**Para reemplazo completo de servicios:**

- **Blue/green**: se crean N instancias nuevas ("green"); cuando funcionan bien se conmuta el tráfico y luego se eliminan las instancias viejas ("blue"). Pico de uso: 2N instancias.
- **Rolling upgrade**: se reemplazan las instancias de a una (o pocas) por vez. Pico de uso: N+1 instancias. Riesgo de inconsistencia temporal (un cliente atendido a veces por la versión vieja, a veces por la nueva) y de incompatibilidad de interfaz si conviven ambas versiones.

**Para reemplazo parcial (multi-versión simultánea):**

- **Canary testing**: un grupo reducido de usuarios (a veces "power users") prueba la nueva versión en producción antes del rollout completo.
- **A/B testing**: se muestran variantes distintas a distintos grupos de usuarios para medir cuál da mejor resultado de negocio (ej. las 41 tonalidades de azul de Google).

# Cómo reconocer Desplegabilidad en un escenario

Aparece cuando el estímulo es la **llegada de una nueva versión/elemento a desplegar** (o la necesidad de revertir uno), y lo que importa es el costo/tiempo/riesgo de llevarlo a producción (o hacer rollback), no el comportamiento en producción en sí. Palabras clave: *release, versión, pipeline CI/CD, rollout, rollback, feature flag, staging/producción*.