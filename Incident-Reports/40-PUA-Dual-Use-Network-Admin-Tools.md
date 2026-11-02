# [INC-20261101-040] PUA — Dual-Use Network Administration Tools (Benign Expected)

> Confidentiality note: identifiers anonymized. `[hostname]` is a placeholder.

## Narrative Summary
Sophos detected several network administration/enumeration tools (a port scanner, a network-resource enumerator, and a remote process manager) as **PUA (Potentially Unwanted Application)**, all located together in the IT team's tool/backup folder. Confirmed as the IT team's legitimate administration toolkit.

## Incident Details
| Field | Value |
|---|---|
| Alert | Potentially unwanted application detected |
| Product | Sophos (via Sentinel) |
| Severity | Low / Medium |
| Host | [hostname] |
| Location | IT tool / backup folder |

## Investigation Process
1. The tools are **dual-use**: legitimate for administration, but also used by attackers during reconnaissance / lateral movement. That is why Sophos flags them as PUA, not malware.
2. The **location** is decisive: they were in an IT folder (not temp or a regular user's downloads), forming a coherent network-administration kit (scanner + enumerator + remote execution).
3. Similar prior detections (recurrence) consistent with tools the IT team uses regularly.

## Conclusion
**Benign positive (expected).** IT team's network administration toolkit, flagged as PUA due to its dual-use nature.

## Recommendation
- Create an **exclusion** for the approved tools / IT path to stop recurring detections.
- **Restrict access** to that folder (IT-only): a repository with scanner + enumerator + remote execution is valuable to an attacker if reachable.

## Lesson Learned
The same tools in a **non-IT** location, packaged or hidden, would flip the verdict to suspicious. Context (who, where, how) defines a dual-use tool.

## Tools Used
- Sophos
- Microsoft Sentinel
