---
description: "SIEM engineering: log onboarding, parsing, normalization, and correlation that make investigations faster and more reliable."
---

# SIEM Engineering

Detections and investigations are only as good as the data underneath them. My SIEM engineering work is about making that data trustworthy: the right sources onboarded, fields parsed and normalized to a common schema, and correlation joins that hold up at 3 AM during an incident. Platforms include Splunk Cloud and Enterprise Security, Google SecOps (Chronicle), Microsoft Sentinel, QRadar, and CrowdStrike NG-SIEM.

---

## Log Onboarding

Every source goes through the same intake: confirm what questions the data must answer, validate volume and timestamp quality, parse the fields investigations actually use, and map them to the common schema before the source counts as onboarded. Identity (Okta), endpoint (CrowdStrike, Windows, macOS through Jamf), cloud (AWS CloudTrail), and network sources all land in this model, so a username or a host means the same thing in every index.

---

## Normalization and Data Quality

Most failed correlations are field problems, not search problems. The standards I hold sources to:

- A consistent `user` and `host` on every source that has them, with case and format differences handled at parse time
- Timestamps in a single time zone convention, checked for skew and late arriving data
- Field validation after every source change, because a silent parsing break looks exactly like a quiet network
- Coverage reviews that compare expected sources against what actually arrived, so gaps surface in a report instead of during an incident

---

## Correlation Work

- [Correlating Okta logins with endpoint activity](okta_windows_user_mapping.md): building a user to device map across Okta, Windows, and macOS logs so identity investigations start with the right host attached

Correlation is treated as shared infrastructure. A join that one investigation needs gets built once, scheduled, saved as a lookup, and documented, so the next analyst starts from the answer instead of rebuilding it.

---

## Why It Matters

This layer is invisible when it works and painful when it does not. Time spent on onboarding standards and data quality is what let detection tuning cut false positives 30 to 40 percent and let wider log coverage improve mean time to detect by 25 percent: the detections got better partly because the data under them got better first.
