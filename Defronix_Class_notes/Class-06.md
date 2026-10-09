# Class 06: Malware Fundamentals and SOC L1 Detection

## Objective

Understand the main types of malware, how they behave, and what indicators a SOC L1 analyst may see in endpoint and network alerts. Learn how to check suspicious activity, document the evidence, and escalate when further investigation is required.

---

## Key Concepts

* **Malware:** Software designed to steal information, disrupt systems, gain unauthorized access, or perform other harmful actions.
* **Malware Classification:** Identifying malware by its main behavior, such as infecting files, spreading across networks, encrypting data, stealing information, or hiding malicious activity.
* **Malware Detection:** Looking for suspicious process activity, file changes, network connections, DNS queries, and system configuration changes.
* **Alert Validation:** Checking the available evidence before deciding whether an alert represents suspicious or malicious activity.
* **L1 Workflow:** Detect, validate, and escalate when required.

---

## Key Terms Learned

* **Virus:** Malware that attaches to another file and spreads when the infected file runs.
* **Worm:** Malware that can spread automatically across networks without requiring user interaction.
* **Trojan:** Malware disguised as legitimate software.
* **Ransomware:** Malware that encrypts files and demands payment.
* **Spyware:** Malware that secretly collects user or system information.
* **Rootkit:** Malware designed to hide other malicious activity.
* **Payload:** The code or action delivered to perform the attacker's intended task.
* **Persistence:** A way for malicious software or an attacker to maintain access to a system.
* **Lateral Movement:** Activity involving movement from one system to another within a network.
* **Data Exfiltration:** Unauthorized transfer of data out of a system or network.
* **SMB:** Server Message Block, a protocol used for sharing files and other resources across a network.
* **DNS:** Domain Name System, which translates domain names into IP addresses.
* **RaaS:** Ransomware-as-a-Service, a model in which ransomware tools or services are provided to other criminals.
* **RAT:** Remote Access Trojan, malware designed to provide remote access to a victim's system.
* **Botnet:** A group of compromised devices controlled by an attacker.
* **Zero-Day Vulnerability:** A vulnerability that is unknown to the responsible vendor or for which an effective fix is not yet available.

---

## SOC Analyst Perspective

As a future L1 analyst, my responsibility during this operational phase is to:

* Identify suspicious behavior from endpoint, antivirus, and network alerts.
* Check the affected host, user, process, files, and network activity.
* Correlate related alerts instead of judging an event using only one indicator.
* Document the evidence and findings in the ticketing platform.
* Notify the L2 analyst or IR team when the activity meets escalation criteria.
* Avoid declaring a system compromised or data stolen unless the available evidence supports that conclusion.

---

## Detailed Notes

### 1. Malware Fundamentals

* Malware is a general term for software designed to perform harmful or unauthorized activities.
* Common objectives include stealing data, disrupting systems, spying on users, maintaining unauthorized access, and encrypting files.
* A malware alert is a starting point for investigation. The analyst must validate the activity using available evidence.

### 2. Virus

* A virus attaches itself to a legitimate file and spreads when the infected file executes.
* The class describes the basic flow as downloading an infected file, running it, executing the virus, and spreading to other files.
* Common indicators include unknown process creation, suspicious file modification, and antivirus alerts.
* An `.exe` file is not automatically malicious. Its behavior and reputation must be assessed in context.

### 3. Worm

* A worm can spread automatically between systems over a network.
* Unlike a typical file-infecting virus, a worm does not need the user to run an infected file on every affected computer.
* Common indicators include sudden network traffic increases, similar payloads on multiple hosts, and lateral scanning.
* The class uses WannaCry and an SMB vulnerability as an example.
* When similar alerts appear across multiple computers within a short period of time, check whether they may be related.

### 4. Trojan

* A Trojan disguises itself as legitimate software.
* Examples include fake cracked software or applications that appear useful but perform malicious actions.
* Trojans may open a backdoor, steal information, or install additional malware.
* Relevant indicators include unusual outbound connections, unknown services, and suspicious startup entries.
* Check what the program executed, what it changed, and where it communicated.

### 5. Ransomware

* Ransomware encrypts files and demands payment.
* The class presents an attack flow involving a phishing email, a user opening an attachment, malware installation, file encryption, and a ransom note.
* Common indicators include mass file renaming, unusual encryption activity, high CPU usage, and shadow copy deletion.
* Rapid file changes affecting many files can indicate an active ransomware incident.
* Validate the endpoint evidence and escalate promptly according to the SOC's incident response procedure.

**Additional topics reported in the class video:**

* Encryption ransomware, scareware, and screen-locking ransomware.
* Ransomware-as-a-Service (RaaS).
* RansomHub, LockBit, and Akira as examples of ransomware families.
* A demonstration of checking whether Windows System Restore is enabled.

These additional topics are based on the supplied video analysis summary, not the uploaded slide deck.

### 6. Spyware

* Spyware secretly collects user or system information.
* Information targeted may include passwords, banking details, screenshots, and keystrokes.
* Relevant indicators include unusual outbound connections, DNS queries to unknown domains, and data exfiltration alerts.
* An unknown domain alone does not confirm spyware. Check the destination, endpoint activity, and supporting evidence.

### 7. Rootkit

* A rootkit is designed to hide malicious activity from normal detection.
* It may hide processes, files, or registry entries.
* Indicators mentioned in the class include mismatches between user-mode and kernel-mode results, integrity check failures, and unusual driver loads.
* Rootkit investigations may require specialist endpoint analysis. An L1 analyst should document the evidence and escalate when appropriate.

**Additional rootkit topics reported in the class video:**

* Bootkits.
* Memory rootkits.
* Application rootkits.
* Kernel-mode rootkits.

Detailed rootkit analysis is beyond the main L1 scope of this lesson.

### 8. Virus Lifecycle

The supplied video analysis reports four virus lifecycle phases:

* **Dormant:** The virus is inactive.
* **Propagation:** The virus spreads.
* **Triggering:** A condition causes the virus to become active.
* **Execution:** The virus performs its intended action.

These phases were reported in the video analysis and are not separately explained in the uploaded slide deck.

### 9. Additional Trojan and Malware Concepts

The supplied video analysis also reports the following topics:

* **Backdoor Trojan:** Provides a way to access a compromised system.
* **Banker Trojan:** Targets financial information or banking activity.
* **Remote Access Trojan (RAT):** Can allow an attacker to access or control a system remotely.
* **Botnets and zombie computers:** Compromised devices may be controlled as part of a larger group.
* **Zero-day vulnerabilities:** The video discusses the threat posed by vulnerabilities without an available effective fix.

These are supplementary topics for recognition. Detailed malware analysis and exploitation are not the primary objectives of the L1 lesson.

### 10. Comparing Malware Types

The main distinctions are:

| Malware Type | Main Behavior | What an L1 Analyst Looks For |
|---|---|---|
| Virus | Infects files | Suspicious execution and file changes |
| Worm | Spreads automatically | Similar activity across several hosts |
| Trojan | Disguises itself as legitimate software | Unusual services, startup entries, and network activity |
| Ransomware | Encrypts files | Rapid file changes and encryption activity |
| Spyware | Collects information secretly | Suspicious outbound connections and potential data theft |
| Rootkit | Hides malicious activity | Integrity problems and unusual driver activity |

A single malware incident may involve several behaviors. Use the observed evidence rather than relying only on a malware label.

### 11. SOC L1 Malware Triage

When a malware alert appears in the ticketing platform, the analyst should ask:

* Is a suspicious file being delivered or executed?
* Are files being modified or encrypted?
* Are multiple hosts affected?
* Are there unusual outbound connections or DNS queries?
* Is there evidence of persistence or lateral movement?
* Could information be leaving the network?
* Does the alert require escalation?

The basic process is:

**Detect → Validate → Escalate**

The L1 analyst must document the relevant evidence and follow the organisation's escalation and incident response procedures.

---

## Tools Mentioned

* **Antivirus:** Mentioned as a source of malware detection alerts. No specific antivirus product is named.
* **Endpoint Monitoring:** Relevant evidence includes process creation, file modification, services, startup entries, and driver activity.
* **Network Monitoring:** Relevant evidence includes traffic spikes, outbound connections, lateral scanning, and DNS queries.
* **Windows System Restore:** The supplied video analysis reports a demonstration of checking whether the feature is enabled.
* **Named SIEM/EDR Products:** None are explicitly identified in the uploaded slide deck.

---

## Lab Evidence

The uploaded slide deck is primarily theoretical and does not document a completed hands-on malware analysis lab.

The supplied video analysis reports a live Windows System Restore configuration check. No lab results or independent verification are recorded in these notes.

**Practical validation areas for future study:**

* Review antivirus alerts and identify the affected file or process.
* Examine suspicious process and file activity in a safe training environment.
* Identify indicators of worm-like propagation across multiple hosts.
* Review ransomware-related file changes and determine when escalation is necessary.
* Examine suspicious DNS requests and outbound connections.
* Practise writing a concise malware alert report.

These are proposed study activities, not completed lab exercises.

---

## SOC L1 Exam Notes

### Must Remember

* Malware is a general term for harmful software.
* A virus infects files; a worm spreads automatically.
* A Trojan disguises itself as legitimate software.
* Ransomware encrypts files and demands payment.
* Spyware collects information secretly.
* A rootkit attempts to hide malicious activity.
* Multiple related indicators provide more useful evidence than one isolated event.
* Rapid file encryption or similar suspicious activity across many hosts may require prompt escalation.
* An alert does not automatically prove a system is compromised.
* The L1 workflow is **Detect, Validate, Escalate**.

### Common Interview Topics

* Difference between a virus and a worm.
* What a Trojan does and how it may be detected.
* Common indicators of ransomware.
* How spyware may be identified through endpoint and network activity.
* Why rootkits are difficult to detect.
* How to investigate similar suspicious activity across multiple hosts.
* What evidence to document before escalating a malware alert.

### Common Alert Types Related To This Topic

* Antivirus malware detection.
* Suspicious process creation.
* Unusual file modification.
* Multiple hosts showing similar suspicious activity.
* Sudden increase in network traffic.
* Suspicious outbound connections.
* DNS queries to unknown domains.
* Suspicious service or startup entry creation.
* Mass file renaming or unusual encryption activity.
* Shadow copy deletion.
* Possible data exfiltration.
* Integrity check failures or unusual driver loads.

---

## Real-World Application and Observations

* If similar suspicious activity appears across several hosts, check whether the events are related and whether the activity may be spreading.
* If an endpoint modifies or renames thousands of files in a short period of time, investigate possible ransomware behavior and promptly notify the appropriate response team when escalation criteria are met.
* If a suspicious application creates a new service and communicates with an unfamiliar external destination, correlate the process, system changes, and network activity before making a determination.
* If an alert suggests spyware, investigate unusual connections and potential data exfiltration without claiming that data was stolen unless evidence supports that conclusion.
* Rootkit-related integrity problems and unusual driver activity may require specialist investigation.
* The uploaded slide deck does not specify Windows Event IDs or individual network port numbers. Those details should be added only after they are verified through separate practical study.

---

## Skills Acquired

* Distinguished the six main malware categories by their primary behavior.
* Identified common file, process, network, and system indicators associated with malware.
* Recognized the importance of checking multiple hosts when investigating possible worm activity.
* Identified common ransomware warning signs.
* Connected suspicious outbound traffic with possible spyware-related activity.
* Recognized why rootkit detection can be difficult.
* Applied the Detect, Validate, Escalate workflow to malware alert scenarios.
* Practised explaining investigation findings using clear, evidence-based language.

---

## Potential Interview Questions and Answers

**Q: What is the difference between a virus and a worm?**

**A:** A virus attaches to a file and spreads when that file runs. A worm can spread automatically over a network without requiring the user to run an infected file on each affected computer.

**Q: What is a Trojan, and how might a SOC analyst identify one?**

**A:** A Trojan disguises itself as legitimate software. I would investigate the program's behavior, including unusual outbound connections, unknown services, and suspicious startup entries.

**Q: Name three indicators of possible ransomware activity.**

**A:** Mass file renaming, unusual encryption activity, and shadow copy deletion. I would validate the related endpoint evidence and escalate promptly when the activity meets the SOC's escalation criteria.

**Q: What would you investigate if several computers showed similar suspicious activity and network traffic increased suddenly?**

**A:** I would identify the affected hosts, compare the alerts and timestamps, and check for similar payloads or lateral scanning. The pattern could indicate a worm or another spreading threat. I would document the evidence and escalate according to the SOC procedure.

**Q: What would you do if an endpoint showed thousands of renamed files and shadow copy deletion?**

**A:** I would treat the activity as a possible ransomware incident. I would review the affected host, suspicious process, file activity, and related alerts, then record the evidence in the ticketing platform. Because the activity suggests potential data disruption, I would promptly escalate according to the incident response procedure and notify the appropriate team.

**Q: What is a rootkit, and why is it difficult to detect?**

**A:** A rootkit is designed to hide malicious activity, such as processes, files, or registry entries. Possible indicators include integrity check failures, unusual driver loads, and inconsistencies between user-mode and kernel-mode results.

**Q: What is the responsibility of a SOC L1 analyst when a malware alert appears?**

**A:** My responsibility is to detect the alert, validate the available evidence, document the findings, and escalate when further investigation or response is required. I should not declare a confirmed compromise based on a single indicator.

---

## References

* Defronix Cyber Security, *SOC Analyst L1 Day 6 — Malware* (uploaded slide deck, pages 2–11).
* Defronix Class 06 video analysis summary supplied during the lesson review.
