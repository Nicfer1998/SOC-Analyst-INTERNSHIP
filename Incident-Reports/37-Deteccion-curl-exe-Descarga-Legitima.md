# [INC-20261101-037] Detección de curl.exe — Descarga Legítima (Falso Positivo)

> Nota de confidencialidad: todos los identificadores del cliente están anonimizados. Los valores entre corchetes (`[hostname]`, `[username]`) son placeholders.

## Resumen Narrativo
Una regla personalizada del cliente generó una alerta por el uso de `curl.exe` en un endpoint, mapeada a la táctica **Command and Control** (MITRE T1105). `curl` es una utilidad legítima incluida en Windows, pero de doble uso: los atacantes la emplean para descargar herramientas adicionales o exfiltrar datos. El análisis de la línea de comando confirmó que se trataba de la descarga del instalador legítimo de una aplicación comercial desde el dominio oficial del fabricante. Se clasificó como falso positivo / benigno, con una observación de Shadow IT.

## Detalles del Incidente
| Campo | Valor |
|---|---|
| Alerta | Custom Alert: Detection of CURL.EXE usage |
| Producto | Microsoft Defender for Endpoint / Microsoft Sentinel |
| Severidad | Baja |
| Táctica (MITRE) | Command and Control (T1105 - Ingress Tool Transfer) |
| Host | [hostname] |
| Usuario | [username] |

## Proceso de Investigación
1. La regla detecta cualquier ejecución de `curl.exe` por su potencial de C2. El dato decisivo no es "se usó curl", sino **qué comando ejecutó**.
2. El evento 4688 (creación de proceso) no estaba disponible en `SecurityEvent` para ese host (sin auditoría de línea de comando / sin forwarding). Se verificó con `SecurityEvent | where EventID == 4688 | where Computer has "[hostname]"` → sin resultados.
3. Se pivoteó a la tabla de Defender for Endpoint (`DeviceProcessEvents`) y se recuperó el `ProcessCommandLine`.
4. La línea de comando mostró a `curl` descargando el **instalador legítimo de una aplicación comercial de asistencia de escritura**, desde el **dominio oficial del fabricante**, por HTTPS, con parámetros de reintento y manejo de errores (firma típica de un instalador automatizado). La ruta de destino correspondía al propio proceso de instalación de la aplicación.

## Conclusión
**Falso positivo / Benigno.** `curl` ejecutó una descarga legítima del instalador de una aplicación conocida desde su dominio oficial; no hubo actividad de Command and Control.

**Observación (Gobernanza / Shadow IT):** la aplicación es software de terceros instalado por el usuario que envía contenido a la nube. Se señala para que el cliente confirme si su uso está aprobado o debe restringirse.

## Lección Aprendida
El nombre de una herramienta de doble uso (`curl`) no define nada por sí solo; la **línea de comando / URL de destino** lo define todo. Ante la falta de visibilidad en una tabla, pivotear a otra fuente de telemetría (Defender) antes de concluir.

## Herramientas Utilizadas
- Microsoft Sentinel (KQL — `SecurityEvent`, `DeviceProcessEvents`)
- Microsoft Defender for Endpoint
