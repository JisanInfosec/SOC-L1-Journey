# Section 04: Threat Intelligence & Adversary Methodologies

**Platform:** TryHackMe  
**Path:** SOC Level 1  
**Status:** Completed

## Overview

This section focused on understanding attacker behavior, threat intelligence frameworks, and detection strategies used by SOC analysts. The rooms introduced the Pyramid of Pain, Cyber Kill Chain, Unified Kill Chain, MITRE ATT&CK, and practical threat-hunting exercises.

Rooms Completed:

1. Pyramid of Pain
2. Cyber Kill Chain
3. Unified Kill Chain
4. MITRE
5. Summit
6. Eviction
7. Friday Overtime (Bonus)

---

# Room 01: Pyramid of Pain

## Objective

Understand how different indicators impact attackers and learn why behavioral detection provides greater defensive value than simple IOC blocking.

## Key Learning Points

### Pyramid of Pain Levels

1. Hash Values
2. IP Addresses
3. Domain Names
4. Host & Network Artifacts
5. Tools
6. TTPs (Tactics, Techniques, and Procedures)

### Core Concept

The higher defenders move up the pyramid, the more effort, time, and resources attackers must spend to modify their operations.

### Additional Concepts

- MD5, SHA1, SHA256 hashing
- Fuzzy Hashing (SSDeep)
- Punycode domains
- Host and network artifacts
- Behavioral detection

## Practical Exercise

Completed a matching exercise by placing indicators into the correct Pyramid of Pain level.

## Key Takeaway

Blocking hashes, IPs, and domains can slow attackers temporarily, but detecting tools and TTPs forces them to redesign their attack methodology.

---

# Room 02: Cyber Kill Chain

## Objective

Understand the seven phases of a cyber attack and identify opportunities to detect or disrupt adversary activity.

## Key Learning Points

### Cyber Kill Chain

1. Reconnaissance
2. Weaponization
3. Delivery
4. Exploitation
5. Installation
6. Command and Control (C2)
7. Actions on Objectives

### Attack Techniques Discussed

- OSINT
- Email harvesting
- Phishing
- Spear phishing
- USB attacks
- Watering hole attacks
- Drive-by downloads
- Web shells
- DNS tunneling
- Timestomping
- Shadow copy deletion

## Practical Exercise

Mapped attacker activities to their corresponding Kill Chain stages.

## Key Takeaway

Breaking any stage of the Kill Chain can prevent the attacker from reaching their objective.

---

# Room 03: Unified Kill Chain

## Objective

Learn a modern threat modeling framework that combines traditional kill chain concepts with attacker behavior analysis.

## Key Learning Points

### Three Main Pillars

#### IN (Initial Foothold)

- Reconnaissance
- Weaponization
- Social Engineering
- Exploitation
- Persistence
- Defense Evasion
- Command and Control

#### THROUGH (Network Propagation)

- Discovery
- Privilege Escalation
- Credential Access
- Execution
- Lateral Movement
- Pivoting

#### OUT (Actions on Objectives)

- Collection
- Exfiltration
- Impact
- Objectives

### Additional Concepts

- STRIDE
- DREAD
- CVSS
- Threat Modeling

## Practical Exercise

Classified attacker actions into the correct Unified Kill Chain phases.

## Key Takeaway

The Unified Kill Chain provides better visibility into attacker movement inside enterprise environments than the traditional Cyber Kill Chain.

---

# Room 04: MITRE ATT&CK

## Objective

Learn how threat intelligence, attacker behaviors, and defensive strategies are organized using MITRE frameworks.

## Key Learning Points

### Core MITRE Resources

#### ATT&CK

Knowledge base of real-world adversary tactics and techniques.

#### CAR

Cyber Analytics Repository containing detection logic and analytics.

#### ENGAGE

Framework for cyber deception and adversary engagement.

#### D3FEND

Defensive countermeasure knowledge base.

#### Emulation Plans

Attack simulation playbooks based on real threat groups.

### Additional Concepts

- APT Groups
- Tactics, Techniques, and Procedures (TTPs)
- Detection Engineering
- Threat-Informed Defense
- ATT&CK Navigator
- CTID

## Practical Exercise

Used MITRE resources to identify threat groups, software, mitigations, and attack techniques.

## Key Takeaway

MITRE ATT&CK enables defenders to focus on adversary behavior instead of relying solely on indicators such as IPs and hashes.

---

# Room 05: Summit

## Objective

Apply Pyramid of Pain concepts through practical detection engineering and Sigma rule creation.

## Practical Activities

### Sample 1

Blocked a malicious file hash.

### Sample 2

Created a firewall rule to block a malicious IP address.

### Sample 3

Created DNS filtering rules for malicious domains.

### Sample 4

Built Sigma rules to detect registry modifications used to disable Windows Defender.

### Sample 5

Created behavioral detection rules for C2 beaconing activity.

### Sample 6

Created Sigma rules to detect command-line activity associated with data exfiltration.

## Key Takeaway

Behavior-based detections provide longer-term defensive value than simple IOC blocking.

---

# Room 06: Eviction

## Objective

Use Cyber Threat Intelligence and MITRE ATT&CK to investigate an APT campaign.

## Scenario

Investigated APT28 activity targeting the organization.

## Key Learning Points

### Initial Access

- Spear phishing links

### Execution

- PowerShell
- Windows Command Shell

### Persistence

- Registry modifications

### Defense Evasion

- Rundll32 abuse

### Discovery

- Network reconnaissance

### Lateral Movement

- SMB
- Windows Admin Shares

### Exfiltration

- External proxies
- Multi-hop proxies

## Key Takeaway

Threat intelligence can be mapped directly to MITRE ATT&CK techniques to improve detection and monitoring.

---

# Bonus Room: Friday Overtime

## Objective

Perform a realistic threat investigation using OSINT, malware analysis, and threat intelligence.

## Investigation Activities

### Malware Analysis

- Extracted malicious files
- Calculated SHA1 hashes
- Identified malware family

### Threat Intelligence

- Investigated malware using VirusTotal
- Linked activity to the Evasive Panda APT group
- Mapped activity to MITRE ATT&CK T1123 (Audio Capture)

### Infrastructure Analysis

- Investigated C2 infrastructure
- Identified associated malware campaigns
- Discovered related Android spyware activity

### IOC Handling

- Practiced IOC defanging
- Researched malicious domains and IPs safely

## Key Takeaway

Threat intelligence investigations often start from a single indicator and expand through pivoting into larger adversary infrastructure.

---

# Section 04 Summary

## Topics Covered

- Pyramid of Pain
- Cyber Kill Chain
- Unified Kill Chain
- MITRE ATT&CK
- Threat Intelligence
- Detection Engineering
- Sigma Rules
- Threat Hunting
- IOC Analysis
- Malware Analysis

## Key Terms Learned

- IOC
- TTP
- APT
- ATT&CK
- D3FEND
- ENGAGE
- CAR
- Sigma Rule
- C2
- OSINT
- CTI
- Fuzzy Hashing
- Beaconing

## Practical Skills Developed

- IOC Investigation
- MITRE ATT&CK Mapping
- Threat Intelligence Analysis
- Sigma Rule Creation
- Malware Hash Analysis
- C2 Detection
- Behavioral Detection Engineering

## Personal Reflection

This section significantly improved my understanding of attacker behavior and threat intelligence. I learned how SOC analysts move beyond basic indicators such as hashes and IP addresses and focus on behavioral detection, threat intelligence, and MITRE ATT&CK mapping to build stronger defenses against real-world adversaries.
