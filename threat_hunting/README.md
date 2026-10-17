---
description: "Hypothesis driven threat hunting methodology, example hunts across Okta and AWS, tools used, and lessons learned."
---

# Threat Hunting

This section includes practical examples and real-world hunts I've conducted, along with methodology, hypotheses, and key findings. The goal is to proactively identify threats that evade traditional security controls by using log data, threat intel, and behavioral patterns.

---

## 🧠 Hunting Methodology

1. **Establish a Hypothesis**  
   Begin with a question or assumption (e.g., “What if an attacker uses a compromised service account for lateral movement?”)

2. **Define Scope and Data Sources**  
   Focus the hunt using data like EDR (CrowdStrike), cloud audit logs (AWS, GCP, Azure), identity logs (Okta), or proxy/DNS (Netskope, Zscaler).

3. **Develop Queries**  
   Use SIEM or data lake queries to test the hypothesis. Tools include:
   - Splunk
   - Google Chronicle (SecOps)
   - QRadar
   - CrowdStrike NGSIEM

4. **Analyze and Pivot**  
   Investigate anomalies, pivot on fields like `username`, `IP`, `command_line`, and correlate across sources.

5. **Document and Escalate**  
   Summarize the hunting results, escalate confirmed findings to IR, and create detection rules for recurrence.

---

## 🔍 Example Hunt: Suspicious Okta MFA Fatigue Attempts

**Hypothesis**: An attacker is trying to exploit MFA fatigue by repeatedly triggering push notifications.

**Data Source**:  
- Okta logs (via Splunk index: `okta`)  
- Chronicle SecOps enriched with IP reputation

**Indicators**:  
- Multiple MFA challenge failures  
- User location anomalies  
- Known bad IP reputation (VirusTotal / GreyNoise)

**SPL Example**:
```spl
index=okta eventType="user.mfa.challenge.failed"
| stats count by user, client.geographicalContext.country, outcome.reason, ipAddress
| where count > 5
```

**Outcome**:  
User alerted, account temporarily locked, threat modeled in MITRE ATT&CK under **T1110.003** (Multi-factor Authentication Request Generation).

---

## 🔍 Example Hunt: Unusual AWS Console Access Locations

**Hypothesis**: A compromised IAM user is logging in from an unusual country.

**Data Source**:  
- AWS CloudTrail logs in Splunk (`aws_cloudtrail` index)  
- IP enrichment from AbuseIPDB

**SPL Example**:
```spl
index=aws_cloudtrail eventName="ConsoleLogin" responseElements.mfaUsed=*
| lookup ip_intel ip as sourceIPAddress OUTPUT risk_score
| where risk_score > 70 OR sourceIPAddressCountry != "United States"
| table userIdentity.arn, sourceIPAddress, risk_score, eventTime
```

**Outcome**:  
Confirmed unauthorized login attempt from Russia using valid credentials; credentials rotated and IAM access reviewed.

---

## 🧰 Tools Used

- **CrowdStrike Falcon** (EDR and telemetry)
- **Splunk** (core hunting and correlation)
- **Google Chronicle (SecOps)** (retrospective IOC and threat intel matching)
- **Netskope** (proxy activity and SSL inspection)
- **Okta** (authentication trail and anomalies)
- **VirusTotal, GreyNoise, AbuseIPDB** (external enrichment)

---

## 📌 Lessons Learned

- Establishing baselines dramatically improves signal-to-noise ratio.
- Linking threat hunts to MITRE techniques helps prioritize detection engineering work.
- Regular retrospective hunts using new threat intel enhances coverage against advanced threats.

---

## 📂 Hunt themes in rotation

- DNS-based threat hunts
- Hunts for unusual system processes on macOS via CrowdStrike telemetry
- Behavioral hunts using GCP logs and Chronicle

