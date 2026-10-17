---
layout: page
title: "Leading Security Programs"
permalink: /leadership/
description: "Case study in leading security programs: rebuilding detection, response, automation, and access governance with measured results."
---

# Leading Security Programs

This page is a case study in how I lead: one detection and response program, taken from noisy and reactive to measured and proactive. The work is sanitized, but the problems, the approach, and the results are real.

---

## The Problem

The program I inherited had three linked problems. Alert volume was high and much of it was noise, so analysts spent their shifts closing false positives instead of investigating. Detection depended on default vendor content and whatever logs happened to be onboarded, so real behavior slipped past quietly. And access had accumulated for years: accounts and permissions nobody used anymore, spread across cloud and SaaS, with no recurring review to clean them up.

Each problem fed the others. Noise hid slow detection. Slow detection meant incidents surfaced late. Stale access meant any single compromised account carried more rights than it should.

---

## The Approach

**Detection engineering as a discipline.** I rebuilt detection work around a lifecycle: every rule has a hypothesis, a test against historical data, triage guidance, and a measured false positive rate. Tuning and triage standards were rewritten with the SOC, and noisy content was tuned or retired instead of tolerated. Coverage was mapped to MITRE ATT&CK so investment went to gaps that mattered.

**Automation where the work was repetitive.** SOAR and scripting took over the predictable volume: informational threat intel cases auto resolved in CrowdStrike Fusion, repetitive detection tickets categorized and closed through Jira automation, and chat updates synced into Chronicle cases so status stopped living in side conversations. Analysts kept judgment calls; machines took the clerical ones.

**Access governance on a schedule.** Access reviews became recurring across cloud and SaaS instead of annual and theatrical. Owners had to justify access or lose it, and stale and unused access was removed by default.

**Vulnerability management with closure tracking.** Scanning alone does not fix anything, so the program tracked findings to verified closure with automated tracking and owner reporting, sequenced by severity and real exposure.

**AI security governance.** As AI tools entered the environment, I led governance assessments against the NIST AI Risk Management Framework, covering prompt injection, data leakage through retrieval systems, and model supply chain risk, so adoption moved at business speed with security review built in rather than bolted on.

---

## The Results

- Alert false positives cut 30 to 40 percent through rebuilt detection tuning and triage standards
- Mean time to detect improved 25 percent through new detections and wider log coverage
- Stale and unused account access reduced 75 percent through recurring access reviews
- Critical vulnerability closure driven to 95 percent within SLA using automated tracking and owner reporting

The numbers matter less than the habit behind them: every part of the program reports a measure, and the measures decide where the next hour of work goes.

---

## How I Lead the People Side

Standards are written down: triage expectations, escalation paths, and what a good case note looks like. Analysts develop through real casework with review and feedback, post incident reviews that produce fixes instead of blame, and tabletop exercises, including ransomware readiness sessions with executives, that rehearse decisions before a real night demands them. Incident command stays calm and explicit: roles named, updates on a cadence, and decisions logged as they are made.

The detail behind this case study is across the site: [Detection Engineering](https://so17213.github.io/detection_engineering/), [Security Automation](https://so17213.github.io/automation/), [Incident Response](https://so17213.github.io/incident_response/), and the [Security Operations Center](https://so17213.github.io/security_operation_center/).
