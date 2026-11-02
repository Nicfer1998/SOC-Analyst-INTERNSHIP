# [INC-20261101-046] Multi-Stage Incident (Fusion) — Credential Access + Lateral Movement (Benign)

> Confidentiality note: identifiers anonymized. `[admin-user]`, `[internal-ip]` are placeholders.

## Narrative Summary
Microsoft Sentinel's **Fusion** feature correlated a "multiple failures + success" alert (credential access) with "Rare RDP Connections" alerts (lateral movement) to a domain controller. Analysis showed both *legs* traced to an administrator's normal activity; benign.

## Incident Details
| Field | Value |
|---|---|
| Alert | Multi-stage incident involving Credential Access & Lateral Movement |
| Product | Microsoft Sentinel (Fusion) |
| Severity | Low |
| Account | [admin-user] |
| Hosts | application server + Domain Controller |

## Investigation Process
1. Fusion correlates individual alerts from different kill-chain stages into a higher-confidence incident. **A multi-stage incident is only as strong as its legs** → validate each one.
2. Leg 1 (credential access): the failures+success alert had already been determined benign (stale credential / interrupt codes).
3. Leg 2 (lateral movement): "Rare RDP" = 4624 logons, LogonType 10 (RemoteInteractive) from an internal IP to a DC, by the admin account — exactly what an administrator does to manage.

## Conclusion
**Benign positive.** Both legs trace to normal administrative activity correlated by Fusion.

## Recommendation / Lesson
Validate **EACH** leg. Still, don't dismiss casually: Credential Access → lateral movement to a DC is the real ransomware kill chain. Govern privileged accounts (PIM / PAW / MFA).

## Tools Used
- Microsoft Sentinel (Fusion, SecurityEvent 4624)
