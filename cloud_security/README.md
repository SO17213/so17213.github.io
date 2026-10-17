---
description: "Cloud security monitoring across AWS, GCP, and Azure: GuardDuty and CloudTrail hunts, identity protections, and guardrails."
---

# Cloud Security

Cloud security work here centers on AWS, with the same discipline applied across GCP and Azure: log everything that matters, watch identity first, and put guardrails in place so common mistakes fail safe. The goal is early warning on the small set of cloud behaviors that precede real damage: credential theft, privilege escalation, public exposure, and data access from the wrong place.

---

## Monitoring Model

AWS telemetry comes from CloudTrail for API activity, GuardDuty for managed threat findings, and VPC Flow Logs for network context, all centralized in Splunk for correlation with identity and endpoint data. GCP work uses Cloud Logging and Security Command Center, and Azure uses Defender and Sentinel. Findings are not triaged in isolation: a GuardDuty alert gets checked against CloudTrail history, the identity that acted, and what the affected resource was doing before the alert fired.

---

## Featured Hunt

- [AWS GuardDuty threat hunt](aws_guardduty_hunt.md): triage of an instance credential exfiltration finding, from the GuardDuty alert through CloudTrail pivots and VPC Flow Log checks to a confirmed outcome, with the fixes that followed

The hunt shows the pattern I use for cloud findings: confirm the finding, reconstruct the API trail, check network context, then decide malicious, misconfigured, or expected.

---

## Identity and Metadata Protections

Cloud incidents usually start with credentials, so identity gets the most attention:

- Least privilege IAM, reviewed through recurring access reviews across cloud and SaaS; that review program cut stale and unused account access by 75 percent
- Alerts on high value API calls such as `GetCallerIdentity` and `ListBuckets` when they come from unexpected roles or addresses
- Instance metadata protections: require IMDSv2 so stolen web requests cannot reach the older metadata endpoint, and restrict IAM roles through trust policy conditions
- Fast revocation paths for exposed keys, with the hunt and the fix documented the same day

---

## Guardrails

Detection alone is not enough. Guardrails make the safe path the default: centralized logging that new accounts inherit, alerting on public exposure of storage and security groups, and preventive policy for the misconfigurations that show up again and again in incidents. When a hunt or an incident exposes a gap, the follow up is a guardrail or a new detection, not just a closed ticket.

Related work: [Threat Hunting](https://so17213.github.io/threat_hunting/) for hunt method, and [Detection Engineering](https://so17213.github.io/detection_engineering/) for how cloud findings become standing detections.
