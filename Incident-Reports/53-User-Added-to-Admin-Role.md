# [INC-20261101-053] User Added to Admin Role (Benign)

> Confidentiality note: identifiers anonymized. `[admin-user]`, `[target-user]` are placeholders.

## Narrative Summary
Microsoft Sentinel raised a privilege-escalation alert for adding a user to a privileged role. A **recurring** alert with prior instances closed as benign. Confirmed it was the organization's administrator assigning a role legitimately.

## Incident Details
| Field | Value |
|---|---|
| Alert | User Added to Admin Role |
| Product | Microsoft Sentinel |
| Severity | Low |
| Tactic (MITRE) | Privilege Escalation |
| Source | [admin-user] |
| Target | [target-user] |

## Investigation Process
1. Verified **who / what / when** against the known pattern: same admin as source, expected target user, during business hours on a normal day — matching the previous alerts.
2. The **role** granted also matters: the same actor and target can warrant a second look if the privilege granted is unusually powerful.

## Conclusion
**Benign positive** — legitimate role assignment by the administrator.

## Recommendation / Lesson
An "always benign" history is a **map of what's normal, not permission to stop looking** (alert fatigue / normalization bias). It's the perfect hiding spot for an attacker who has already compromised the environment. Use the pattern as a reference, not an automatic answer.

## Tools Used
- Microsoft Sentinel
