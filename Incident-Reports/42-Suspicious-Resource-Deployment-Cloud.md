# [INC-20261101-042] Suspicious Resource Deployment in the Cloud (Benign — Anomaly)

> Confidentiality note: identifiers anonymized. `[user]`, `[resource-group]` are placeholders.

## Narrative Summary
Microsoft Defender for Cloud raised a **"Suspicious Resource deployment"** alert for an Azure resource deployment flagged as "rare", performed by a previously-unseen caller in the prior 14 days. Analysis confirmed it was a legitimate cloud-infrastructure administrator/automation; the anomaly was due to low frequency, not malice.

## Incident Details
| Field | Value |
|---|---|
| Alert | Suspicious Resource deployment |
| Product | Microsoft Defender for Cloud (via Sentinel) |
| Severity | Low |
| Tactic (MITRE) | Impact |
| Operation | MICROSOFT.RESOURCES/DEPLOYMENTS/WRITE |
| Caller | [user] / service principal |
| Resource Group | [resource-group] |

## Investigation Process
1. The detection is **anomaly-based**: "rare deployment, previously-unseen caller in 14 days". It targets resource abuse (e.g. crypto-mining), hence the Impact tactic. "Rare" = unusual, not necessarily malicious.
2. The caller was identified: a **cloud-infrastructure administrator** (consistent history of deployment/network/storage operations) or a legitimate app/automation.
3. The source IP was the administrator's **habitual** one; the deployment fits their role.

## Conclusion
**Benign positive.** Legitimate cloud administrator/automation activity, flagged as an anomaly because it was infrequent within the 14-day window.

## Recommendation / Lesson
The decisive checks: **WHAT** was deployed (expensive compute = crypto-mining red flag) and **WHO** the caller is (role + IP history). For privileged cloud accounts, verify MFA is always enforced.

## Tools Used
- Microsoft Defender for Cloud
- Microsoft Sentinel (KQL — `AzureActivity`, `SigninLogs`, `IdentityInfo`)
