# [INC-20261101-042] Despliegue de Recursos Sospechoso en la Nube (Anomalía Benigna)

> Nota de confidencialidad: identificadores anonimizados. `[user]`, `[resource-group]` son placeholders.

## Resumen Narrativo
Microsoft Defender for Cloud generó una alerta **"Suspicious Resource deployment"** por un despliegue de recursos en Azure "raro", realizado por un caller nunca visto en los 14 días previos, táctica **Impact**. El análisis confirmó que el actor era un administrador legítimo de infraestructura cloud (o una automatización / service principal) y que la anomalía se debía a la baja frecuencia, no a actividad maliciosa.

## Detalles del Incidente
| Campo | Valor |
|---|---|
| Alerta | Suspicious Resource deployment |
| Producto | Microsoft Defender for Cloud (vía Sentinel) |
| Severidad | Baja |
| Táctica (MITRE) | Impact |
| Caller | [user] |
| Resource Group | [resource-group] |
| Operación | MICROSOFT.RESOURCES/DEPLOYMENTS/WRITE |

## Proceso de Investigación
1. La detección es por **anomalía**: "deployment raro" + "caller nunca visto en 14 días". Busca abuso de recursos (p. ej. cryptomining), de ahí la táctica Impact.
2. Se identificó al caller: su historial en `AzureActivity` mostró operaciones habituales de administración de infraestructura (deployments, redes, almacenamiento). Es un perfil de administrador / automatización, no un usuario comprometido.
3. La IP de origen coincidía con su patrón habitual (su IP/bastión de administración, con historial de semanas).
4. "Raro / nunca visto" = infrecuente en esa ventana de 14 días, no malicioso.

## Conclusión
**Benigno positivo.** Despliegue legítimo de un administrador de infraestructura cloud, marcado como anómalo por su baja frecuencia.

## Recomendación
- Exigir **MFA** en cuentas privilegiadas / de administración cloud (en un caso se observó autenticación single-factor).
- Confirmar **qué** se desplegó (descartar cómputo caro = bandera de cryptomining).

## Lección Aprendida
"Caller nunca visto" significa inusual, no malicioso. Lo decisivo es el **rol/historial del caller** y **QUÉ** se desplegó.

## Herramientas Utilizadas
- Microsoft Defender for Cloud
- Microsoft Sentinel (KQL — `AzureActivity`, `SigninLogs`)
- VirusTotal (reputación de IP)
