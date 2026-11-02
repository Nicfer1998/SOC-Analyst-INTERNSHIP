# [INC-20261101-037] curl.exe Detection — Legitimate Download (False Positive)

> Confidentiality note: all client identifiers are anonymized. Bracketed values (`[hostname]`, `[username]`) are placeholders.

## Narrative Summary
A client custom rule raised an alert on the use of `curl.exe` on an endpoint, mapped to the **Command and Control** tactic (MITRE T1105). curl is a legitimate utility bundled with Windows, but dual-use: attackers use it to download additional tooling or exfiltrate data. Analysis of the command line confirmed it was downloading the legitimate installer of a commercial application from the vendor's official domain. Classified as benign / false positive, with a Shadow-IT note.

## Incident Details
| Field | Value |
|---|---|
| Alert | Custom Alert: Detection of CURL.EXE usage |
| Product | Microsoft Defender for Endpoint / Microsoft Sentinel |
| Severity | Low |
| Tactic (MITRE) | Command and Control (T1105 - Ingress Tool Transfer) |
| Host | [hostname] |
| User | [username] |

## Investigation Process
1. The rule fires on any `curl.exe` execution due to its C2 potential. The decisive data point is not "curl ran" but **what command it ran**.
2. Event 4688 (process creation) was not available in `SecurityEvent` for this host (no command-line auditing / no forwarding) — confirmed with a 4688 query returning no results.
3. Pivoted to the Defender for Endpoint table (`DeviceProcessEvents`) and retrieved the `ProcessCommandLine`.
4. The command line showed `curl` downloading the **legitimate installer of a commercial writing-assistant application** from the **vendor's official domain**, over HTTPS, with retry and error-handling parameters (typical of an automated installer). The destination path corresponded to the application's own install process.

## Conclusion
**False positive / Benign.** curl performed a legitimate download of a known application's installer from its official domain; no Command and Control activity.

**Note (Governance / Shadow IT):** the application is user-installed third-party software that sends content to the cloud. Flagged for the client to confirm whether its use is approved or should be restricted.

## Lesson Learned
The name of a dual-use tool (`curl`) means nothing on its own; the **command line / destination URL** is what decides. When visibility is missing in one table, pivot to another telemetry source (Defender) before concluding.

## Tools Used
- Microsoft Sentinel (KQL — `SecurityEvent`, `DeviceProcessEvents`)
- Microsoft Defender for Endpoint
