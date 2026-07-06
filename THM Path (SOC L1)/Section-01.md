# Section 01: Blue Team Introduction

## Overview

This section introduced the role of a SOC Analyst, Blue Team operations, human-based attacks, and system-based attacks.

Rooms Completed:
1. Junior Security Analyst Intro
2. SOC Role in the Blue Team
3. Humans as Attack Vectors
4. Systems as Attack Vectors

---

## Room 01: Junior Security Analyst Intro

### Objective
Understand the role and responsibilities of a Junior SOC Analyst.

### Key Learning Points
* SOC Analysts monitor security alerts[cite: 1].
* SOC operations run 24/7[cite: 1].
* Analysts investigate suspicious activity[cite: 1].
* Analysts escalate incidents when needed[cite: 1].
* Security teams work together to protect the organization[cite: 1].

### SOC Team Roles
* L1 Analyst
  * Alert triage[cite: 1]
  * Initial investigation[cite: 1]
  * Documentation[cite: 1]
* L2 Analyst
  * Advanced investigations[cite: 1]
  * Threat hunting[cite: 1]
* SOC Engineer
  * Security tool deployment[cite: 1]
  * SIEM and EDR maintenance[cite: 1]
* SOC Manager
  * Team management[cite: 1]
  * Operational oversight[cite: 1]

### Practical Exercise
Performed alert triage in a simulated SOC dashboard.
Tasks completed:
* Identified malicious IP address
* Determined escalation path
* Blocked malicious IP through firewall simulation

### Interview Question
**Q: What does a Junior SOC Analyst do?**
**A:** A Junior SOC Analyst monitors security alerts, investigates suspicious activity, documents findings, and escalates confirmed incidents to senior analysts when required[cite: 1].

---

## Room 02: SOC Role in the Blue Team

### Objective
Understand the structure of cybersecurity teams and SOC hierarchy.

### Key Learning Points
* **Red Team:** Simulates attacks and finds vulnerabilities.
* **Blue Team:** Detects attacks, responds to incidents, and protects systems[cite: 1].
* **GRC Team:** Handles governance, risk management, and compliance.

### SOC Structure
* L1 Analyst[cite: 1]
* L2 Analyst[cite: 1]
* SOC Engineer[cite: 1]
* SOC Manager[cite: 1]
* Threat Intelligence Analyst[cite: 1]
* Digital Forensics Analyst[cite: 1]
* AppSec Engineer[cite: 1]

### Practical Exercise
Matched SOC roles with their operational responsibilities.

### Interview Question
**Q: Difference between L1 and L2 SOC Analyst?**
**A:** L1 analysts perform alert triage and initial investigation. L2 analysts handle deeper investigations and incident response activities[cite: 1].

---

## Room 03: Humans as Attack Vectors

### Objective
Understand how social engineering attacks target human behavior and how to defend against them.

### Key Learning Points
* Attackers exploit human traits like trust and emotion (urgency, fear, curiosity).
* Common human-based attacks include phishing, malware downloads, impersonation, and deepfakes.
* Technology alone cannot secure an organization; human awareness is required.

### Core Defensive Controls
* **Security Awareness Training:** Teaches employees how to recognize attacks and phishing simulations.
* **Anti-Phishing Solutions:** Blocks malicious emails before they reach the user.
* **Antivirus / EDR:** Prevents malware execution on host machines.
* **Verification Policies:** Verifies unusual or high-risk requests independently.

### Practical Exercise
Reviewed employee risk profiles and updated corporate security policies to mandate multi-factor authentication (MFA).

### Interview Question
**Q: Why are humans considered a major attack vector?**
**A:** Attackers target humans because exploiting human emotions like fear or urgency is often much easier than bypassing purely technical security controls.

---

## Room 04: Systems as Attack Vectors

### Objective
Understand how software vulnerabilities, misconfigurations, and supply chain threats expose systems to compromise.

### Key Learning Points
* Every system has a specific attack value based on its role and data access.
* **Vulnerabilities:** Weaknesses or bugs in software code that can be exploited.
* **Misconfigurations:** Insecure settings created during system deployment or management.
* **Zero-Day:** A vulnerability exploited by attackers before a fix or patch is available.
* **CVE (Common Vulnerabilities and Exposures):** Publicly documented software flaws.

### Practical Exercise
Analyzed systems at risk and designed a corporate remediation plan to secure vulnerable server assets.

### Interview Question
**Q: What is the difference between a vulnerability and a misconfiguration?**
**A:** A vulnerability is a flaw built directly into the software code by the creator, while a misconfiguration is an administrative mistake made when deploying or setting up the system.

---

# Section 01 Summary

### Topics Covered
* SOC Fundamentals
* SOC Team Structure
* Blue Team Operations
* Human Attack Vectors
* System Attack Vectors
* Vulnerabilities
* Misconfigurations
* Social Engineering

### Key Terms Learned
* SOC (Security Operations Center)
* SIEM (Security Information and Event Management)
* EDR (Endpoint Detection and Response)
* IOC (Indicator of Compromise)
* CVE (Common Vulnerabilities and Exposures)
* Zero-Day
* MFA (Multi-Factor Authentication)
* Phishing
* Threat Intelligence

### Interview Readiness
* [x] Explain SOC structure
* [x] Explain L1 Analyst responsibilities
* [x] Differentiate Red Team and Blue Team
* [x] Explain social engineering attacks
* [x] Differentiate vulnerability and misconfiguration
* [x] Explain basic incident escalation workflow[cite: 1]
