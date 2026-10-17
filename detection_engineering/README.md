---
description: "Detection engineering lifecycle, tuning discipline, and MITRE ATT&CK coverage, with a worked Okta MFA detection example."
---

# Detection Engineering

I run detection engineering as a lifecycle, not a pile of rules. Every detection has a reason to exist, an owner, a test history, and a measured false positive rate. Coverage spans identity, endpoint, cloud, and SaaS, and every rule maps to MITRE ATT&CK so gaps are visible and plannable instead of accidental.

---

## The Lifecycle

1. **Intake**  \
   New detections come from threat intelligence, incident findings, threat hunts, red team results, and analyst pain points. If nothing is driving a rule, it does not get built.

2. **Hypothesis and data check**  \
   State the behavior to catch in one sentence, then confirm the logs actually contain the fields needed to see it. Missing data becomes a logging request, not a weak rule.

3. **Build and test**  \
   Write the logic, run it against historical data, and check what it would have fired on. A detection that pages on normal admin work is not ready.

4. **Deploy with context**  \
   Ship with severity, triage steps, and escalation guidance attached, so the analyst on shift knows what a fire means and what to do first.

5. **Tune and measure**  \
   Track fires, true positives, and false positives per rule. Noisy rules get tuned, narrowed, or retired. Quiet rules get reviewed for dead data feeds.

---

## Tuning Discipline

Alert noise is a program failure, not an analyst problem. The SOC feeds tuning work back through tickets with examples attached, and detection reviews look at the noisiest rules first. Rebuilding tuning and triage standards cut alert false positives 30 to 40 percent, and new detections with wider log coverage improved mean time to detect by 25 percent. Those numbers are the standard the program is held to: if quality slips, the metrics show it early.

---

## MITRE ATT&CK Coverage

Each detection maps to the ATT&CK technique it covers. The map drives planning in both directions: incident and hunt findings expose uncovered techniques, and coverage reviews expose techniques with rules that never fire. Priority goes to techniques seen in real incidents and in intelligence reporting against similar environments, not to filling the matrix for its own sake.

Platforms: Splunk Cloud and Enterprise Security, Microsoft Sentinel, Google SecOps (Chronicle), QRadar, and CrowdStrike NG-SIEM, with Sigma and KQL used where they fit the platform.

---

## Worked Example

- [Okta MFA bypass detection](okta_mfa_bypass_detection.md): detection logic for MFA bypass and session token abuse in Okta, including the SPL, triage process, and tuning notes

More correlation work that detections depend on is in [SIEM Engineering](https://so17213.github.io/SIEM_Engineering/), and the hunting method that feeds new detections is in [Threat Hunting](https://so17213.github.io/threat_hunting/).
