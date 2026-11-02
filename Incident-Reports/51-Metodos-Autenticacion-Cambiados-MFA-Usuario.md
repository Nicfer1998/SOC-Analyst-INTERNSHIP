# [INC-20261101-051] Métodos de Autenticación Cambiados — Registro de MFA por el Propio Usuario (Benigno)

> Nota de confidencialidad: identificadores anonimizados. `[user]`, `[external-ip]` son placeholders.

## Resumen Narrativo
Microsoft Sentinel generó una alerta de persistencia por el registro de un **nuevo método de autenticación** (ej. llave FIDO2) en una cuenta. Se confirmó que el propio usuario registró el método desde su contexto habitual. Benigno.

## Detalles del Incidente
| Campo | Valor |
|---|---|
| Alerta | Authentication methods changed |
| Producto | Microsoft Sentinel |
| Severidad | Baja |
| Táctica (MITRE) | Persistence |
| Cuenta | [user] |

## Proceso de Investigación
1. Agregar un método de MFA es **técnica de persistencia** (un atacante registra su propio método para mantener acceso). Por eso se vigila.
2. Se confirmó que el **propio usuario** inició el registro, desde un flujo de autoservicio normal (app Authenticator), desde una IP/país coincidente con su patrón habitual, sin actividad sospechosa previa. (Para un colaborador externo, un "país extranjero" es normal porque es donde trabaja — el contexto del usuario define lo anómalo.)

## Conclusión
**Benigno positivo** — registro legítimo de un método de MFA por el usuario (acción positiva de seguridad).

## Recomendación / Lección
"La cuenta usa MFA" por sí solo no prueba que el cambio lo hizo el dueño; verificar el registro contra el patrón histórico de IP/dispositivo del usuario y descartar compromiso previo.

## Herramientas Utilizadas
- Microsoft Sentinel (KQL — SigninLogs, AuditLogs)
- VirusTotal / AbuseIPDB (reputación de IP)
