# [INC-20261101-038] Account Lockout via Smart Lockout (50053) — Contained Credential Attack

> Confidentiality note: identifiers anonymized. `[username]`, `[org-country]` are placeholders.

## Narrative Summary
An account was locked by Entra ID Smart Lockout (error **50053**) after multiple failed sign-in attempts from several IPs in different countries. Analysis confirmed that the only successful sign-ins came from the organization's home country (legitimate user), while all foreign attempts failed and triggered the lockout. Verdict: account under distributed credential-guessing attack, **NOT compromised**.

## Incident Details
| Field | Value |
|---|---|
| Alert | Failed login attempts to Azure Portal involving one user |
| Product | Microsoft Sentinel / Entra ID |
| Severity | Low |
| Tactic (MITRE) | Credential Access (T1110 - Brute Force) |
| User | [username] |
| Error | 50053 - Account locked (Smart Lockout) |

## Investigation Process
1. Error 50053 indicates the account was locked due to too many attempts with incorrect credentials (Smart Lockout) — the defense already acted.
2. Reviewed source IPs: failures came from **multiple countries**. The geographic dispersion ruled out the "stale credential on a device" benign theory.
3. Separated successes from failures: the only successful sign-ins came from the organization's country (`[org-country]`); everything foreign was a failure.
4. Beyond the threat, the external assault repeatedly locked out the legitimate user (DoS-like side effect).

## Conclusion
**Account attacked, NOT compromised.** Distributed credential-guessing attack from foreign IPs; no external access succeeded (Smart Lockout contained it). The legitimate user continues to sign in from their country.

## Recommendation
- **Conditional Access** geo-restriction (allow the organization's country, block the rest) to stop the attack before the password stage and free the user.
- Enforce / verify MFA.
- Note: Smart Lockout (50053) releases on a time delay; it does not require a password change.

## Lesson Learned
"Repeated failures" is not compromise. The defining question: was there any **SUCCESSFUL** sign-in from an external IP? If all foreign attempts fail, the defense held.

## Tools Used
- Microsoft Sentinel (KQL — `SigninLogs`)
- Entra ID Protection
