# [INC-20261101-041] Inicios de Sesión desde IP Anónima — El Contexto Decide

> Nota de confidencialidad: identificadores anonimizados. `[username]`, `[external-ip]` son placeholders.

## Resumen Narrativo
Alerta de Entra ID Protection **"Anonymous IP address"** (Tor / VPN anonimizadora), táctica Initial Access. El mismo tipo de alerta se resolvió con veredictos distintos según el contexto, demostrando que la reputación de la IP y su coincidencia con el patrón del usuario —no el nombre de la alerta— definen el caso.

## Detalles del Incidente
| Campo | Valor |
|---|---|
| Alerta | Anonymous IP address |
| Producto | Microsoft Entra ID Protection (vía Sentinel) |
| Severidad | Media |
| Táctica (MITRE) | Initial Access |
| Usuario | [username] |
| IP | [external-ip] |

## Proceso de Investigación y Escenarios
Reflejo de verificación: (1) reputación y tipo de la IP; (2) si es habitual para el usuario; (3) éxito + MFA; (4) otras detecciones de riesgo; (5) actividad post-inicio (reglas de reenvío de buzón = huella clásica de toma de cuenta).

- **Benigno:** inicio exitoso con MFA, desde una IP de VPN/hosting coincidente con el uso del usuario; app accedida = aplicación de línea de negocio habitual → usuario legítimo con VPN.
- **Ataque contenido (verdadero positivo):** adivinación distribuida desde varias IPs externas (incl. VPN comercial); contraseña adivinada desde una IP pero el inicio se detuvo en el **MFA** → credencial comprometida, bloqueada por MFA. Acción: reset de contraseña, verificar que no haya vía sin MFA, revocar sesiones.
- **No resuelto / sospechoso:** inicio desde IP de hosting/datacenter que **NO** coincidía con el patrón del usuario (dispositivo/ubicación habitual distintos), usada exclusivamente por esa cuenta → escalado, pendiente de revisar reglas de buzón.

## Conclusión / Lección
Una IP/país/VPN es benigna o sospechosa según **coincida con el historial y el rol del usuario**. El MFA es la barrera que distingue "atacada" de "comprometida": que use MFA en general no prueba que el inicio puntual sea del dueño; hay que verificar éxito + MFA en ese evento.

## Herramientas Utilizadas
- Microsoft Sentinel (KQL — `SigninLogs`, `AADNonInteractiveUserSignInLogs`, `AADUserRiskEvents`, `IdentityLogonEvents`)
- Entra ID Protection
- VirusTotal / AbuseIPDB (reputación de IP)
