# Section 02: SOC Team Internals

**Platform:** TryHackMe  
**Path:** SOC Level 1  
**Status:** Completed  
**Rooms Completed:** 4/4

---

## Overview

This section focused on how a Security Operations Center (SOC) functions internally. It introduced alert triage, reporting and escalation procedures, SOC workbooks, asset inventories, network diagrams, and performance metrics used to measure SOC effectiveness.

### Rooms Completed

1. SOC L1 Alert Triage
2. SOC L1 Alert Reporting
3. SOC Workbooks and Lookups
4. SOC Metrics and Objectives

---

# Room 01: SOC L1 Alert Triage

## Objective

Understand how an L1 SOC Analyst reviews, prioritizes, investigates, and validates security alerts.

## Key Learning Points

- Difference between events and alerts.
- Important alert fields such as severity, timestamp, description, and source.
- Alert prioritization strategies.
- The workflow used by L1 analysts during alert triage.
- Importance of proper documentation.

## SOC Team Responsibilities

### L1 Analyst
- Monitor alerts.
- Perform initial investigation.
- Determine if alerts are true or false positives.
- Escalate confirmed threats.

### L2 Analyst
- Perform deeper investigations.
- Execute remediation actions.
- Handle escalated incidents.

### SOC Engineer
- Configure and maintain detection rules.
- Ensure alerts contain useful information.

### SOC Manager
- Monitor triage quality.
- Track team performance and response times.

## Alert Prioritization Methods

Analysts commonly prioritize alerts using:

1. Severity
2. Time of occurrence
3. Alert filtering and categorization

This helps ensure critical threats receive immediate attention.

## Practical Exercise

A simulated SOC dashboard contained multiple pending alerts with different severity levels.

Tasks completed:

- Reviewed queued alerts.
- Prioritized alerts based on severity.
- Assigned alerts for investigation.
- Validated alerts as true or false positives.
- Added investigation comments and documentation.

## Interview Question

**Q: What is the primary goal of alert triage?**

**A:** Alert triage helps analysts quickly identify and prioritize genuine threats while filtering out false positives, ensuring that critical incidents receive immediate attention.

---

# Room 02: SOC L1 Alert Reporting

## Objective

Learn how SOC analysts document investigations and communicate findings effectively through reporting and escalation.

## Key Learning Points

- Importance of reporting and communication.
- Escalation procedures.
- Writing clear analyst notes.
- Using structured reporting methodologies.
- Handling common communication challenges.

## The Five Ws Reporting Method

Every alert report should answer:

### Who
Which user, account, or system was involved?

### What
What activity occurred?

### When
When did the activity begin and end?

### Where
Which device, IP address, application, or location was involved?

### Why
Why was the activity classified as malicious or suspicious?

## Communication Scenarios Learned

- Escalating critical incidents when senior analysts are unavailable.
- Contacting compromised users through alternative communication methods.
- Managing high alert volumes during busy periods.
- Correcting previous misclassifications.
- Handling missing or incomplete log data.

## Practical Exercise

Tasks completed:

- Investigated new alerts.
- Validated alert legitimacy.
- Wrote structured investigation notes.
- Escalated confirmed threats to L2 analysts.
- Applied the Five Ws methodology.

## Interview Question

**Q: Why is documentation important in a SOC?**

**A:** Clear documentation allows senior analysts to understand the investigation quickly, reduces response time, and ensures important evidence is preserved.

---

# Room 03: SOC Workbooks and Lookups

## Objective

Understand how SOC analysts use workbooks, inventories, and network diagrams during investigations.

## Key Learning Points

- Importance of asset inventory.
- Importance of identity inventory.
- Understanding network diagrams.
- Following investigation playbooks and workbooks.
- Standardizing investigations across the SOC.

## SOC Workbooks

SOC workbooks provide predefined investigation procedures for specific alerts and incidents.

Benefits include:

- Consistent investigations.
- Faster response times.
- Reduced analyst mistakes.
- Improved escalation quality.

## Asset and Identity Inventories

### Asset Inventory

Tracks:

- Servers
- Workstations
- Applications
- Network devices

### Identity Inventory

Tracks:

- Users
- Accounts
- Privileges
- Department ownership

These inventories help analysts understand the importance and ownership of affected assets.

## Practical Exercise

Completed multiple decision-tree exercises involving:

- Email investigations
- PowerShell investigations
- Network investigations

The objective was to arrange investigation steps in the correct operational sequence.

## Interview Question

**Q: Why are SOC playbooks important?**

**A:** Playbooks provide standardized investigation procedures that improve consistency, reduce response time, and help analysts handle incidents effectively.

---

# Room 04: SOC Metrics and Objectives

## Objective

Understand how SOC performance is measured and how security teams improve operational effectiveness.

## Key Learning Points

- SOC performance metrics.
- Alert management metrics.
- False positive reduction.
- Response time measurements.
- Team performance evaluation.

## Core SOC Metrics

### Alert Count

Measures the number of alerts handled by analysts.

### False Positive Rate

Measures how many alerts are incorrectly identified as threats.

High false positive rates create alert fatigue and reduce efficiency.

### Alert Escalation Rate

Measures how often L1 analysts escalate alerts.

This metric helps evaluate analyst independence and experience.

### Threat Detection Rate

Measures how effectively real threats are identified.

The goal is to detect every legitimate threat.

## Time-Based Metrics

### Mean Time to Detect (MTTD)

Average time required to detect an attack.

### Mean Time to Acknowledge (MTTA)

Average time required for an analyst to begin investigating an alert.

### Mean Time to Respond (MTTR)

Average time required to contain or stop an incident.

## Practical Exercise

Acted as a SOC manager responsible for improving team performance.

Tasks included:

- Reviewing analyst performance data.
- Identifying operational weaknesses.
- Selecting appropriate improvements.
- Assigning responsibility to the correct team members.

## Interview Question

**Q: What is the difference between MTTD and MTTR?**

**A:** MTTD measures how quickly an attack is detected, while MTTR measures how quickly the organization responds to and contains the attack.

---

# Section 02 Summary

## Topics Covered

- Alert Triage
- Alert Reporting
- Incident Escalation
- SOC Communication
- SOC Workbooks
- Asset Inventory
- Identity Inventory
- Network Diagrams
- SOC Metrics
- SOC Performance Improvement

## Key Terms Learned

- Alert Triage
- True Positive
- False Positive
- Escalation
- Playbook
- Asset Inventory
- Identity Inventory
- SLA
- MTTD
- MTTA
- MTTR

## Skills Acquired

- Prioritized alerts based on severity and impact.
- Performed structured alert investigations.
- Wrote escalation reports using the Five Ws methodology.
- Followed SOC workbooks and investigation playbooks.
- Used asset and identity inventories during investigations.
- Evaluated SOC performance using operational metrics.

## Personal Reflection

This section helped me understand how a SOC operates beyond simply reviewing alerts. I learned how investigations are documented, how incidents are escalated, why workbooks are important, and how SOC teams measure their performance using industry-standard metrics. The practical exercises provided a realistic introduction to the workflow followed by SOC analysts during daily operations.
