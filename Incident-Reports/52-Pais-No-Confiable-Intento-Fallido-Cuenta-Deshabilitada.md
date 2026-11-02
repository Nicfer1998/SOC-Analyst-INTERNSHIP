# [INC-20261101-052] "Inicio Exitoso desde País No Confiable" — Intento Fallido en Cuenta Deshabilitada (Sin Compromiso)

> Nota de confidencialidad: identificadores anonimizados. `[user]`, `[external-ip]` son placeholders.

## Resumen Narrativo
Una alerta titulada **"Successful sign-in from untrusted country"** resultó ser, al verificar, un inicio de sesión **FALLIDO** (ResultType 50012) desde el extranjero contra una cuenta externa que nunca tuvo un acceso exitoso. Sin compromiso.

## Detalles del Incidente
| Campo | Valor |
|---|---|
| Alerta | Successful sign-in from untrusted country |
| Producto | Microsoft Sentinel |
| Severidad | Media |
| Cuenta | [user] (externa) |
| IP | [external-ip] (país no confiable) |

## Proceso de Investigación
1. El **título** decía "successful", pero el log mostraba ResultType **50012** (Authentication failed) — el inicio FALLÓ.
2. La cuenta mostraba **0 exitosos / 29 fallidos** en 90 días → nunca tuvo un inicio exitoso, indicando fuertemente una cuenta deshabilitada/dada de baja (confirmar en Entra ID). Las "6 alertas en 1 incidente" eran múltiples detecciones sobre un único evento.

## Conclusión
**Intento de acceso fallido desde país no confiable contra una cuenta (probablemente) deshabilitada — sin compromiso.**

## Recomendación / Lección
No confiar en el **TÍTULO** de la alerta ("successful") — verificar el código de resultado. "Failed" está probado; "disabled" se infiere del historial 0/29 y debe confirmarse en Entra.

## Herramientas Utilizadas
- Microsoft Sentinel (KQL — SigninLogs, IdentityInfo)
