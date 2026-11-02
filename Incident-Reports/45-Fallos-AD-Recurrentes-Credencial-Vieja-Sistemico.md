# [INC-20261101-045] Fallos de Autenticación Recurrentes en AD — Credencial Vieja Cacheada (Problema Sistémico)

> Nota de confidencialidad: identificadores anonimizados. `[service-host]`, `[internal-ip]` son placeholders.

## Resumen Narrativo
Reglas de "múltiples fallos + éxito" / "fuerza bruta" en Active Directory se dispararon de forma **recurrente** para varias cuentas. El análisis mostró que son credenciales viejas cacheadas en un servidor interno de aplicaciones reintentando por NTLM, mientras la actividad legítima entra por Kerberos. No es un ataque; es un problema sistémico de sincronización de credenciales.

## Detalles del Incidente
| Campo | Valor |
|---|---|
| Alerta | Multiple authentication failures followed by a success / Brute force on AD |
| Producto | Microsoft Sentinel (SecurityEvent 4625/4624) |
| Severidad | Baja / Media |
| Táctica (MITRE) | Credential Access (T1110) |
| Host origen | [service-host] ([internal-ip]) |

## Proceso de Investigación
1. Los fallos (4625) venían por **NTLM** con "bad password" (0xC000006A) desde un único host interno; los éxitos (4624) por **Kerberos** desde otros hosts internos. Son dos cosas sin relación que la regla agrupa.
2. Causa raíz: un servicio/tarea en el servidor de aplicaciones tiene una contraseña vencida cacheada y reintenta en bucle, bloqueando repetidamente la cuenta (4740).
3. Verificación clave (lección): confirmar que **TODAS** las IPs de origen sean internas. En un caso, la "IP externa" resultó ser un servicio legítimo de seguridad de correo (benigno) — pero igual hubo que verificarlo.

## Conclusión
**Benigno positivo.** Credencial vieja cacheada reintentando; actividad legítima vía Kerberos. Afecta a múltiples cuentas = problema sistémico de un servidor interno.

## Recomendación / Lección
Escalar como **problema sistémico**: IT debe auditar el servidor de origen y actualizar las credenciales cacheadas de TODAS las cuentas de servicio/tareas afectadas de una vez, en lugar de cerrar tickets uno por uno. El split "interno vs externo" es el triaje más rápido.

## Herramientas Utilizadas
- Microsoft Sentinel (KQL — SecurityEvent 4624/4625/4740)
