#  Class 05: Cyber Kill Chain, Alert Reporting, Escalation & SOC Communication

##  Objective
Understand the attacker lifecycle from reconnaissance through actions on objectives, and apply that knowledge to L1 alert triage, documentation, escalation, and SOC communication. The class also establishes how an alert moves from detection to action. SOC Analyst L1 Day 5 SOC Analyst L1 Day 5

---

##  Key Concepts
* **Cyber Kill Chain:** A seven-stage model used to describe the progression of a cyber attack.
* **Alert Reporting:** Documenting what was detected, analyzed, concluded, and acted upon. SOC Analyst L1 Day 5
* **Alert Escalation:** Handing an alert to L2 or another responsible team when it exceeds L1 authority or meets escalation criteria. SOC Analyst L1 Day 5
* **SOC Communication:** Converting technical findings into clear, factual information for analysts, IR, IT/Admin, or management. SOC Analyst L1 Day 5

---

##  Key Terms Learned
* **Reconnaissance:** Gathering information about a target before an attack.
* **Weaponization:** Preparing an exploit and payload for delivery.
* **Delivery:** Sending the malicious weapon to the target.
* **Exploitation:** Triggering or abusing a vulnerability or malicious functionality.
* **Persistence:** Maintaining access so the attacker remains after events such as a system restart.
* **Command & Control (C2):** Communication between a compromised system and attacker infrastructure.
* **Beaconing:** Repeated communication from a compromised host at regular intervals.
* **Payload:** What executes after an exploit succeeds.
* **Exploit:** The method used to abuse a vulnerability. SOC Analyst L1 Day 5
* **Escalation:** Passing an alert to a higher tier or responsible team for further action.
* **True Positive:** An alert that represents genuine suspicious or malicious activity.

---

##  SOC Analyst Perspective
As a future L1 analyst, my responsibility during this operational phase is to:
* Identify where observed activity fits within the attack lifecycle and validate it using available evidence.
* Document the alert clearly, determine whether escalation is required, and provide the next analyst with enough context to continue the investigation.

---

##  Detailed Notes

### 1. Cyber Kill Chain
* The Cyber Kill Chain consists of **Reconnaissance, Weaponization, Delivery, Exploitation, Installation, C2, and Actions on Objectives**. SOC Analyst L1 Day 5
* The model helps an analyst understand where an observed event may fit within an attack.
* An L1 analyst should focus on recognizing the stage represented by an alert and identifying available defensive opportunities.

### 2. Reconnaissance
* Attackers collect information such as employee emails, domains, subdomains, technologies, public profiles, and leaked credentials. SOC Analyst L1 Day 5
* Defensive monitoring can include excessive DNS queries, scanning behavior, and suspicious phishing patterns. SOC Analyst L1 Day 5
* Reconnaissance activity is not automatically malicious; the analyst must consider source, target, timing, and context.

### 3. Weaponization
* Weaponization combines an exploit with a malicious payload.
* Examples include malware embedded in a PDF, a reverse shell hidden in a Word macro, or an exploit targeting an unpatched service. SOC Analyst L1 Day 5
* SOC activities mentioned include file hash analysis, malware sandboxing, and detecting known exploit signatures. SOC Analyst L1 Day 5

### 4. Delivery
* The malicious content may be delivered through phishing email, attachments, drive-by websites, USB devices, or compromised advertisements. SOC Analyst L1 Day 5
* Relevant defensive controls include email gateway filtering, URL reputation, and attachment sandboxing.
* Receiving a suspicious file is not the same as confirmed compromise; investigate what happened after delivery.

### 5. Exploitation
* Exploitation occurs when the vulnerability or malicious functionality is actually triggered.
* Examples include malicious-link execution, buffer overflow, outdated-plugin exploitation, and macro execution. SOC Analyst L1 Day 5
* An L1 analyst should distinguish an attempted attack from evidence of successful execution.

### 6. Installation & Persistence
* Installation may establish persistence so the attacker can remain on the system after a restart.
* The class identifies Registry Run Keys, Scheduled Tasks, Startup Folders, and backdoor users as examples. SOC Analyst L1 Day 5
* EDR, registry monitoring, and autorun analysis are relevant defensive areas.

### 7. Command & Control
* A compromised host communicates with attacker infrastructure through a C2 channel.
* The class highlights beaconing, DNS tunneling, HTTPS-based C2, abnormal outbound traffic, and known bad IPs/domains. SOC Analyst L1 Day 5
* L1 should avoid treating HTTPS or repeated communication as malicious by itself; context and correlation are required.

### 8. Actions on Objectives
* This stage covers the attacker's intended outcome, including data theft, ransomware deployment, credential dumping, lateral movement, and financial fraud. SOC Analyst L1 Day 5
* Potential data risk is specifically identified as an escalation factor. SOC Analyst L1 Day 5
* The analyst should document observed evidence without claiming impact that has not been established.

### 9. Standard Hacking Methodology
* The lesson also presents Reconnaissance, Scanning, Gaining Access, Maintaining Access, and Clearing Tracks. SOC Analyst L1 Day 5
* These stages broadly map to the Cyber Kill Chain terminology used in the same lesson. SOC Analyst L1 Day 5
* The purpose for L1 is terminology recognition and attack-flow understanding rather than offensive execution.

### 10. Alert Reporting
* An alert report should state what was detected, what was analyzed, the conclusion, and the action taken or recommended. SOC Analyst L1 Day 5
* Reporting supports audit trails, shift handover, incident response, and detection improvement. SOC Analyst L1 Day 5
* A strong L1 report is short, factual, evidence-based, and useful to the next analyst.

### 11. Alert Escalation
* L1 identifies and hands over incidents that require higher-level expertise or action. SOC Analyst L1 Day 5
* Escalation conditions include confirmed malicious activity, privileged accounts, critical assets, correlated alerts, data risk, and High/Critical severity or policy requirements. SOC Analyst L1 Day 5
* An escalation note should include the reason, evidence, severity, and recommended action. SOC Analyst L1 Day 5

### 12. SOC Communication
* Communication may occur between analysts, the IR team, IT/Admin, and management. SOC Analyst L1 Day 5
* Communication should remain clear, factual, assumption-free, calm, and non-blaming. SOC Analyst L1 Day 5
* IT/Admin communication should state the required action, while management communication should focus on impact and status. SOC Analyst L1 Day 5
* The class workflow is: **Alert Detected → Alert Triage → Alert Report Written → Escalation (if needed) → Clear Communication → Action Taken.** SOC Analyst L1 Day 5

---

##  Tools Mentioned
* **Reconnaissance / OSINT:** Google Dorking, WHOIS, DNS enumeration, OSINT. SOC Analyst L1 Day 5
* **Offensive Security:** Metasploit, Cobalt Strike. SOC Analyst L1 Day 5
* **Scanning:** Nmap, Nessus. SOC Analyst L1 Day 5
* **C2 Frameworks:** Mythic, Havoc. SOC Analyst L1 Day 5
* **Endpoint / Defensive:** EDR, registry monitoring, autorun analysis. SOC Analyst L1 Day 5
* **Email / Web Controls:** Email gateway filtering, URL reputation, attachment sandboxing. SOC Analyst L1 Day 5

---


##  SOC L1 Exam Notes

### Must Remember
* The seven Cyber Kill Chain stages: **Reconnaissance → Weaponization → Delivery → Exploitation → Installation → C2 → Actions on Objectives**. SOC Analyst L1 Day 5
* **Exploit** is how a vulnerability is abused; **payload** is what executes after exploitation. SOC Analyst L1 Day 5
* Receiving a malicious file does not automatically mean compromise; investigate execution and follow-on activity.
* Beaconing, abnormal outbound traffic, DNS tunneling, and known malicious destinations can indicate C2 activity. SOC Analyst L1 Day 5
* L1 should escalate confirmed malicious activity, privileged-account involvement, critical assets, correlated alerts, data risk, or policy-driven High/Critical alerts. SOC Analyst L1 Day 5
* Good reporting is **clear, factual, evidence-based, and actionable**. SOC Analyst L1 Day 5
* Do not state assumptions as facts.

### Common Interview Topics
* Explain the seven stages of the Cyber Kill Chain.
* Difference between an exploit and a payload.
* How to recognize basic C2 activity.
* When an L1 analyst should escalate.
* What information belongs in an alert report.
* Difference between suspicious activity and confirmed compromise.

### Common Alert Types Related To This Topic
* Phishing email / malicious attachment
* Suspicious PowerShell execution
* Suspicious parent-child process relationship
* Persistence mechanism created
* Abnormal outbound communication
* Beaconing behavior
* DNS tunneling
* Known malicious IP/domain communication
* Credential dumping
* Lateral movement
* Potential data exfiltration

---

##  Real-World Application & Observations
* A suspicious PowerShell alert becomes more significant when correlated with the **parent process, encoded command, execution time, and external communication**, as demonstrated in the class example. SOC Analyst L1 Day 5
* For network activity, an L1 analyst may review DNS requests, outbound connections, destination domains/IPs, and connection patterns when assessing possible C2 activity. SOC Analyst L1 Day 5
* The lesson does **not** provide specific Windows Event IDs or individual port numbers, so none should be added to the Class 5 scope without separate verification.
* In a ticketing platform, the analyst should record the evidence, verdict, severity, and next action so another analyst can continue the investigation without repeating the initial triage.

---

##  Skills Acquired
* Mapped attacker activity to the seven stages of the Cyber Kill Chain.
* Distinguished between an exploit and a payload.
* Identified common reconnaissance, delivery, persistence, and C2 indicators.
* Assessed when an alert should be escalated from L1.
* Structured alert findings into a concise report.
* Communicated technical findings according to the intended audience.
* Correlated multiple indicators instead of judging a single event in isolation.

---

##  Potential Interview Questions & Answers

**Q: What are the seven stages of the Cyber Kill Chain?**  
**A:** Reconnaissance, Weaponization, Delivery, Exploitation, Installation, Command & Control, and Actions on Objectives. I use the stages to understand where observed activity fits in the attack lifecycle. SOC Analyst L1 Day 5

**Q: What is the difference between an exploit and a payload?**  
**A:** An exploit is how a vulnerability is abused, while the payload is what executes after the exploit succeeds. SOC Analyst L1 Day 5

**Q: You receive an alert showing Word launching PowerShell with an encoded command outside business hours, followed by an external connection. What would you do?**  
**A:** I would correlate the Word-to-PowerShell process chain, encoded command, execution time, and external communication. I would document the evidence and classify the activity based on what is observed. Because the indicators support suspicious execution and external communication, I would escalate to L2 with the process tree, command line, destination domain, severity, and recommended containment. SOC Analyst L1 Day 5

**Q: Two alerts show a phishing email followed by suspicious execution on a system associated with an administrator and a critical asset. What is your response?**  
**A:** I would correlate the alerts as potentially related activity rather than treating them independently. Because a privileged account, critical asset, and multiple correlated alerts are involved, I would escalate according to the SOC process and provide the supporting evidence and recommended action. SOC Analyst L1 Day 5

**Q: What should a Tier-1 analyst include in an escalation note?**  
**A:** I would include the escalation reason, supporting evidence such as the process tree, command line, and destination domain, the severity, and the recommended action. SOC Analyst L1 Day 5

**Q: What is the correct L1 mindset when handling an incident that is beyond your authority?**  
**A:** My responsibility is to validate the alert, document the evidence, and hand it over correctly to the appropriate team rather than attempting actions outside my authority. SOC Analyst L1 Day 5

---

##  References
* Defronix Class 05 — *SOC Analyst L1 Day 5*
