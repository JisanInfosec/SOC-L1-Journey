# Section 03: Core SOC Solutions

**Platform:** TryHackMe  
**Path:** SOC Level 1  
**Status:** Completed  
**Rooms Completed:** 5/5

---

# Overview

This section introduced the core security solutions used daily by SOC analysts. It covered Endpoint Detection and Response (EDR), Security Information and Event Management (SIEM), Splunk, Elastic Stack (ELK), and Security Orchestration, Automation, and Response (SOAR).

The goal of this section was to understand how security tools collect telemetry, generate alerts, support investigations, and automate incident response workflows.

---

# Room 01: Introduction to EDR

## Objective

Understand the purpose of Endpoint Detection and Response (EDR), how it differs from traditional antivirus solutions, and how analysts investigate endpoint activity using EDR telemetry.

## Key Learning Points

- EDR provides advanced endpoint visibility and protection.
- EDR collects endpoint telemetry through installed agents.
- All endpoint telemetry is centralized within an EDR console.
- EDR classifies alerts using severity levels such as Critical, High, Medium, Low, and Informational.
- Analysts use EDR to investigate suspicious activity and respond to threats.

### Core EDR Capabilities

#### Visibility
Provides detailed endpoint activity through process trees and telemetry records.

#### Detection
Identifies suspicious behavior using:
- Behavioral analysis
- Anomaly detection
- IOC matching
- MITRE ATT&CK mapping
- Machine learning

#### Response
Allows analysts to investigate and respond directly from the EDR platform.

### Common Telemetry Sources

- Process execution
- Process termination
- Network connections
- Command-line activity
- File modifications
- Registry modifications

## Practical Exercise

Used an EDR dashboard to investigate multiple detected threats.

Activities included:

- Exploring process trees.
- Tracking suspicious file downloads.
- Identifying parent-child process relationships.
- Reviewing threat intelligence information.
- Investigating endpoint telemetry associated with malicious activity.

## Skills Acquired

- Navigated an EDR console.
- Investigated endpoint telemetry.
- Analyzed process trees.
- Traced malicious file execution paths.
- Reviewed threat intelligence indicators.

## Interview Question

**Q: What is the difference between traditional antivirus and EDR?**

**A:** Traditional antivirus primarily relies on signature-based detection to identify known malware. EDR provides continuous endpoint monitoring, collects detailed telemetry, detects suspicious behavior, and allows analysts to investigate and respond to threats directly from a centralized console.
---

# Room 02: Introduction to SIEM

## Objective

Understand the role of SIEM platforms in collecting, normalizing, correlating, and analyzing security logs across an organization.

## Key Learning Points

- Security logs originate from multiple systems and devices.
- Raw logs are often decentralized and difficult to analyze.
- SIEM platforms centralize security monitoring.
- SIEM solutions transform large volumes of log data into actionable alerts.

### Core SIEM Functions

#### Log Collection
Collects data from multiple sources.

#### Log Normalization
Converts different log formats into a standardized structure.

#### Log Correlation
Links related events together to provide context.

#### Real-Time Alerting
Generates alerts when detection rules identify suspicious activity.

#### Dashboards and Reporting
Provides visibility through charts, dashboards, and reports.

### Common Log Ingestion Methods

- Agents / Forwarders
- Syslog
- Manual Uploads
- Port-Based Collection

## Practical Exercise

Used a SIEM dashboard to investigate alerts and determine whether they represented malicious activity.

Activities included:

- Reviewing alert details.
- Identifying affected users.
- Investigating host information.
- Determining whether alerts were true or false positives.

## Skills Acquired

- Navigated SIEM dashboards.
- Investigated security alerts.
- Analyzed centralized logs.
- Validated alert severity and legitimacy.

## Interview Question

**Q: Why do organizations use SIEM solutions?**

**A:** SIEM platforms centralize logs from multiple systems, normalize and correlate events, generate alerts, and provide visibility into security activity across the environment. This helps analysts detect and investigate threats more efficiently.
---

# Room 03: Splunk Basics

## Objective

Learn the architecture of Splunk and understand how analysts use it to search and investigate log data.

## Key Learning Points

### Splunk Components

#### Forwarder
Collects and forwards log data.

#### Indexer
Processes and stores incoming data.

#### Search Head
Provides searching, reporting, and visualization capabilities.

### Splunk Workflow

1. Collect data through forwarders.
2. Process and index data.
3. Search and analyze events.
4. Generate reports and dashboards.

## Practical Exercise

Uploaded VPN log data into Splunk and performed investigations using the platform.

Activities included:

- Uploading log files.
- Configuring data ingestion settings.
- Searching indexed events.
- Investigating user activity.
- Identifying source IP information.
- Reviewing event counts and traffic patterns.

## Skills Acquired

- Uploaded data into Splunk.
- Performed basic Splunk investigations.
- Used Splunk search functionality.
- Analyzed VPN log activity.
- Investigated user and IP-based events.

## Interview Question

**Q: What are the three main components of Splunk?**

**A:** Splunk consists of Forwarders, which collect data; Indexers, which process and store data; and Search Heads, which allow analysts to search, visualize, and investigate events.

---

# Room 04: Elastic Stack (ELK) Basics

## Objective

Understand the Elastic Stack architecture and learn how analysts search, filter, and visualize security data.

## Key Learning Points

### Elastic Stack Components

#### Elasticsearch
Stores and searches data.

#### Logstash
Processes and transforms data.

#### Beats
Collects and forwards endpoint data.

#### Kibana
Provides visualization and analysis capabilities.

### ELK Workflow

Beats → Logstash → Elasticsearch → Kibana

### Kibana Features

- Log searching
- Filtering
- Dashboards
- Visualizations
- Investigations

### Introduction to KQL

Learned the basics of Kibana Query Language (KQL).

#### Search Types

- Free-text search
- Field-based search

#### Logical Operators

- AND
- OR
- NOT

## Practical Exercise

Analyzed VPN connection logs using Kibana.

Activities included:

- Filtering data using KQL.
- Investigating user activity.
- Identifying source IPs.
- Building visualizations.
- Creating dashboards.
- Reviewing traffic trends.

## Skills Acquired

- Navigated Kibana.
- Used KQL searches.
- Filtered security data.
- Created visualizations.
- Built dashboards.
- Investigated VPN activity.

## Interview Question

**Q: What is Kibana used for?**

**A:** Kibana is the visualization component of the Elastic Stack. It allows analysts to search logs, create dashboards, build visualizations, and investigate security events.

---

# Room 05: Introduction to SOAR

## Objective

Understand how SOAR platforms automate repetitive security tasks and improve SOC efficiency.

## Key Learning Points

### Challenges Faced by SOC Teams

- Alert fatigue
- Manual processes
- Tool fragmentation
- Limited analyst resources

### SOAR Capabilities

#### Orchestration
Connects multiple security tools into a unified workflow.

#### Automation
Automates repetitive investigation tasks.

#### Response
Supports faster containment and remediation actions.

### Playbooks

SOAR platforms use playbooks to automate investigation and response procedures.

Examples:

- Phishing response workflow
- Vulnerability remediation workflow
- Threat intelligence enrichment workflow

## Practical Exercise

Built a threat intelligence automation workflow.

Activities included:

- Designing an incident response process.
- Determining automated actions.
- Identifying tasks requiring analyst approval.
- Executing a SOAR playbook.

## Skills Acquired

- Understood SOAR workflows.
- Worked with security playbooks.
- Built automated investigation processes.
- Integrated threat intelligence workflows.


# Interview Question

**Q: What problem does SOAR solve for SOC teams?**

**A:** SOAR reduces manual effort by automating repetitive security tasks, integrating multiple security tools, and executing predefined response playbooks. This helps analysts respond to incidents faster and more consistently.
---

# Section 03 Summary

## Topics Covered

- EDR
- SIEM
- Splunk
- Elastic Stack (ELK)
- Kibana
- KQL
- SOAR
- Security Telemetry
- Security Automation

## Key Terms Learned

- EDR
- Telemetry
- Process Tree
- SIEM
- Log Normalization
- Log Correlation
- Splunk
- Forwarder
- Indexer
- Search Head
- ELK
- Elasticsearch
- Logstash
- Beats
- Kibana
- KQL
- SOAR
- Playbook

## Skills Acquired

- Investigated endpoint telemetry using EDR.
- Performed alert analysis using SIEM platforms.
- Uploaded and analyzed data in Splunk.
- Used Kibana and KQL for log investigations.
- Built dashboards and visualizations.
- Designed automated security workflows using SOAR concepts.

## Personal Reflection

This section provided my first practical exposure to the primary security solutions used inside modern Security Operations Centers. I learned how endpoint telemetry is collected, how logs are centralized and analyzed, how analysts investigate suspicious activity using SIEM platforms, and how automation can improve incident response efficiency. These tools form the foundation of daily SOC operations and will be essential throughout the rest of the SOC Level 1 learning path.
