# [INC-20261101-045] Recurring AD Authentication Failures — Stale Cached Credential (Systemic Issue)

> Confidentiality note: identifiers anonymized. `[service-host]`, `[internal-ip]` are placeholders.

## Narrative Summary
"Multiple failures + success" / "brute force" rules on Active Directory fired **recurrently** for several accounts. Analysis showed these are stale credentials cached on an internal application server retrying via NTLM, while legitimate activity succeeds via Kerberos. Not an attack; a systemic credential-synchronization issue.

## Incident Details
| Field | Value |
|---|---|
| Alert | Multiple authentication failures followed by a success / Brute force on AD |
| Product | Microsoft Sentinel (SecurityEvent 4625/4624) |
| Severity | Low/Medium |
| Tactic (MITRE) | Credential Access (T1110) |
| Source host | [service-host] ([internal-ip]) |

## Investigation Process
1. Failures (4625) came via **NTLM** with "bad password" (0xC000006A) from a single internal host; successes (4624) via **Kerberos** from other internal hosts. Two unrelated things the rule groups together.
2. Root cause: a service/task on the application server holds a stale cached password and retries in a loop, repeatedly locking out the account (4740).
3. Key verification (lesson): confirm **ALL** source IPs are internal. In one case the "external IP" turned out to be a legitimate email-security service (benign) — but it still had to be verified.

## Conclusion
**Benign positive.** Stale cached credential retrying; legitimate activity via Kerberos. Affecting multiple accounts = systemic issue on an internal server.

## Recommendation / Lesson
Escalate as a **systemic issue**: IT should audit the source server and update the cached credentials of ALL affected service accounts/tasks at once, rather than closing tickets one by one. The "internal vs external" split is the fastest triage.

## Tools Used
- Microsoft Sentinel (KQL — SecurityEvent 4624/4625/4740)
