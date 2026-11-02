# [INC-20261101-048] New OAuth Application Consented in the Tenant (Governance)

> Confidentiality note: identifiers anonymized. `[tenant]` is a placeholder.

## Narrative Summary
Microsoft Defender for Cloud Apps notified of consent to a **new third-party OAuth application** (an AI meeting assistant) connected to cloud mail/calendar. This is not a benign-vs-malicious triage but a **governance decision**.

## Incident Details
| Field | Value |
|---|---|
| Alert | Create an OAuth app policy to notify you about new OAuth applications |
| Product | Microsoft Defender for Cloud Apps |
| Severity | Low |
| Application | Third-party OAuth app (AI meeting assistant) |

## Investigation Process
1. When a user consents to an OAuth app, it receives a token and permissions in the tenant. The policy monitors new apps because of **OAuth consent phishing** risk (a consented malicious app = persistent token access that survives password change and MFA).
2. The app is legitimate but requests broad permissions and sends content to a third party → Shadow IT + privacy.

## Conclusion
**Governance review** (not benign-vs-malicious).

## Recommendation / Lesson
Verify it is the **GENUINE** app (verified publisher / app id) and not a look-alike used for consent phishing. Review scopes and user-vs-admin consent; decide sanction/restrict/block; consider restricting user consent tenant-wide.

## Tools Used
- Microsoft Defender for Cloud Apps
- Microsoft Entra ID (Enterprise applications)
