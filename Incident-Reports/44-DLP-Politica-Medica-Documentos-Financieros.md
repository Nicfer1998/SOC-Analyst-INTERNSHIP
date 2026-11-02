# [INC-20261101-044] DLP — Política de Información Médica sobre Documentos Financieros (Falso Positivo)

> Nota de confidencialidad: identificadores anonimizados. `[user]` es placeholder.

## Resumen Narrativo
Microsoft Purview DLP (motor por defecto) disparó la política de **"información médica protegida"** sobre archivos PDF de estados contables consolidados almacenados en la nube. El contenido (financiero) no coincide con la política (médica), lo que apunta a un falso positivo del clasificador por defecto.

## Detalles del Incidente
| Campo | Valor |
|---|---|
| Alerta | File containing protected medical information detected on the cloud |
| Producto | Microsoft Purview DLP (vía Sentinel) |
| Severidad | Media |
| Usuarios | [user] (área contable / finanzas) |
| Aplicación | Almacenamiento en la nube (OneDrive/SharePoint) |

## Proceso de Investigación
1. **Mismatch política/contenido**: una política médica se disparó sobre documentos financieros. Los clasificadores por defecto hacen coincidencia por patrones; secuencias numéricas de documentos financieros pueden colisionar con patrones de identificadores de salud.
2. Confirmar inspeccionando el tipo de información sensible / contenido coincidente y el rol de los usuarios (finanzas).

## Conclusión
**Probable falso positivo.**

## Recomendación / Lección
Afinar la política / el umbral de confianza. Por separado, **verificar que los archivos NO hayan sido compartidos externamente** (sería una preocupación real distinta, independiente del falso disparo médico). La naturaleza del caso es de gobernanza/decisión, no solo veredicto.

## Herramientas Utilizadas
- Microsoft Purview DLP
- Microsoft Sentinel
