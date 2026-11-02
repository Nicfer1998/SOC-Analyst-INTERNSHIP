# [INC-20261101-047] Suspected Password Guessing — Scanning Service Account (Benign)

> Confidentiality note: identifiers anonymized. `[scanner-account]`, `[internal-ip]` are placeholders.

## Narrative Summary
A **"Suspected password guessing"** rule fired on authentication failures (4625) from multiple source IPs. The account involved is a scanning-tool service account; the failures came from its internal collector nodes with a stale credential. Benign.

## Incident Details
| Field | Value |
|---|---|
| Alert | Suspected password guessing attempts in Active Directory |
| Product | Microsoft Sentinel (SecurityEvent 4625) |
| Severity | Medium |
| Tactic (MITRE) | Credential Access (T1110) |
| Account | [scanner-account] |

## Investigation Process
1. The **account name** gives away a scanning/service account (vuln scanner / security tool). These authenticate against many hosts from several collectors → naturally trip the "multiple source IPs" rule.
2. The source IPs were **ALL internal** (private) → argues against a distributed external attack.
3. 4625 drill-down: SubStatus **0xC000006A** (wrong password), LogonType 3 (Network). Cause: the scanner's stale credential configured in its tool, retrying across the network.

## Conclusion
**Benign positive** — scanning service-account misconfiguration.

## Recommendation / Lesson
**Root-cause fix**: update the scanner's stored credential in the tool; then exclude / raise the threshold. Validate the source IPs are the known scanner nodes and the account isn't being abused (a "scanner" account has broad reach → juicy target).

## Tools Used
- Microsoft Sentinel (KQL — SecurityEvent 4625)
