# [INC-20261101-041] Anonymous IP Sign-ins — Context Decides

> Confidentiality note: identifiers anonymized. `[username]`, `[external-ip]` are placeholders.

## Narrative Summary
Entra ID Protection **"Anonymous IP address"** alert (Tor / anonymizing VPN), Initial Access tactic. The same alert type resolved to different verdicts depending on context, showing that the IP's reputation and whether it fits the user's pattern — not the alert name — decide the case.

## Incident Details
| Field | Value |
|---|---|
| Alert | Anonymous IP address |
| Product | Microsoft Entra ID Protection (via Sentinel) |
| Severity | Medium |
| Tactic (MITRE) | Initial Access |
| User | [username] |
| IP | [external-ip] |

## Investigation Process and Scenarios
Verification reflex: (1) IP reputation and type; (2) whether it is habitual for the user; (3) success + MFA; (4) other risk detections; (5) post-sign-in activity (mailbox forwarding rules = classic account-takeover footprint).

- **Benign:** successful sign-in with MFA, from a VPN/hosting IP consistent with the user's usage; app accessed = the user's normal line-of-business application → legitimate user on a VPN.
- **Contained attack (true positive):** distributed guessing from several external IPs (incl. a commercial VPN); password guessed from one IP but the sign-in was stopped at **MFA** → credential compromised, blocked by MFA. Action: password reset, verify no MFA-bypass path, revoke sessions.
- **Unresolved / suspicious:** sign-in from a hosting/datacenter IP that did **NOT** match the user's pattern (different habitual device/location), used exclusively by that account → escalated, pending mailbox-rule review.

## Conclusion / Lesson
An IP/country/VPN is benign or suspicious depending on whether it **fits the user's history and role**. MFA is the barrier that separates "attacked" from "compromised": a user generally using MFA does not prove a specific sign-in is the owner's; verify success + MFA on that event.

## Tools Used
- Microsoft Sentinel (KQL — `SigninLogs`, `AADNonInteractiveUserSignInLogs`, `AADUserRiskEvents`, `IdentityLogonEvents`)
- Entra ID Protection
- VirusTotal / AbuseIPDB (IP reputation)
