# [INC-20261101-042] Suspicious Cloud Resource Deployment (Benign Anomaly)

> Confidentiality note: identifiers anonymized. `[user]`, `[resource-group]` are placeholders.

## Narrative Summary
Microsoft Defender for Cloud raised a **"Suspicious Resource deployment"** alert for a "rare" Azure deployment by a previously-unseen caller (no similar activity in the prior 14 days), **Impact** tactic. Analysis confirmed the actor was a legitimate cloud-infrastructure administrator (or automation / service principal) and the anomaly was due to infrequency, not malicious activity.

## Incident Details
| Field | Value |
|---|---|
| Alert | Suspicious Resource deployment |
| Product | Microsoft Defender for Cloud (via Sentinel) |
| Severity | Low |
| Tactic (MITRE) | Impact |
| Caller | [user] |
| Resource Group | [resource-group] |
| Operation | MICROSOFT.RESOURCES/DEPLOYMENTS/WRITE |

## Investigation Process
1. The detection is **anomaly-based**: "rare deployment" + "previously-unseen caller in 14 days". It targets resource abuse (e.g., crypto-mining), hence the Impact tactic.
2. The caller was identified: their `AzureActivity` history showed routine infrastructure-administration operations (deployments, networking, storage). An admin / automation profile, not a compromised user.
3. The source IP matched their habitual pattern (their admin IP/bastion, with weeks of history).
4. "Rare / previously-unseen" = infrequent in that 14-day window, not malicious.

## Conclusion
**Benign positive.** Legitimate deployment by a cloud-infrastructure administrator, flagged as anomalous due to low frequency.

## Recommendation
- Enforce **MFA** on privileged / cloud-admin accounts (single-factor authentication was observed in one case).
- Confirm **what** was deployed (rule out expensive compute = crypto-mining red flag).

## Lesson Learned
"Previously-unseen caller" means unusual, not malicious. What's decisive is the **caller's role/history** and **WHAT** was deployed.

## Tools Used
- Microsoft Defender for Cloud
- Microsoft Sentinel (KQL — `AzureActivity`, `SigninLogs`)
- VirusTotal (IP reputation)
