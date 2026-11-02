# [INC-20261101-053] Usuario Agregado a Rol de Administrador (Benigno)

> Nota de confidencialidad: identificadores anonimizados. `[admin-user]`, `[target-user]` son placeholders.

## Resumen Narrativo
Microsoft Sentinel generó una alerta de escalación de privilegios por agregar un usuario a un rol privilegiado. Alerta **recurrente** con instancias previas cerradas como benignas. Se confirmó que fue el administrador de la organización asignando un rol de forma legítima.

## Detalles del Incidente
| Campo | Valor |
|---|---|
| Alerta | User Added to Admin Role |
| Producto | Microsoft Sentinel |
| Severidad | Baja |
| Táctica (MITRE) | Privilege Escalation |
| Origen | [admin-user] |
| Destino | [target-user] |

## Proceso de Investigación
1. Se verificó el **quién / qué / cuándo** contra el patrón conocido: mismo administrador de origen, usuario destino esperado, en horario laboral un día común — coincidiendo con las alertas anteriores.
2. También importa **CUÁL** rol se asignó: el mismo actor y destino pueden merecer una segunda mirada si el privilegio otorgado es inusualmente poderoso.

## Conclusión
**Benigno positivo** — asignación de rol legítima por el administrador.

## Recomendación / Lección
Un historial de "siempre benigno" es el **mapa de lo normal, no permiso para dejar de mirar** (fatiga / sesgo de normalización). Es el escondite perfecto para un atacante que ya comprometió el entorno. Usar el patrón como referencia, no como respuesta automática.

## Herramientas Utilizadas
- Microsoft Sentinel
