# [INC-20261101-042] Despliegue de Recursos Sospechoso en la Nube (Benigno — Anomalía)

> Nota de confidencialidad: identificadores anonimizados. `[user]`, `[resource-group]` son placeholders.

## Resumen Narrativo
Microsoft Defender for Cloud generó una alerta de **"Suspicious Resource deployment"** por un despliegue de recursos de Azure considerado "raro", hecho por un caller nunca visto en los 14 días previos. El análisis confirmó que se trataba de un administrador/automatización legítimo de infraestructura cloud; la anomalía era por baja frecuencia, no por malicia.

## Detalles del Incidente
| Campo | Valor |
|---|---|
| Alerta | Suspicious Resource deployment |
| Producto | Microsoft Defender for Cloud (vía Sentinel) |
| Severidad | Baja |
| Táctica (MITRE) | Impact |
| Operación | MICROSOFT.RESOURCES/DEPLOYMENTS/WRITE |
| Caller | [user] / service principal |
| Resource Group | [resource-group] |

## Proceso de Investigación
1. La detección es por **anomalía**: "deployment raro, caller nunca visto en 14 días". Busca abuso de recursos (p. ej. cryptomining), de ahí la táctica Impact. "Raro" = inusual, no necesariamente malicioso.
2. Se identificó al caller: **administrador de infraestructura cloud** (historial consistente de operaciones de deployment/red/almacenamiento) o aplicación/automatización legítima.
3. La IP de origen era la **habitual** del administrador; el despliegue encaja con su rol.

## Conclusión
**Benigno positivo.** Actividad legítima de un administrador/automatización de cloud, marcada como anómala por ser infrecuente en la ventana de 14 días.

## Recomendación / Lección
Las verificaciones decisivas: **QUÉ** se desplegó (cómputo caro = bandera de cryptomining) y **QUIÉN** es el caller (rol + historial de IP). Para cuentas privilegiadas de cloud, verificar que usen MFA siempre.

## Herramientas Utilizadas
- Microsoft Defender for Cloud
- Microsoft Sentinel (KQL — `AzureActivity`, `SigninLogs`, `IdentityInfo`)
