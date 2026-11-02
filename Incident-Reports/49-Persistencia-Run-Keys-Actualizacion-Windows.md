# [INC-20261101-049] Persistencia — Claves Run Creadas por Actualización de Windows (Benigno + Hallazgo PUP)

> Nota de confidencialidad: identificadores anonimizados. `[hostname]` es placeholder.

## Resumen Narrativo
Microsoft Sentinel generó un incidente de persistencia por la creación de claves de registro Run/RunOnce (MITRE T1547.001) en un endpoint. El análisis mostró que fue una actualización de Windows (feature update) orquestada por un agente de parcheo RMM. Al leer los valores, se encontró **incidentalmente un navegador PUP** en autoarranque.

## Detalles del Incidente
| Campo | Valor |
|---|---|
| Alerta | Persistence incident on one endpoint |
| Producto | Microsoft Sentinel |
| Severidad | Media |
| Técnica (MITRE) | Boot/Logon Autostart Execution: Registry Run Keys (T1547.001) |
| Host | [hostname] |

## Proceso de Investigación
1. Las claves Run/RunOnce ejecutan programas en cada logon → mecanismo favorito del malware para persistir. Por eso cualquier creación dispara la alerta.
2. El proceso era `SetupHost.exe` con `Install.esd` = actualización in-place de Windows, orquestada por un agente de parcheo RMM. El upgrade escribe Run/RunOnce legítimamente para reanudarse entre reinicios. El prefijo `$OFFLINE_RW_` = acceso offline a hives; entradas en todos los hives de usuario = upgrade system-wide.
3. Al leer los **VALORES** de las claves, la mayoría eran autostart legítimos (OneDrive, una utilidad, software OEM). PERO uno era un **navegador PUP** (hijacker de búsqueda) en autoarranque.

## Conclusión
**Alerta de persistencia = benigna** (el upgrade surfó las entradas Run existentes). **Hallazgo incidental:** un navegador PUP a remediar.

## Recomendación / Lección
Juzgar por el **ARTEFACTO** real (el valor Run / binario / firma), no por la etiqueta de la técnica. Leer los valores no solo confirmó benigno: cazó algo que la alerta ni buscaba. Remediar el PUP e investigar cómo llegó (bundling / malvertising).

## Herramientas Utilizadas
- Microsoft Sentinel
- VirusTotal (clasificación del PUP)
