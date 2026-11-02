# [INC-20261101-052] "Successful Sign-in from Untrusted Country" — Failed Attempt on a Disabled Account (No Compromise)

> Confidentiality note: identifiers anonymized. `[user]`, `[external-ip]` are placeholders.

## Narrative Summary
An alert titled **"Successful sign-in from untrusted country"** turned out, on verification, to be a **FAILED** sign-in (ResultType 50012) from abroad against an external account that never had a successful login. No compromise.

## Incident Details
| Field | Value |
|---|---|
| Alert | Successful sign-in from untrusted country |
| Product | Microsoft Sentinel |
| Severity | Medium |
| Account | [user] (external) |
| IP | [external-ip] (untrusted country) |

## Investigation Process
1. The **title** said "successful", but the log showed ResultType **50012** (Authentication failed) — the sign-in FAILED.
2. The account showed **0 successful / 29 failed** sign-ins over 90 days → it never had a successful login, strongly indicating a disabled/deactivated account (confirm in Entra ID). The "6 alerts in 1 incident" were multiple detections on a single event.

## Conclusion
**Failed access attempt from an untrusted country against a (likely) disabled account — no compromise.**

## Recommendation / Lesson
Don't trust the alert **TITLE** ("successful") — verify the result code. "Failed" is proven; "disabled" is inferred from the 0/29 history and should be confirmed in Entra.

## Tools Used
- Microsoft Sentinel (KQL — SigninLogs, IdentityInfo)
