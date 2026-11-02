# [INC-20261101-049] Persistence — Run Keys Created by a Windows Update (Benign + Incidental PUP)

> Confidentiality note: identifiers anonymized. `[hostname]` is a placeholder.

## Narrative Summary
Microsoft Sentinel raised a persistence incident for the creation of Run/RunOnce registry keys (MITRE T1547.001) on an endpoint. Analysis showed it was a Windows feature update orchestrated by an RMM patch-management agent. Reading the values surfaced an **incidental PUP browser** set to autostart.

## Incident Details
| Field | Value |
|---|---|
| Alert | Persistence incident on one endpoint |
| Product | Microsoft Sentinel |
| Severity | Medium |
| Technique (MITRE) | Boot/Logon Autostart Execution: Registry Run Keys (T1547.001) |
| Host | [hostname] |

## Investigation Process
1. Run/RunOnce keys execute programs at each logon → malware's favorite persistence mechanism. Any creation fires the alert.
2. The process was `SetupHost.exe` with `Install.esd` = Windows in-place feature update, orchestrated by an RMM patch-management agent. The upgrade legitimately writes Run/RunOnce to resume across reboots. The `$OFFLINE_RW_` prefix = offline hive access; entries across all user hives = system-wide upgrade.
3. Reading the key **VALUES**, most were legitimate autostart entries (OneDrive, a utility, OEM software). BUT one was a **PUP browser** (search hijacker) set to autostart.

## Conclusion
**Persistence alert = benign** (the upgrade surfaced existing Run entries). **Incidental finding:** a PUP browser to remediate.

## Recommendation / Lesson
Judge by the actual **ARTIFACT** (the Run value / binary / signature), not the technique label. Reading the values didn't just confirm benign — it caught something the alert wasn't even about. Remediate the PUP and investigate how it arrived (bundling / malvertising).

## Tools Used
- Microsoft Sentinel
- VirusTotal (PUP classification)
