# [INC-20261101-046] Incidente Multi-Stage (Fusion) — Credential Access + Lateral Movement (Benigno)

> Nota de confidencialidad: identificadores anonimizados. `[admin-user]`, `[internal-ip]` son placeholders.

## Resumen Narrativo
La función **Fusion** de Microsoft Sentinel correlacionó una alerta de "múltiples fallos + éxito" (acceso a credenciales) con alertas de "Rare RDP Connections" (movimiento lateral) hacia un controlador de dominio. El análisis mostró que ambos *legs* trazaban a la actividad normal de un administrador; benigno.

## Detalles del Incidente
| Campo | Valor |
|---|---|
| Alerta | Multi-stage incident involving Credential Access & Lateral Movement |
| Producto | Microsoft Sentinel (Fusion) |
| Severidad | Baja |
| Cuenta | [admin-user] |
| Hosts | servidor de aplicaciones + Domain Controller |

## Proceso de Investigación
1. Fusion correlaciona alertas sueltas de distintas etapas del kill chain en un incidente de mayor confianza. **Un incidente multi-stage vale lo que valen sus legs** → hay que validar cada uno.
2. Leg 1 (acceso a credenciales): la alerta de fallos+éxito ya se había determinado benigna (credencial vieja / códigos de interrupción).
3. Leg 2 (movimiento lateral): "Rare RDP" = logons 4624 LogonType 10 (RemoteInteractive) desde una IP interna hacia un DC, por la cuenta de admin — exactamente lo que hace un administrador para gestionar.

## Conclusión
**Benigno positivo.** Ambos legs trazan a la actividad administrativa normal correlacionada por Fusion.

## Recomendación / Lección
Validar **CADA** leg. Aun así, no descartar a la ligera: Credential Access → movimiento lateral a un DC es la kill chain real del ransomware. Gobernar cuentas privilegiadas (PIM / PAW / MFA).

## Herramientas Utilizadas
- Microsoft Sentinel (Fusion, SecurityEvent 4624)
