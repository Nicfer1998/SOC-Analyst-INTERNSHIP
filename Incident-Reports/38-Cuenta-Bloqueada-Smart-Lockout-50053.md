# [INC-20261101-038] Cuenta Bloqueada por Smart Lockout (50053) — Ataque de Credenciales Contenido

> Nota de confidencialidad: identificadores anonimizados. `[username]`, `[org-country]` son placeholders.

## Resumen Narrativo
Una cuenta fue bloqueada por el Smart Lockout de Entra ID (error **50053**) tras múltiples intentos fallidos de inicio de sesión desde varias IPs en distintos países. El análisis confirmó que los únicos accesos exitosos provenían del país de la organización (usuario legítimo), mientras que todos los intentos extranjeros fallaban y disparaban el bloqueo. Veredicto: cuenta bajo ataque de adivinación de credenciales distribuido, **NO comprometida**.

## Detalles del Incidente
| Campo | Valor |
|---|---|
| Alerta | Failed login attempts to Azure Portal involving one user |
| Producto | Microsoft Sentinel / Entra ID |
| Severidad | Baja |
| Táctica (MITRE) | Credential Access (T1110 - Brute Force) |
| Usuario | [username] |
| Error | 50053 - Account locked (Smart Lockout) |

## Proceso de Investigación
1. El error 50053 indica que la cuenta se bloqueó por demasiados intentos con credenciales incorrectas (Smart Lockout) — la defensa ya actuó.
2. Se revisaron las IPs de origen: los intentos fallidos venían de **múltiples países**. La dispersión geográfica descartó la hipótesis benigna de "credencial vieja en un dispositivo".
3. Se separaron éxitos y fallos: los únicos inicios de sesión exitosos provenían del país de la organización (`[org-country]`); todo lo extranjero era fallo.
4. El asedio externo, además de la amenaza, bloqueaba la cuenta del usuario legítimo (efecto tipo denegación de servicio).

## Conclusión
**Cuenta atacada, NO comprometida.** Ataque de adivinación de credenciales distribuido desde IPs extranjeras; ningún acceso externo tuvo éxito (el Smart Lockout contuvo). El usuario legítimo sigue accediendo desde su país.

## Recomendación
- **Acceso Condicional** restringiendo por ubicación geográfica (permitir el país de la organización, bloquear el resto) para cortar el ataque antes de la fase de contraseña y liberar al usuario.
- Reforzar / verificar MFA.
- Nota: el Smart Lockout (50053) se libera por tiempo; no requiere cambio de contraseña.

## Lección Aprendida
"Fallos repetidos" no es compromiso. La pregunta que define es: ¿hubo algún inicio de sesión **EXITOSO** desde una IP externa? Si todo lo foráneo falla, la defensa aguantó.

## Herramientas Utilizadas
- Microsoft Sentinel (KQL — `SigninLogs`)
- Entra ID Protection
