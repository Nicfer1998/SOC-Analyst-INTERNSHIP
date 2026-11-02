# [INC-20261101-044] DLP — Medical-Information Policy on Financial Documents (False Positive)

> Confidentiality note: identifiers anonymized. `[user]` is a placeholder.

## Narrative Summary
Microsoft Purview DLP (default engine) fired the **"protected medical information"** policy on consolidated financial-statement PDFs stored in the cloud. The content (financial) does not match the policy (medical), pointing to a false positive of the default classifier.

## Incident Details
| Field | Value |
|---|---|
| Alert | File containing protected medical information detected on the cloud |
| Product | Microsoft Purview DLP (via Sentinel) |
| Severity | Medium |
| Users | [user] (finance / accounting) |
| Application | Cloud storage (OneDrive/SharePoint) |

## Investigation Process
1. **Policy/content mismatch**: a medical policy fired on financial documents. Default classifiers pattern-match; numeric sequences in financial documents can collide with health-identifier patterns.
2. Confirm by inspecting the matched sensitive-info type/content and the users' role (finance).

## Conclusion
**Likely false positive.**

## Recommendation / Lesson
Tune the policy / confidence threshold. Separately, **verify the files were NOT shared externally** (that would be a real, separate concern independent of the medical mis-fire). The nature of this case is governance/decision, not just a verdict.

## Tools Used
- Microsoft Purview DLP
- Microsoft Sentinel
