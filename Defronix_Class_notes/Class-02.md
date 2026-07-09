# 📝 Class 02: Human Attack Vectors, Process & Technology

## 🎯 Objective
To establish a foundational understanding of the core pillars of a Security Operations Center (SOC) and to define the precise operational boundaries, triage frameworks, and defensive technologies used to mitigate human risk factors.

---

## 🧭 Key Concepts
* **The Core Triad Matrix:** A mature security environment depends entirely on the synchronized coexistence of specialized personnel (**People**), standardized playbooks (**Process**), and central deployment platforms (**Technology**).
* **The First Responder Boundary:** The Tier-1 analyst serves as the initial collection engine, executing fundamental alert triage and ticket documentation before passing complex indicators up the operational hierarchy.
* **Psychological Exploitation Vectors:** Social engineering focuses on bypassing corporate network perimeters by manipulating human emotions—such as urgency or fear—rather than exploiting technical infrastructure vulnerabilities.

---

## 🔤 Key Terms Learned
* **Alert Triage:** The initial operational phase where a security event is ingested, reviewed, and categorized to determine if it requires escalation or immediate closure.
* **True Positive:** A verified security alert that represents an actual malicious event or unauthorized corporate policy violation.
* **False Positive:** A benign security alert triggered by legitimate user behavior or an improperly tuned detection rule logic.
* **Social Engineering:** The tactical manipulation of human psychology to trick users into providing access credentials, executing unauthorized downloads, or exposing sensitive corporate networks.

---

## 👨‍💻 SOC Analyst Perspective
As a future L1 analyst, my responsibility during this operational phase is to:
* Continuously monitor the central dashboards for incoming telemetry exceptions and analyze raw logs to validate alerts.
* Execute basic triage workflows, document the complete administrative context of an event, and systematically escalate high-severity indicators to Tier-2 or Incident Response teams.

---

## 📋 Detailed Notes
### 1. Architectural Structure of a SOC Team
* **Hierarchical Escalation Paths:** Incidents move from initial logging at Tier-1, through deep historical correlation at Tier-2, to advanced containment, eradication, and recovery strategies handled by Tier-3 and specialized Incident Response teams.
* **Supporting Engineering Elements:** Security Engineers manage the ongoing deployment, configuration, and stability of your security software stacks, while Detection Engineers focus specifically on maintaining the alert logic rules within the detection engines.
* **Specialized Crisis Handlers:** When an active network breach expands beyond the standard handling capacity of an internal SOC, a dedicated Cyber Incident Response Team (CIRT/CSIRT) or national CERT handles the threat independently of routine tool telemetry.

### 2. Standard Triage and Reporting Workflows
* **The 5 Ws Diagnostic Engine:** Every alert ticket must answer five specific criteria before escalation or closure: **Who** initiated the action, **What** artifact or malware type was detected, **When** the timestamp occurred, **Where** the asset path resides, and **Why** the file executed.
* **Definitive Ticket Documentation:** A standard ticket template requires a comprehensive breakdown of the 5 Ws combined with a logical description of the analysis and raw screenshots attached directly as formal investigation evidence.

### 3. Core Technologies & Human Defense Mechanisms
* **SIEM Core Capabilities:** The central tool acts exclusively as an aggregation and correlation platform that compiles logs from different network sources to fire alerts based on pre-defined matching rules.
* **EDR/XDR Capabilities:** Endpoint agents provide real-time and historical host behavior visibility, allowing analysts to isolate infected systems or respond to malicious process loops with a few clicks.
* **The "Trust but Verify" Principle:** A strict operational standard requiring that all communications, system validation steps, or credential requests be authenticated through secondary administrative controls.

---

## 🛠️ Tools Mentioned
* **SIEM / Logging Platforms:** Wazuh (Open-Source), Splunk (Industry context).
* **Network / Endpoint Visibility:** Enterprise EDR, XDR, EPP, Firewalls, and IDS/IPS categories.
* **Threat Community Tools:** Cisco Talos Fish Tank (Public repository for live phishing verification).
* **Ticketing Systems:** Enterprise-grade incident ticketing platforms.

---

## 🔬 Lab Evidence
No hands-on lab was associated with this theoretical session.

**Future practical validation areas planned:**
* Validate Wazuh agent deployment and log parsing formats on TryHackMe.
* Execute basic alert triage scripts using simulated host and network metrics.

---

## 🚨 SOC L1 Exam Notes

### Must Remember
* The SIEM tool provides **Detection** capabilities by correlating multi-source logs; it does not replace the host-level protection of an EDR.
* A complete triage report is incomplete without mapping out all **5 Ws** and capturing screenshot evidence inside the ticket platform.
* CIRT/CSIRT teams are external or advanced teams that act as emergency cyber-firefighters when a breach goes completely out of control.

### Common Interview Topics
* Explaining the explicit operational differences between L1, L2, and Security Engineering roles.
* Walking through a triage framework (such as the 5 Ws) using a simulated security incident.
* Explaining why threat actors target the human vector over traditional technical firewall perimeters.

### Common Alert Types Related To This Topic
* `Malware detected on Host` (Data stealers or unverified execution patterns).
* `Phishing Campaign Detection` (External emails spoofing corporate entities).
* `Firewall Rule Drop / Block Event` (Unauthorized attempts to cross internal networks).

---

## 🌐 Real-World Application & Observations
* **Analyzing a Live Event Signature:** When an alert like `Malware detected on Host: GEORGE PC` fires, the underlying log tracks technical indicators including a timestamp (e.g., `13:20`), an absolute file path directory, a localized user descriptor, and a clear root-cause trace pointing to an unauthorized browser download originating from a pirated software domain.

---

## 🧠 My Takeaways
1. A solid triage process relies heavily on structured thinking; using the 5 Ws ensures no critical information is dropped when handing a ticket off to tier-2 or tier-3 analysts.
2. Technology alone cannot defend an enterprise environment. Because attackers routinely target human psychology to bypass perimeter filters, an analyst must maintain a persistent "Trust but Verify" mindset for incoming user telemetry.

---

## 💪 Skills Acquired
* Mapped the precise escalation paths and separation of duties inside an enterprise SOC tier structure.
* Analyzed log fields to extract the 5 Ws required for formal ticketing documentation.
* Evaluated active phishing profiles using the Fish Tank community intelligence repository.

---

## 💬 Potential Interview Questions & Answers
**Q: If an endpoint tool generates an alert for malicious activity, why should an L1 analyst investigate the context rather than letting the tool handle it automatically?**
**A:** Automated tools are excellent for basic containment, but they cannot evaluate human context or identify underlying root causes. If an employee falls for a phishing email or a social engineering trick, the tool might block the final file execution, but it will miss the broader compromise of user credentials or the initial delivery method. An L1 analyst applies the 5 Ws framework to identify exactly how the asset was compromised, ensuring that we patch the process flaw or notify the IR team if a broader campaign is active.

**Q: A critical ransomware alert indicates a host is rapidly encrypting files, and it appears to be spreading faster than your standard triage playbook allows. What do you do?**
**A:** My priority is to prevent lateral movement across the network. I would instantly capture the core 5 Ws data fields from the log, document the indicators, and escalate the ticket directly to Tier-2/3 or the Incident Response team according to our high-severity protocols. If the infection bypasses corporate tool controls and threatens total environment availability, our playbooks dictate engaging the CIRT/CSIRT firefighters to execute deep, tool-independent crisis containment.

---

## 📖 References
* Defronix Class 02: Human Attack Vectors, Process & Technology
* TryHackMe SOC Level 1 Pathway Architecture Resources
