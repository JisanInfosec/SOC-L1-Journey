# 🛡️ Section 01: Blue Team Introduction

**Platform:** TryHackMe  
**Path:** SOC Level 1  
**Status:** Completed ✅  
**Rooms Completed:** 4/4

---

## 📖 Section Overview

This section introduced the fundamentals of Security Operations Centers (SOC), the responsibilities of a Junior SOC Analyst, common human-based attack techniques, and system-level attack vectors.

### Rooms Completed

1. Junior Security Analyst Intro
2. SOC Role in the Blue Team
3. Humans as Attack Vectors
4. Systems as Attack Vectors

---

# 🏛️ Room 01: Junior Security Analyst Intro

## 🎯 Objective

Understand the role and responsibilities of a Junior SOC Analyst and how security incidents are handled inside a SOC.

## 📚 Key Learning Points

- SOC Analysts monitor and investigate security alerts.
- SOC operations run 24/7 to maintain organizational security visibility.
- Analysts investigate suspicious activity and validate alerts.
- Analysts document findings and escalate incidents when necessary.
- Security teams collaborate to protect organizational assets.

## 👥 SOC Team Roles

### L1 Analyst
- Alert triage
- Initial investigation
- Documentation

### L2 Analyst
- Advanced investigations
- Threat hunting

### SOC Engineer
- Security tool deployment
- SIEM and EDR maintenance

### SOC Manager
- Team management
- Operational oversight

## 🔬 Practical Exercise

Performed alert triage using a simulated SOC dashboard.

Tasks completed:

- Identified a malicious IP address
- Determined the escalation path
- Blocked the malicious IP using a firewall simulation

## 💬 Interview Question

**Q: What does a Junior SOC Analyst do?**

**A:** A Junior SOC Analyst monitors security alerts, investigates suspicious activity, documents findings, and escalates confirmed incidents to senior analysts when required.

---

# 🏛️ Room 02: SOC Role in the Blue Team

## 🎯 Objective

Understand the structure of cybersecurity teams and the hierarchy within a Security Operations Center.

## 📚 Key Learning Points

### Red Team
- Simulates attacks
- Identifies vulnerabilities

### Blue Team
- Detects attacks
- Responds to incidents
- Protects systems

### GRC Team
- Governance
- Risk Management
- Compliance

## 👥 SOC Structure

- L1 Analyst
- L2 Analyst
- SOC Engineer
- SOC Manager
- Threat Intelligence Analyst
- Digital Forensics Analyst
- Application Security Engineer

## 🔬 Practical Exercise

Matched cybersecurity roles with their responsibilities inside an organization.

## 💬 Interview Question

**Q: What is the difference between an L1 and an L2 SOC Analyst?**

**A:** L1 analysts perform alert triage and initial investigation, while L2 analysts conduct deeper investigations and handle more advanced incidents.

---

# 🏛️ Room 03: Humans as Attack Vectors

## 🎯 Objective

Understand how attackers exploit human behavior and learn the controls used to reduce human-related security risks.

## 📚 Key Learning Points

- Attackers often exploit trust, fear, urgency, and curiosity.
- Humans are frequently targeted through social engineering.
- Common attacks include phishing, impersonation, malware delivery, and deepfake-based deception.
- Security awareness is an important layer of defense.

## 🛡️ Defensive Controls

### Security Awareness Training
- Teaches users how to recognize phishing and social engineering attacks.

### Anti-Phishing Solutions
- Blocks malicious emails before reaching users.

### Antivirus / EDR
- Detects and prevents malware execution.

### Verification Procedures
- Encourages employees to verify unusual requests before taking action.

## 🔬 Practical Exercise

Reviewed employee risk scenarios and updated corporate security policies to enforce Multi-Factor Authentication (MFA).

## 💬 Interview Question

**Q: Why are humans considered a major attack vector?**

**A:** Attackers target humans because manipulating trust and emotions is often easier than bypassing technical security controls.

---

# 🏛️ Room 04: Systems as Attack Vectors

## 🎯 Objective

Understand how vulnerabilities, misconfigurations, and supply chain threats can expose systems to compromise.

## 📚 Key Learning Points

- Every system has value to an attacker.
- Vulnerabilities are weaknesses in software or hardware.
- Misconfigurations are insecure settings introduced during deployment or administration.
- Zero-day vulnerabilities are exploited before a patch is available.
- CVEs provide publicly documented vulnerability information.

## 🔍 Important Concepts

### Vulnerability
A flaw or weakness in software or hardware that can be exploited.

### Misconfiguration
An insecure setting or deployment mistake made by an administrator.

### CVE
Common Vulnerabilities and Exposures database used to track publicly known vulnerabilities.

### Zero-Day
A vulnerability exploited before a fix is available.

## 🔬 Practical Exercise

Analyzed systems at risk and developed a remediation plan for vulnerable assets.

## 💬 Interview Question

**Q: What is the difference between a vulnerability and a misconfiguration?**

**A:** A vulnerability is a flaw in the software or hardware itself, while a misconfiguration is an insecure setting introduced during deployment or administration.

---

# 📊 Section 01 Summary

## Topics Covered

- SOC Fundamentals
- SOC Team Structure
- Blue Team Operations
- Human Attack Vectors
- System Attack Vectors
- Vulnerabilities
- Misconfigurations
- Social Engineering

## Key Terms Learned

- SOC (Security Operations Center)
- SIEM (Security Information and Event Management)
- EDR (Endpoint Detection and Response)
- IOC (Indicator of Compromise)
- CVE (Common Vulnerabilities and Exposures)
- Zero-Day
- MFA (Multi-Factor Authentication)
- Phishing
- Threat Intelligence

## Interview Readiness Checklist

- [x] Explain SOC structure
- [x] Explain L1 Analyst responsibilities
- [x] Differentiate Red Team and Blue Team
- [x] Explain social engineering attacks
- [x] Differentiate vulnerability and misconfiguration
- [x] Explain the basic incident escalation workflow

---

# 📝 Personal Reflection

This section helped me understand how a SOC operates, the responsibilities of a Junior SOC Analyst, and how attackers target both humans and systems. I also gained my first practical exposure to alert triage, escalation workflows, and the overall structure of a defensive security team.
