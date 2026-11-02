# [INC-20261101-040] PUA — Herramientas de Administración de Red de Doble Uso (Benigno Esperado)

> Nota de confidencialidad: identificadores anonimizados. `[hostname]` es placeholder.

## Resumen Narrativo
Sophos detectó varias herramientas de administración/enumeración de red (escáner de puertos, enumerador de recursos de red y gestor de procesos remotos) como **PUA (Potentially Unwanted Application)**, todas ubicadas juntas en la carpeta de herramientas/respaldo del equipo de IT. Se confirmó como el kit de administración legítimo del equipo técnico.

## Detalles del Incidente
| Campo | Valor |
|---|---|
| Alerta | Potentially unwanted application detected |
| Producto | Sophos (vía Sentinel) |
| Severidad | Baja / Media |
| Host | [hostname] |
| Ubicación | Carpeta de herramientas de IT / backup |

## Proceso de Investigación
1. Las herramientas son de **doble uso**: legítimas para administración, pero también usadas por atacantes en fase de reconocimiento / movimiento lateral. Por eso Sophos las marca como PUA, no como malware.
2. La **ubicación** es decisiva: estaban en una carpeta de IT (no en temp ni en descargas de un usuario común), formando un kit coherente de administración de red (escáner + enumerador + ejecución remota).
3. Detecciones similares previas (recurrencia) consistentes con herramientas que el equipo de IT usa habitualmente.

## Conclusión
**Benigno positivo (esperado).** Kit de herramientas de administración de red del equipo de IT, marcado como PUA por su naturaleza de doble uso.

## Recomendación
- Crear una **exclusión** para las herramientas aprobadas / la ruta de IT, para cortar las detecciones recurrentes.
- **Restringir el acceso** a esa carpeta (solo IT): un repositorio con escáner + enumerador + ejecución remota es valioso para un atacante si es alcanzable.

## Lección Aprendida
Las mismas herramientas en una ubicación **NO** de IT, empaquetadas u ocultas, invertirían el veredicto a sospechoso. El contexto (quién, dónde, cómo) define una herramienta de doble uso.

## Herramientas Utilizadas
- Sophos
- Microsoft Sentinel
