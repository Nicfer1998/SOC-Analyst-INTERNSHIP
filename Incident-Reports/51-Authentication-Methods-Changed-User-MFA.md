# [INC-20261101-051] Authentication Methods Changed — User Self-Registered MFA (Benign)

> Confidentiality note: identifiers anonymized. `[user]`, `[external-ip]` are placeholders.

## Narrative Summary
Microsoft Sentinel raised a persistence alert for the registration of a **new authentication method** (e.g. FIDO2 key) on an account. Confirmed that the user themselves registered the method from their habitual context. Benign.

## Incident Details
| Field | Value |
|---|---|
| Alert | Authentication methods changed |
| Product | Microsoft Sentinel |
| Severity | Low |
| Tactic (MITRE) | Persistence |
| Account | [user] |

## Investigation Process
1. Adding an MFA method is a **persistence technique** (an attacker registers their own method to keep access). Hence it is monitored.
2. Confirmed the **user themselves** initiated the registration, from a normal self-service flow (Authenticator app), from an IP/country consistent with their habitual pattern, with no prior suspicious activity. (For an external collaborator, a "foreign country" is normal because that is where they work — the user's context defines what is anomalous.)

## Conclusion
**Benign positive** — legitimate MFA method registration by the user (a security-positive action).

## Recommendation / Lesson
"The account uses MFA" alone does not prove the change was the owner's; verify the registration against the user's historical IP/device pattern and rule out prior compromise.

## Tools Used
- Microsoft Sentinel (KQL — SigninLogs, AuditLogs)
- VirusTotal / AbuseIPDB (IP reputation)
