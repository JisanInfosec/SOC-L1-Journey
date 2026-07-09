# 📝 Class 03: System Attack Vectors and Vulnerability Lifecycles



## 🎯 Objective

To transition from a beginner mindset to an analytical approach by evaluating systems as attack vectors, understanding vulnerability mechanics, and recognizing how misconfigurations trigger active security incidents.

---

## 🧭 Key Concepts

* **Systems as Attack Vectors:** Treating every endpoint, server, and network device as a potential pathway an attacker can exploit to gain initial access, move laterally, maintain persistence, or exfiltrate corporate data.


* **SOC Operating Architecture:** The operational division of labor between internal, outsourced (MSSP), and hybrid security teams handling the log ingestion and alert pipeline.



---

## 🔤 Key Terms Learned

* **Vulnerability:** An existing flaw or weakness in software, hardware, or an operating system code that can be exploited to compromise security parameters.


* **Exploit:** The specific method, technique, or code utilized by a threat actor to actively abuse a system vulnerability.


* **Incident:** An active security event where damage, unauthorized access, or systemic compromise is currently occurring on the corporate network.
* **Zero-Day Vulnerability:** A security flaw that is actively exploited in the wild before the software vendor is aware of it or has released a functional patch.



---

## 👨‍💻 SOC Analyst Perspective

As a future L1 analyst, my responsibility during this operational phase is to:

* Monitor the SIEM ticketing platform to ingest, normalize, and correlate incoming raw log files into clear alert visualizations.


* Analyze process trees and network traffic to determine whether a vulnerability was actually exploited or if the event is a false positive.
* Triage alerts efficiently, decide on severity parameters, and execute proper escalation or mitigation steps within a short period of time to notify the IR team.



---

## 📋 Detailed Notes

### 1. The Vulnerability and Patching Lifecycle

* A newly discovered software bug undergoes tracking via the Common Vulnerabilities and Exposures (CVE) cataloging system.


* When an active zero-day is identified, defenders must apply temporary mitigations—such as restricting access to trusted IPs, blocking signatures on the WAF/IPS, or shutting down exposed services—while waiting for the vendor to release an official patch.


* Failing to understand this lifecycle leads beginners to panic over every scanning alert, rather than verifying if a system is unpatched and vulnerable to the specific exploit code being used.

### 2. Common Infrastructure Vulnerability Categories

* **Application Flaws:** Code-level security holes such as SQL Injection, Command Injection, Cross-Site Scripting (XSS), and Remote Code Execution (RCE) that show up in web server logs and WAF alerts.


* **System Misconfigurations:** Human errors made by IT administrators or developers—such as default credentials, open admin panels, disabled MFA, or missing network segmentation—that allow attackers easy access without needing advanced exploit code.


* **Network Weaknesses:** The exposure of legacy, unpatched appliances or unencrypted plaintext protocols that allow credential harvesting across the wire.



---

## 🛠️ Tools Mentioned

* **SIEM (Security Information and Event Management):** Splunk, Microsoft Sentinel


* **EDR (Endpoint Detection and Response):** CrowdStrike, Microsoft Defender for Endpoint


* **Network & App Defense:** Web Application Firewalls (WAF), Intrusion Detection/Prevention Systems (IDS/IPS)


* **Attacker Utilities & Vectors:** Rubber Ducky, Keyloggers, SECRETSDUMP, WMIExec, netscan (netapp.exe), AnyDesk



---

## 🔬 Lab Evidence

### Mock SOC Simulation Scenario
The practical portion of this session featured a guided walkthrough of a SOC incident response and remediation scenario modeled after TryHackMe environmental layouts.

#### 1. Brute Force Analysis (Host: HQ-MAIL-02)
* **Incident Profile:** The alerting queue captured high-volume, rapid sequential authentication failures targeting a critical mail server host.
* **Analysis & Remediation:** Triaged the attack vector as an automated credential-guessing attempt. The floor resolution plan requires implementing an account lockout threshold policy and enforcing robust character requirements across user profiles to eliminate the attack surface.

#### 2. Public Web Defacement Vector
* **Incident Profile:** Perimeter alerting indicated unauthorized modification of a public-facing corporate web server interface.
* **Analysis & Remediation:** The compromise points directly back to an unpatched software vulnerability (such as an input validation flaw like SQLi or RCE code execution). Operational mitigation demands scheduling emergency patch management and setting up routine vulnerability scanning cycles to close unpatched gaps before deployment.

#### 3. Third-Party Supply Chain Compromise Simulation
* **Incident Profile:** An internal endpoint exhibited anomalous outbound network connections immediately following a trusted application software update, mimicking a SolarWinds-style supply chain breach signature.
* **Analysis & Remediation:** Isolated the endpoint using EDR controls. Long-term defense controls require strict application whitelisting, removing local administrative install privileges from user workstations, and setting up rigorous network segmentation so that an isolated client subnet compromise cannot cross into server infrastructure zones.

---

## 🚨 SOC L1 Exam Notes

### Must Remember

* Misconfigurations (like wide-open cloud buckets or unchanged default passwords) are often more dangerous than zero-days because they are common and require no complex engineering to exploit.


* Servers hold a much higher severity classification than individual endpoints because they store corporate data, run critical applications, and possess internal network trust.



### Common Interview Topics

* The core operational definitions differentiating a vulnerability, an exploit, and a security incident.


* The risk profiles associated with plaintext protocols like Telnet and FTP.


* Temporary mitigation strategies when dealing with a high-severity Zero-Day vulnerability before a patch is deployed.



### Common Alert Types Related To This Topic

* Web Application Firewall (WAF) alerts flag unusual `POST` requests or exploitation strings.


* Endpoint Detection and Response (EDR) alerts capture malicious process executions or sudden kernel driver loading events.


* Identity and Access Management (IAM) alerts track configuration errors like public S3 buckets or disabled MFA flags.



---

## 🌐 Real-World Application & Observations

* An attacker exploiting a severe domain vulnerability like Zerologon (CVE-2020-1472) will immediately attempt to create domain admin accounts, triggering active privilege escalation and lateral movement alerts across internal logs.



---

## 🧠 My Takeaways

1. I realized that a closed ticket shouldn't just mean a piece of malware was deleted from a laptop; I need to look upstream at network logs to trace how that endpoint interacted with other systems before the alert fired.
2. I learned that misconfigurations are just as critical to hunt down as software bugs. A developer leaving an open database port or a public cloud bucket is essentially handing over network keys without the attacker needing to write a single line of exploit code.



---

## 💪 Skills Acquired

* Evaluated infrastructure nodes through the lens of entry, persistence, lateral movement, and exfiltration vectors.


* Differentiated the security monitoring responsibilities between internal enterprise security teams and managed security service providers (MSSPs).



---

## 💬 Potential Interview Questions & Answers

**Q: What is the operational difference between a vulnerability, an exploit, and an incident on a corporate network?**
**A:** A vulnerability is a baseline flaw or security hole in an application or operating system code. An exploit is the specific mechanism or payload an attacker runs to take advantage of that specific flaw. An incident is the resulting state of active compromise or ongoing damage occurring inside our environment that requires immediate mitigation.

**Q: If an application server triggers an alert for an unapproved kernel driver load alongside sudden outbound connections, how do you triage this?**
**A:** This indicates that the initial access phase has passed. The attacker is likely executing privilege escalation, establishing persistence, and initiating command and control traffic. Because this is a high-value server running corporate operations rather than a single user endpoint, I would flag this as a critical priority event and isolate the host to protect the internal network trust.

---

## 📖 References

* Defronix Academy SOC L1 Training - Class 03

