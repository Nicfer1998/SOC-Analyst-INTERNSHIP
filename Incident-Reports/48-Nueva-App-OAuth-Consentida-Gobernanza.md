# [INC-20261101-048] Nueva Aplicación OAuth Consentida en el Tenant (Gobernanza)

> Nota de confidencialidad: identificadores anonimizados. `[tenant]` es placeholder.

## Resumen Narrativo
Microsoft Defender for Cloud Apps notificó el consentimiento de una **nueva aplicación OAuth de terceros** (un asistente de reuniones con IA) conectada al correo/calendario en la nube. No es un triaje benigno-vs-malicioso, sino una **decisión de gobernanza**.

## Detalles del Incidente
| Campo | Valor |
|---|---|
| Alerta | Create an OAuth app policy to notify you about new OAuth applications |
| Producto | Microsoft Defender for Cloud Apps |
| Severidad | Baja |
| Aplicación | App OAuth de terceros (asistente de reuniones IA) |

## Proceso de Investigación
1. Cuando un usuario consiente una app OAuth, ésta obtiene un token y permisos en el tenant. La política vigila apps nuevas por el riesgo de **OAuth consent phishing** (app maliciosa consentida = acceso persistente por token que sobrevive a cambio de password y MFA).
2. La app es legítima, pero pide permisos amplios y envía contenido a un tercero → Shadow IT + privacidad.

## Conclusión
**Revisión de gobernanza** (no benigno-vs-malicioso).

## Recomendación / Lección
Verificar que sea la app **GENUINA** (publisher verificado / app id) y no un impostor usado para consent phishing. Revisar scopes y consent de usuario vs admin; decidir sancionar/restringir/bloquear; considerar restringir el consentimiento de usuario a nivel tenant.

## Herramientas Utilizadas
- Microsoft Defender for Cloud Apps
- Microsoft Entra ID (Enterprise applications)
