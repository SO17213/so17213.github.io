---
description: "Tools built by Segun Oladipo: endpoint scripting for Intune and Python automation for vulnerability management."
---

# Tools

I build small tools where manual work repeats. The test for building one is simple: the task happens often, the steps are known, and a script or automation makes it faster or more consistent than doing it by hand. Two examples are described here, one endpoint script and one automation that runs a program.

---

## PowerShell: Retiring McAfee from Managed Endpoints

- [intune_disable_mcafee.ps1](intune_disable_mcafee.ps1): PowerShell script that stops and disables McAfee services and removes related registry keys on managed Windows endpoints

The script is built for deployment through Microsoft Intune under the SYSTEM account. It checks for each known McAfee service, including the firewall host, update, and WebAdvisor services, stops and disables what it finds, removes the associated registry keys, and skips anything that is not present instead of failing. Every step writes output, so the Intune deployment report shows exactly what happened on each machine.

It exists for a common operational moment: a legacy agent has to come off a fleet cleanly, at scale, without touching machines one at a time and without leaving half removed services behind to fight the replacement tooling.

---

## Python: Vulnerability Management Automation

Finding vulnerabilities is the easy half of vulnerability management. The slow half is turning scan output into owned, tracked, verified fixes. I built Python automation for that half: it takes scanner results and creates Jira tickets for the findings, with severity attached and each ticket routed to the team that owns the affected system.

Owners get a ticket they can act on instead of a spreadsheet row. The program then tracks tickets to verified closure and reports status by owner, which is how critical vulnerability closure reached 95 percent within SLA. The automation also removes the weekly manual work of exporting scan results, sorting them, and chasing people by email, and it keeps a consistent record of what was found, who owned it, and when it closed.

The broader automation approach, including SOAR examples, is in [Security Automation](https://so17213.github.io/automation/).
