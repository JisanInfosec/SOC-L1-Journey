# 📝 Class 04: Alert Triage, TTPs & Threat IOCs

## 🎯 Objective

Understand the core operational frameworks—TTPs, IOCs, and Alert Triage—required to analyze security events, prioritize incoming alerts, and accurately differentiate true threats from system noise on the SOC floor.

---

## 🧭 Key Concepts

* **Tactics, Techniques, and Procedures (TTPs):** A behavioral framework detailing adversary goals (Tactics), execution methodologies (Techniques), and specific tool commands or scripts (Procedures).


* **Indicators of Compromise (IOCs):** Observable digital evidence left behind during or after an attack, such as IP addresses, domains, file hashes, or email headers.


* **Alert Triage:** The structured workflow of reviewing incoming SIEM/EDR alerts, validating context, assigning priority, and determining a True Positive or False Positive verdict.



---

## 🔤 Key Terms Learned

* **Tactics:** The immediate operational goal of an attacker during a specific phase of the attack lifecycle (e.g., Initial Access, Credential Access).


* **Techniques:** The technical method used by an adversary to achieve a tactic (e.g., LSASS Memory Dumping).


* **Procedures:** The exact commands, tools, scripts, or syntax executed by an attacker (e.g., `procdump.exe -accepteula -ma lsass.exe lsass.dmp`).


* **True Positive (TP):** An alert that correctly identifies actual malicious activity or policy violations.


* **False Positive (FP):** An alert triggered by benign activity that mimics malicious behavior.



---

## 👨💻 SOC Analyst Perspective

As a future L1 analyst, my responsibility during this operational phase is to:

* Validate incoming security alerts using contextual telemetry before taking containment actions or escalating.


* Map observed adversary behaviors to established frameworks like MITRE ATT&CK to understand attack progression.


* Document triage steps clearly in the ticketing platform to maintain high investigation quality and smooth shift handoffs.



---

## 📋 Detailed Notes

### 1. Tactics, Techniques, and Procedures (TTPs)

* TTPs describe adversary behavior, explaining how and why an attack occurred rather than just listing static artifacts.


* Tracking TTPs allows SOC teams to detect attacks even when adversaries change their IP addresses or file hashes to evade static signatures.


* TTPs are mapped using frameworks like MITRE ATT&CK (14 tactics) and the Cyber Kill Chain (7 stages).



### 2. Indicators of Compromise (IOCs) & Data Sources

* IOCs represent the digital footprints left behind during an incident, providing immediate targets for perimeter blocking or host isolation.


* **Network IOCs:** Malicious IPs, domains, C2 communication protocols. Found in Firewall, Proxy, DNS, and IDS/IPS logs.


* **Host/Endpoint IOCs:** Suspicious processes, unauthorized services, registry modifications, scheduled tasks. Found in EDR/XDR, Windows Event Logs, Sysmon, and Linux Audit logs.


* **File IOCs:** Cryptographic hashes (MD5, SHA256), anomalous file sizes, file patterns. Found in Antivirus, EDR, Email Security Gateways, and Sandboxes.


* **Email IOCs:** Malicious sender addresses, phishing subject lines, malicious URLs, header anomalies. Found in Secure Email Gateways (SEGs) and M365 Defender.



### 3. Alert Triage & Prioritization

* SIEM platforms aggregate millions of raw events into alerts; triage separates real security incidents from operational noise.


* Prioritization rules require analysts to:
1. **Filter:** Ignore alerts currently handled by other analysts.


2. **Sort by Severity:** Process Critical alerts first, followed by High, Medium, and Low.


3. **Sort by Time:** Address older unhandled alerts before newer ones to limit active adversary dwell time.




* Investigation steps require verifying the affected user/host, checking surrounding temporal events, querying threat intelligence, and documenting the final verdict.



---

## 🛠️ Tools Mentioned

* **SIEM / XDR / EDR:** Wazuh, Microsoft Defender for Endpoint, CrowdStrike Falcon


* **Log Sources:** Windows Event Logs, Sysmon, Linux Auditd, Firewall/Proxy Logs


* **Email Security Gateways:** Proofpoint, Mimecast, M365 Defender


* **Frameworks & Intel:** MITRE ATT&CK, Cyber Kill Chain, Threat Intelligence Platforms



---

## 🔬 Lab Evidence

Practical demonstration conducted using a SIEM alert triage interface (TryHackMe SOC L1 environment).

**Practical validation completed:**

* Investigated a high-volume data transfer alert ($5\text{ GB}+$) to `zoom.us` and classified it as a **False Positive** due to legitimate video conferencing activity.


* Validated a double-extension file creation alert (`cats2024_25.4.exe`) executed under `chrome.exe` as a **True Positive** malicious download.


* Verified a GitHub repository download alert accessed by an internal developer (`gchandler`) as a **False Positive** benign action.



---

## 🚨 SOC L1 Exam Notes

### Must Remember

* IOCs tell you *what* happened (short-lived, easy to change); TTPs tell you *how and why* it happened (long-lived, hard to change).


* Triage prioritization order: Filter unassigned $\rightarrow$ Sort Critical to Low $\rightarrow$ Sort Oldest to Newest.


* Analyst performance and promotion in a SOC depend on triage quality and investigation accuracy, not ticket closure volume.



### Common Interview Topics

* The operational differences between IOCs and TTPs.


* Explaining the 7 stages of the Cyber Kill Chain vs. the 14 tactics of MITRE ATT&CK.


* The step-by-step methodology for triaging a high-severity alert in a SIEM.



### Common Alert Types Related To This Topic

* Potential Data Exfiltration Over Non-Standard Ports


* Executable File Creation with Double Extension


* LSASS Process Memory Dump Attempt


* Suspicious PowerShell Command Line Execution



---

## 🌐 Real-World Application & Observations

* **Windows Event ID 4688 / Sysmon Event ID 1:** Triggers when a new process is created; essential for catching command-line execution procedures (e.g., `procdump.exe` targeting `lsass.exe`).


* **Windows Event ID 13 / Sysmon Event ID 13:** Registry modification events used to detect persistence mechanisms added to Run/RunOnce keys.



---

## 🧠 My Takeaways

1. Alert severity does not automatically equal a true positive; high-volume network alerts often turn out to be legitimate business applications like Zoom or administrative tools when contextualized properly.


2. Focus on process relationships and command-line arguments rather than tool names alone, as legitimate binaries are regularly abused by adversaries.



---

## 💪 Skills Acquired

* Differentiated between static IOC signatures and behavioral TTP indicators.


* Executed systematic alert prioritization based on analyst assignment, severity scoring, and chronological order.


* Investigated process trees and network context within a SIEM to issue True Positive and False Positive verdicts.



---

## 💬 Potential Interview Questions & Answers

**Q: How do you differentiate between an IOC and a TTP, and why are TTPs more valuable for long-term detection?**
**A:** An IOC is an observable artifact like an IP address or file hash that answers *what* happened. IOCs are short-lived because attackers can easily change their infrastructure. A TTP describes the underlying behavior and methodology—*how* the attacker operates. TTPs are harder for attackers to change, making them far more effective for building durable detection rules in a SIEM or EDR.

**Q: Walk me through your triage process when a High-severity alert lands in your queue.**
**A:** First, I assign the alert to myself and update its status to *In Progress* to prevent duplicate work. Next, I review the alert metadata—affected host, user account, timestamp, and process command lines. I then analyze surrounding logs to check for suspicious activity immediately before or after the alert. I cross-reference destination IPs/hashes against threat intel and verify business context with internal playbooks. Finally, I classify the alert as a True or False Positive, document my findings in the ticketing platform, and either close it or escalate to L2 if containment is required.

---

## 📖 References

* Defronix SOC Analyst L1 Course - Class 04


* TryHackMe Room: SOC L1 Alert Triage


* MITRE ATT&CK Framework Matrix
