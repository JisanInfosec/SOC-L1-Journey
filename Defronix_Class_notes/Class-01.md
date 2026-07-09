# 📝 Class 01: Introduction to the SOC & The Defensive Mindset

## 🎯 Objective
To establish a foundational understanding of enterprise defensive architecture, map the operational differences between infrastructure teams, define the primary tools of a security analyst, and master the core alert triage workflow.

---

## 🧭 Key Concepts
* **The Digital Watchtower:** Shifting the perspective from offensive exploitation to continuous, proactive corporate monitoring and visibility.
* **The Human Element:** Automated alerts are useless noise without an analytical human brain to investigate the contextual timeline surrounding an event.
* **The First Line of Defense:** The Level 1 SOC Analyst acts as the organization's essential filter, preventing critical breaches from slipping through while systematically managing alert fatigue.

---

## 🔤 Key Terms Learned
* **SOC:** Security Operations Center
* **NOC:** Network Operations Center
* **IR:** Incident Response
* **SIEM:** Security Information and Event Management
* **EDR:** Endpoint Detection and Response
* **IOC:** Indicator of Compromise
* **True Positive:** A legitimate, verified security threat or malicious event.
* **False Positive:** A benign system action or false alarm that mistakenly triggers a security rule.

---

## 👨‍💻 SOC Analyst Perspective
As a future L1 analyst, my primary responsibility is not to definitively prove a massive attack occurred or conduct deep forensic investigations. My core responsibility is to:
* Gather initial telemetry and evidence efficiently.
* Validate the surrounding context of an alert.
* Accurately document every step of my analytical thought process.
* Escalate verified threats quickly while closing down false alerts safely.

---

## 📋 Detailed Notes

### 1. Enterprise Team Boundaries (The Castle Metaphor)
* **Network Operations Center (NOC):** Responsible for network performance, speed, routing optimization, and maximizing uptime. This team ensures that data flows smoothly and maintains the structural health of the "castle walls."
* **Security Operations Center (SOC):** Responsible explicitly for security events, hunting for malicious patterns, and identifying unauthorized access attempts. This team mans the "watchtower."
* **Incident Response (IR):** The rapid-deployment tactical team that enters to isolate assets, eradicate threats, and execute deep digital forensics once a breach is actively confirmed. They act as the "special forces."

### 2. The 3-Step L1 Analyst Shift Workflow
1. **Monitor & Triage:** Actively watch central dashboards, pull surrounding log context, and quickly separate real dangers from background network noise.
2. **Analyze & Document:** Inspect system logs (firewall, proxy, host events) to extract active Indicators of Compromise (IOCs) and log every analytical step clearly.
3. **Escalate or Close:** Package valid threats cleanly for escalation to L2/L3 or IR, or close out false alarms with clear written technical justification.

### 3. Core Enterprise Defensive Software
* **SIEM Platforms (Splunk / Microsoft Sentinel):** Centrally ingests, aggregates, and correlates logs from every machine, firewall, and database across the corporate network into one searchable pane.
* **EDR Solutions (CrowdStrike / Microsoft Defender):** Acts as a host-level security camera monitoring process trees, registry modifications, and local execution behavior directly on user endpoints.
* **Threat Intelligence (VirusTotal / MITRE ATT&CK):** Global databases used to enrich local investigations by checking the reputation of unknown file hashes, bad IPs, or tracking documented adversary tactics.
* **Ticketing Platforms (Jira / ServiceNow):** The standard tracking platform used to build an audit trail of an investigation, manage cross-team handoffs, and track organizational compliance metrics.

---

## 🛠️ Tools Mentioned
* **SIEM:** Splunk, Microsoft Sentinel, IBM QRadar
* **EDR/XDR:** CrowdStrike Falcon, Microsoft Defender for Endpoint, Carbon Black
* **Threat Intel:** VirusTotal, AbuseIPDB, MITRE ATT&CK Navigator
* **Ticketing:** Jira, ServiceNow

---

## 🛡️ Threat Vector Reference Matrix

| Attack Type | Log Evidence / Telemetry Observed | Immediate L1 Containment Step |
| :--- | :--- | :--- |
| **Phishing** | Inbound email gateway anomalies, suspicious headers, outbound traffic to brand-new external domains. | Parse mail headers, run domain reputation lookups, and isolate the endpoint if a link/attachment was executed. |
| **Brute Force** | High-frequency failed authentication events (Windows Event ID 4625) or sudden multi-account lockouts. | Identify source IP (internal vs. external), check for post-login success, block the IP, and trigger a password reset. |
| **Malware** | EDR alerts flagging unsigned executables launching suspicious child processes or beaconing to external C2 servers. | Quarantine the file instantly via the EDR console, verify the hash on VirusTotal, and map the process tree. |
| **Ransomware** | Rapid, automated mass file encryption, Volume Shadow Copy/backup deletion commands, and ransom text files dropping. | **Critical Emergency Protocol:** Instantly disconnect the system from the local network to halt lateral spread and notify the IR team. |

---

## 🔬 Lab Evidence
No hands-on lab was associated with this introductory theoretical session.

**Future practical validation areas planned:**
* Splunk log ingestion and query writing.
* Windows Event Viewer analysis (Parsing explicit Event IDs).
* Live EDR alert simulations and endpoint isolation drills.

---

## 🌐 Real-World Application & Observations
* **Windows Event ID 4625** is the fundamental event log generated on a system when an authentication attempt fails.
* Observing a high volume of failed 4625 events over a short period of time indicates automated scripting (brute-forcing) rather than a user forgetting their password.
* The most dangerous signature to catch during a triage shift is a long string of failed authentication attempts followed immediately by a single success log, pointing to a potentially compromised credential.

---

## 🧠 My Takeaways
1. Security and networking are completely different jobs. A NOC wants a port wide open if it makes a business app run faster; a SOC wants that port analyzed, restricted, and continuously monitored.
2. As an L1 analyst, I am the gateway. If I rush or get lazy with log context, a critical alert gets buried, and a major breach occurs.
3. SIEM provides the macro-view (everything talking across the network), while EDR provides the micro-view (what is happening inside the memory and processes of a single laptop).
4. Guessing has no place on the SOC floor. If an alert is ambiguous or weird, documenting the facts and escalating is the correct operational decision.

---

## 💪 Skills Acquired
* Differentiated between NOC, SOC, and Incident Response team parameters.
* Mapped out the lifecycle of a Level 1 analyst triage and escalation pipeline.
* Identified the primary data collection roles of SIEM, EDR, Threat Intel, and Ticketing platforms.
* Recognized the specific logging fingerprints left behind by Phishing, Brute Force, Malware, and Ransomware.

---

## 💬 Potential Interview Questions & Answers

**Q: What is a SOC and why does an enterprise need one?**
**A:** A Security Operations Center is a centralized team responsible for monitoring, detecting, analyzing, and responding to cyber threats 24/7/365. Organizations need a SOC because automated security tools generate millions of logs daily; human analysts are required to evaluate context, catch complex attack patterns, and mitigate breaches before they cause catastrophic financial or reputational damage.

**Q: How do you differentiate between the goals of a SOC and a NOC?**
**A:** A NOC focuses entirely on network performance, availability, bandwidth, and maximizing uptime—ensuring the business keeps moving smoothly. A SOC focuses exclusively on the security posture—monitoring events to identify malicious activity, unauthorized access, or policy violations. The NOC ensures the data flows; the SOC ensures the data is secure.

**Q: If you see an alert for an unknown file executing on a workstation, what is your approach?**
**A:** I would pull the host's EDR telemetry to check the process tree and look for indicators like unsigned binaries or unexpected outbound connections. I would extract the file hash and query enrichment tools like VirusTotal to see if it matches known global malware signatures. If suspicious, I would use the EDR to isolate the workstation from the network, document my findings, and escalate the issue.

---

## 📖 References
* Defronix Academy SOC Analyst L1 Live Programme - Class 01 Lecture & Slides
* TryHackMe SOC Level 1 Learning Path - Section 1 Introduction Modules
