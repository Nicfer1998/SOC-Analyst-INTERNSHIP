# [INC-20261101-047] Password Guessing Sospechado — Cuenta de Servicio de Escaneo (Benigno)

> Nota de confidencialidad: identificadores anonimizados. `[scanner-account]`, `[internal-ip]` son placeholders.

## Resumen Narrativo
Una regla de **"Suspected password guessing"** se disparó por fallos de autenticación (4625) desde múltiples IPs de origen. La cuenta involucrada es una cuenta de servicio de una herramienta de escaneo; los fallos venían de sus nodos colectores internos con una credencial desactualizada. Benigno.

## Detalles del Incidente
| Campo | Valor |
|---|---|
| Alerta | Suspected password guessing attempts in Active Directory |
| Producto | Microsoft Sentinel (SecurityEvent 4625) |
| Severidad | Media |
| Táctica (MITRE) | Credential Access (T1110) |
| Cuenta | [scanner-account] |

## Proceso de Investigación
1. El **nombre de la cuenta** delata una cuenta de servicio/escaneo (vuln scanner / herramienta de seguridad). Estas se autentican contra muchos hosts desde varios colectores → disparan naturalmente la regla de "múltiples IPs de origen".
2. Las IPs de origen eran **TODAS internas** (privadas) → argumenta contra un ataque externo distribuido.
3. Drill-down del 4625: SubStatus **0xC000006A** (contraseña incorrecta), LogonType 3 (Network). Causa: credencial del scanner desactualizada en la herramienta, reintentando por la red.

## Conclusión
**Benigno positivo** — configuración incorrecta de una cuenta de servicio de escaneo.

## Recomendación / Lección
Fix de **causa raíz**: actualizar la credencial del scanner en la herramienta; luego exclusión / subir umbral. Validar que las IPs de origen sean los nodos de escaneo conocidos y que la cuenta no esté siendo abusada (una cuenta "scanner" tiene alcance amplio → blanco goloso).

## Herramientas Utilizadas
- Microsoft Sentinel (KQL — SecurityEvent 4625)
