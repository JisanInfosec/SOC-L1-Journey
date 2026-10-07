# Section 12: Threat Analysis Tools

**Platform:** TryHackMe  
**Path:** SOC Level 1  
**Status:** Completed  
**Rooms Completed:** 4/4  

---

# Overview

Section 12 focuses on Cyber Threat Intelligence (CTI) and the practical tools used by SOC analysts to transform raw security observables into actionable intelligence. The section covers the CTI lifecycle, intelligence classifications, threat intelligence standards, file and hash analysis, IP and domain enrichment, reputation analysis, OSINT investigation, IOC correlation, and multi-stage threat hunting. The practical exercises demonstrate how analysts validate suspicious indicators, enrich them using multiple intelligence sources, correlate host and network artifacts, and use the resulting intelligence to support incident triage, threat hunting, and defensive decision-making.

---

## Rooms Completed

- Intro to Cyber Threat Intel
- File and Hash Threat Intel
- IP and Domain Threat Intel
- Invite Only

---

# Room 01: Intro to Cyber Threat Intel

## Objective

The objective of this room was to establish a foundation in Cyber Threat Intelligence (CTI), including how raw security data is transformed into actionable intelligence. It also introduced intelligence classifications, the CTI lifecycle, threat intelligence frameworks, and standardized intelligence-sharing mechanisms.

## Key Learning Points

### Data, Information, and Intelligence

- **Data** consists of raw, discrete observables such as:
  - IP addresses
  - Domain names
  - URLs
  - File hashes
  - Log entries

- **Information** is processed or aggregated data that provides additional context and answers operational questions. For example, repeated connections from a host to a particular external IP provide more context than the IP address alone.

- **Intelligence** is information that has been analyzed and placed into context to produce actionable conclusions about threats, adversaries, campaigns, or defensive requirements.

### Four Classifications of CTI

| Intelligence Type | Purpose | Typical Audience |
|---|---|---|
| **Strategic** | Provides high-level information about threat trends, risks, and business impact | Executives, C-suite, Risk Management |
| **Operational** | Explains adversary motivations, capabilities, intent, and campaigns | Security Managers, Threat Intelligence Teams |
| **Tactical** | Describes adversary Tactics, Techniques, and Procedures (TTPs) | SOC Analysts, Detection Engineers, Security Engineers |
| **Technical** | Provides concrete IOCs such as IPs, domains, URLs, hashes, and malware artifacts | SOC Analysts, Incident Responders |

### CTI Lifecycle

The CTI lifecycle provides a structured process for producing useful intelligence.

1. **Direction**
   - Define intelligence requirements.
   - Identify defensive priorities.
   - Determine what questions the intelligence process should answer.

2. **Collection**
   - Gather raw data from internal and external sources.
   - Sources may include SIEM logs, endpoint telemetry, threat feeds, malware repositories, OSINT, and security reports.

3. **Processing**
   - Normalize, organize, parse, and enrich collected data.
   - Remove unnecessary or duplicate information.
   - Convert raw data into a format suitable for analysis.

4. **Analysis**
   - Correlate indicators and contextual information.
   - Identify adversary behavior, relationships, and patterns.
   - Produce actionable security conclusions.

5. **Dissemination**
   - Deliver intelligence to the appropriate stakeholders.
   - Technical findings may be converted into detection rules, blocklists, hunting queries, or incident reports.
   - Strategic findings may be communicated to management.

6. **Feedback**
   - Evaluate whether the intelligence answered the original intelligence requirements.
   - Identify gaps and improve future collection and analysis.

### Threat Intelligence Standards and Frameworks

- **MITRE ATT&CK**
  - Knowledge base describing adversary tactics and techniques.
  - Useful for mapping observed behavior to known attacker tradecraft.
  - Supports threat hunting, detection engineering, and incident reporting.

- **STIX**
  - Structured language for representing and exchanging cyber threat intelligence.
  - Can represent objects such as indicators, malware, threat actors, relationships, and attack patterns.

- **TAXII**
  - Protocol designed to exchange CTI programmatically.
  - Enables organizations and platforms to share structured threat intelligence.

- **Lockheed Martin Cyber Kill Chain**
  - Represents an intrusion as a sequence of attack stages:
    1. Reconnaissance
    2. Weaponization
    3. Delivery
    4. Exploitation
    5. Installation
    6. Command and Control
    7. Actions on Objectives

- **Diamond Model**
  - Analyzes intrusions through four primary vertices:
    - Adversary
    - Victim
    - Infrastructure
    - Capabilities
  - Useful for understanding relationships between different parts of an attack.

## Practical Exercise

The practical exercise simulated a phishing-related security investigation. The investigation began with a suspicious email and a downloaded executable payload. The objective was to use the available evidence to construct an initial adversary profile rather than treating individual artifacts in isolation.

The investigation methodology involved:

1. Identifying the suspicious sender and email context.
2. Examining the downloaded executable as a potential malicious artifact.
3. Treating the sender, payload, and associated infrastructure as separate but potentially related observables.
4. Applying CTI concepts to organize the evidence into an adversary profile.
5. Using frameworks such as the Cyber Kill Chain and MITRE ATT&CK to understand the potential attack progression.
6. Converting raw observations into intelligence that could support SOC detection and response activities.

The exercise demonstrated that CTI is not simply the collection of IOCs. The analyst must correlate multiple pieces of evidence and determine what they collectively indicate about an attack.

## Skills Acquired

- Distinguishing data, information, and intelligence.
- Applying the six-phase CTI lifecycle.
- Classifying intelligence according to its intended audience and purpose.
- Understanding STIX and TAXII-based intelligence sharing.
- Mapping adversary behavior to established threat frameworks.
- Building an initial threat profile from multiple observables.

## Interview Question

**Q: What is Cyber Threat Intelligence, and how would you use the CTI lifecycle as an L1/L2 SOC Analyst?**

**A:** Cyber Threat Intelligence is analyzed information about threats, threat actors, campaigns, infrastructure, and adversary behavior that can support security decision-making.

As an L1/L2 SOC Analyst, I would primarily consume and operationalize tactical and technical intelligence. For example, if an alert contains a suspicious IP address, I would first validate the indicator and enrich it using internal telemetry and external intelligence sources. I would then determine whether the indicator is associated with known malicious infrastructure, malware, or attacker activity.

The CTI lifecycle provides a structured approach:

1. **Direction** — Understand the investigation requirement.
2. **Collection** — Gather logs, endpoint data, threat feeds, and OSINT.
3. **Processing** — Normalize and organize the collected information.
4. **Analysis** — Correlate indicators and determine their significance.
5. **Dissemination** — Communicate actionable findings or create detections/blocking recommendations.
6. **Feedback** — Determine whether the intelligence was useful and identify additional investigation requirements.

The key point is that an IOC should not automatically be considered malicious simply because a reputation service reports it. CTI should provide context and confidence that can support an evidence-based SOC decision.

## Key Takeaway

Effective CTI converts raw security observations into contextual and actionable intelligence. A SOC analyst should understand both the technical indicators and the adversary behavior or campaign context surrounding them.

---

# Room 02: File and Hash Threat Intel

## Objective

The objective of this room was to develop a structured workflow for investigating suspicious files using metadata, cryptographic hashes, threat intelligence platforms, and malware sandboxes. The focus was on determining whether a file is malicious and extracting useful host and network indicators from the investigation.

## Key Learning Points

### Filename and File Path Analysis

- File names can be manipulated to make malicious executables appear legitimate.
- Common techniques include:
  - Typosquatting legitimate process names.
  - Double extensions such as `document.pdf.exe`.
  - Names resembling trusted Windows processes.
  - Disguising executable files as documents or installers.

- File location is an important contextual indicator.
- Executables running from locations such as:

```text
C:\Users\<user>\AppData\Local\Temp
```

may warrant additional investigation, particularly when combined with suspicious execution behavior.

- Analysts should consider:
  - Parent process
  - File creation time
  - User context
  - Directory permissions
  - File signer
  - Execution location
  - Persistence mechanisms

### Cryptographic Hashing

Cryptographic hashes provide deterministic fingerprints of files.

Common hashes include:

- **MD5**
- **SHA-1**
- **SHA-256**

SHA-256 is generally preferred for modern IOC tracking because of its stronger collision resistance.

A hash can be used to:

- Search public malware repositories.
- Query VirusTotal.
- Search internal security platforms.
- Correlate the same file across multiple systems.
- Determine whether a sample has previously been identified.

### Fuzzy Hashing

**SSDEEP** is a fuzzy hashing technique that can identify files with similar characteristics even when their cryptographic hashes differ.

This is useful when malware has been:

- Modified.
- Recompiled.
- Slightly altered.
- Packed or otherwise changed.

Unlike a traditional cryptographic hash, fuzzy hashing is intended for similarity comparison rather than exact file identification.

### Import Hash

**Imphash** is associated primarily with Portable Executable (PE) files and represents information about imported functions.

It can help analysts identify relationships between PE files that may belong to the same malware family or share similar functionality.

### VirusTotal

VirusTotal can be used to enrich suspicious files and hashes using multiple security vendors and datasets.

Useful information may include:

- Detection results.
- File type.
- File metadata.
- Historical submissions.
- Network connections.
- Related files.
- Behavioral information.
- Community context.

VirusTotal results should be treated as intelligence rather than absolute proof of maliciousness.

### Hybrid Analysis and Sandboxing

When a file has little or no existing reputation, sandbox analysis can provide behavioral intelligence.

A sandbox can reveal:

- Processes created.
- Files dropped.
- Registry modifications.
- Persistence mechanisms.
- DNS queries.
- HTTP/HTTPS connections.
- C2 infrastructure.
- Command execution.
- System modifications.

Observed behavior can then be mapped to frameworks such as MITRE ATT&CK.

### Static vs. Dynamic Analysis

| Analysis Type | Examples | Primary Purpose |
|---|---|---|
| **Static** | Hashes, strings, PE metadata, imports | Analyze a file without executing it |
| **Dynamic** | Sandbox execution, process monitoring, network observation | Understand runtime behavior |

### File Investigation Workflow

A practical file triage workflow can be structured as:

```text
Suspicious File
      |
      v
Filename / Path / Metadata
      |
      v
Calculate SHA-256
      |
      v
Threat Intelligence Lookup
      |
      +---- Known --> Enrich Existing Intelligence
      |
      +---- Unknown --> Sandbox Analysis
                         |
                         v
                  Extract Host / Network IOCs
                         |
                         v
                  Map to ATT&CK / Report
```

## Practical Exercise

The practical exercise focused on investigating a suspicious executable through a combination of static and dynamic threat intelligence techniques.

The investigation methodology included:

1. Examining the suspicious filename and execution path.
2. Identifying characteristics that could indicate masquerading or suspicious execution.
3. Generating cryptographic hashes for the sample.
4. Searching the hash across threat intelligence services such as **VirusTotal**.
5. Using malware analysis platforms such as **Hybrid Analysis** to investigate behavioral evidence.
6. Reviewing available detections and metadata.
7. Investigating runtime activity when static reputation information was insufficient.
8. Extracting host-based indicators such as created files, registry activity, processes, and persistence mechanisms.
9. Extracting network-based indicators such as domains, IP addresses, DNS activity, and potential C2 infrastructure.
10. Organizing the resulting indicators into a threat intelligence summary.

The exercise reinforced that a hash lookup is only the starting point of file triage. A strong investigation combines file metadata, reputation, behavior, relationships, and network activity.

## Skills Acquired

- Performing suspicious file triage.
- Generating and interpreting cryptographic hashes.
- Understanding SSDEEP and imphash use cases.
- Using VirusTotal for file and hash enrichment.
- Using malware sandbox results to extract behavioral IOCs.
- Separating static and dynamic analysis.
- Mapping malware behavior to MITRE ATT&CK.

## Interview Question

**Q: You receive a suspicious executable on an endpoint. How would you investigate it using threat intelligence tools?**

**A:** I would begin with local evidence preservation and basic static triage. I would record the filename, full path, file size, timestamps, user context, parent process, and file type. I would calculate a SHA-256 hash and use it to search trusted threat intelligence sources such as VirusTotal and internal repositories.

I would not rely solely on a vendor detection verdict. I would examine the number and quality of detections, historical submissions, related samples, file metadata, and known network relationships.

If the sample is not known, I would use an isolated malware analysis environment such as Hybrid Analysis to observe its behavior. I would look for process creation, dropped files, registry modifications, persistence, DNS requests, external connections, and possible C2 communication.

I would then extract and categorize the resulting IOCs:

- File hashes
- Domains
- IP addresses
- URLs
- File paths
- Registry keys
- Processes

Finally, I would correlate those indicators with endpoint and network telemetry in the SIEM or EDR. Based on the confidence and impact, I would recommend containment, blocking, eradication, or additional hunting.

## Key Takeaway

A file hash provides a powerful pivot point, but effective malware triage requires multiple layers of evidence. Analysts should combine static metadata, reputation intelligence, sandbox behavior, and internal telemetry before making a containment or escalation decision.

---

# Room 03: IP and Domain Threat Intel

## Objective

The objective of this room was to investigate IP addresses and domains using OSINT and threat intelligence enrichment techniques. The focus was on understanding attacker infrastructure, historical DNS relationships, reputation, exposed services, and anonymization technologies.

## Key Learning Points

### WHOIS and Domain Registration

WHOIS information can provide:

- Registrar information.
- Registration dates.
- Expiration dates.
- Name servers.
- Domain ownership information where available.
- Registration history.

A recently registered domain is not automatically malicious, but a newly registered domain combined with suspicious DNS activity, malicious content, or phishing indicators can significantly increase its investigative value.

### Passive DNS

**Passive DNS (pDNS)** provides historical DNS resolution information.

It can help analysts identify:

- Previous IP addresses associated with a domain.
- Infrastructure changes.
- Shared hosting relationships.
- Historical C2 infrastructure.
- Infrastructure reuse by threat actors.

This enables analysts to pivot from one indicator to potentially related infrastructure.

### Subdomain Enumeration

Subdomain discovery can identify:

- Staging environments.
- Development systems.
- Administrative interfaces.
- Forgotten infrastructure.
- Additional domains associated with an organization.

From a threat intelligence perspective, related subdomains may reveal infrastructure relationships that are not obvious from the primary domain.

### IP Address and ASN Analysis

An IP address should be investigated in context rather than treated as a standalone indicator.

Analysts can examine:

- ASN.
- Hosting provider.
- ISP.
- Geolocation.
- Historical reputation.
- Associated domains.
- Abuse reports.

An unfamiliar ASN or hosting provider may provide useful context, but infrastructure ownership alone is not evidence of malicious activity.

### Reputation Analysis

Threat intelligence services such as:

- **VirusTotal**
- **AbuseIPDB**
- **Cisco Talos**

can provide information about an IP or domain's reputation.

Analysts should evaluate:

- Historical abuse reports.
- Malicious activity reports.
- Scanner activity.
- Botnet associations.
- Malware relationships.
- Reporting confidence.
- Recency of the evidence.

### Shodan and Censys

Internet-wide search engines such as **Shodan** and **Censys** can provide passive information about externally exposed infrastructure.

Useful information includes:

- Open ports.
- Service banners.
- TLS certificates.
- Software information.
- Hosting characteristics.
- Internet-facing services.

This information can help analysts understand infrastructure without directly interacting with the target service.

### VPN, Proxy, and Tor Analysis

Threat actors may use:

- Tor exit nodes.
- Commercial VPN providers.
- Residential proxies.
- Public proxy infrastructure.
- Compromised systems.

An IP associated with a VPN or proxy should not automatically be classified as malicious. Analysts must distinguish between legitimate privacy infrastructure and infrastructure actively associated with malicious activity.

### Defanging Indicators

Indicators are commonly defanged when included in reports to prevent accidental interaction.

Examples:

```text
https://malicious.example.com
```

can be represented as:

```text
hxxps://malicious[.]example[.]com
```

Similarly:

```text
192.0.2.10
```

may be written as:

```text
192[.]0[.]2[.]10
```

Defanging is particularly useful when publishing threat reports, tickets, emails, or intelligence feeds.

## Investigation Workflow

A structured IP/domain investigation can follow this process:

1. **Identify the Indicator**
   - Determine whether the observable is an IP, domain, URL, or related network artifact.

2. **Query Registration Data**
   - Review WHOIS information and domain age.

3. **Review Historical DNS**
   - Investigate previous domain-to-IP relationships.

4. **Profile the IP**
   - Identify the ASN, hosting provider, ISP, and geographic information.

5. **Check Reputation**
   - Query services such as VirusTotal, AbuseIPDB, and Cisco Talos.

6. **Inspect Exposed Services**
   - Use passive internet intelligence platforms such as Shodan or Censys.

7. **Identify Related Infrastructure**
   - Pivot through domains, IP addresses, certificates, DNS history, and other relationships.

8. **Assess Anonymization**
   - Determine whether the infrastructure belongs to a VPN, Tor, proxy, or residential proxy network.

9. **Correlate with Internal Telemetry**
   - Search the SIEM, DNS logs, proxy logs, firewall logs, and EDR for organizational activity involving the indicator.

10. **Assign Risk**
   - Consider reputation, context, historical behavior, internal observations, and confidence before deciding whether the indicator should be blocked or escalated.

## Practical Exercise

The practical exercise focused on enriching suspicious network infrastructure and determining whether the observed IP and domain characteristics indicated potentially malicious activity.

The investigation included:

1. Reviewing domain registration information using WHOIS-style intelligence.
2. Examining historical DNS relationships through passive DNS information.
3. Investigating the IP's ASN, hosting organization, and geographic context.
4. Checking reputation across services such as **AbuseIPDB**, **VirusTotal**, and **Cisco Talos**.
5. Reviewing exposed services and infrastructure characteristics using passive internet-scanning intelligence.
6. Investigating whether anonymization technologies such as VPNs or proxies could explain the observed source IP.
7. Pivoting between related domains and IP addresses to identify infrastructure relationships.
8. Evaluating the collected evidence rather than relying on a single reputation score.

The technical conclusion was that IP and domain investigation requires contextual enrichment. Reputation, ownership, DNS history, hosting information, and internal telemetry should be combined to determine the actual risk associated with an indicator.

## Skills Acquired

- Performing IP and domain enrichment.
- Interpreting WHOIS and domain registration information.
- Using passive DNS for infrastructure pivoting.
- Performing ASN and hosting-provider analysis.
- Using VirusTotal, AbuseIPDB, and Cisco Talos for reputation analysis.
- Understanding VPN, proxy, and Tor-related infrastructure.
- Correlating external intelligence with internal SOC telemetry.
- Safely documenting and defanging network indicators.

## Interview Question

**Q: An endpoint has connected to an IP address that AbuseIPDB reports as malicious. How would you determine whether the connection represents a real security incident?**

**A:** I would not immediately block or escalate the IP solely because of an AbuseIPDB reputation score. Reputation is one source of evidence and needs to be evaluated in context.

First, I would identify the endpoint, user, timestamp, destination port, protocol, process responsible for the connection, and frequency of communication. I would then enrich the IP using multiple sources such as VirusTotal, Cisco Talos, WHOIS information, ASN databases, passive DNS, and potentially Shodan or Censys.

I would investigate whether the IP is associated with known malware, C2 infrastructure, scanning activity, phishing, or other malicious behavior. I would also determine whether the IP belongs to a legitimate cloud provider, VPN, CDN, or shared hosting environment where reputation may be affected by unrelated activity.

Next, I would pivot from the IP to related domains, DNS history, certificates, URLs, and other infrastructure. On the internal side, I would search proxy, firewall, DNS, and EDR telemetry for additional connections.

If the evidence shows repeated communication from a suspicious process to infrastructure associated with known malicious activity, I would raise the confidence of the alert and follow the organization's containment and escalation procedures.

## Key Takeaway

An IP or domain reputation score is an investigative starting point, not a final verdict. Effective network threat intelligence combines multiple external sources with internal telemetry to distinguish genuine malicious activity from legitimate or shared infrastructure.

---

# Room 04: Invite Only

## Objective

The objective of this room was to simulate a realistic Tier-1/Tier-2 SOC investigation involving multiple related indicators. The investigation required analysts to pivot between file hashes, execution relationships, network infrastructure, malware behavior, and external OSINT to reconstruct a multi-stage attack chain.

## Key Learning Points

### Indicator Pivoting

Threat intelligence investigations rarely end with the first IOC.

An analyst may begin with:

```text
IP Address
   |
   +--> Related Domains
   |
   +--> Communicating Files
   |
   +--> Malware Family
   |
   +--> Campaign / Threat Actor
```

Similarly, a file hash can be pivoted into:

```text
File Hash
   |
   +--> Filename
   |
   +--> Parent Process
   |
   +--> Dropped Files
   |
   +--> Network Connections
   |
   +--> Related Samples
   |
   +--> Malware Family
```

The objective is to identify relationships rather than investigate each indicator independently.

### Parent and Child Process Relationships

Process relationships can reveal the execution chain of a compromise.

For example:

```text
Initial User Action
       |
       v
Downloader / Installer
       |
       v
Script or Secondary Payload
       |
       v
Primary Malware
       |
       v
C2 Communication
```

Understanding these relationships helps determine:

- Initial execution mechanism.
- Malware delivery method.
- Secondary payloads.
- Persistence mechanisms.
- Final payload.
- C2 infrastructure.

### Multi-Stage Malware Delivery

Modern malware campaigns frequently use multiple stages rather than delivering the final payload directly.

An initial file may:

- Download another executable.
- Drop a script.
- Modify the registry.
- Establish persistence.
- Retrieve additional components.
- Connect to C2 infrastructure.

This means an analyst investigating only the original file may miss the actual primary payload.

### Correlating Network and Host IOCs

Host and network indicators should be correlated.

For example:

```text
Suspicious Process
       |
       +--> File Hash
       |
       +--> DNS Request
       |
       +--> Destination IP
       |
       +--> C2 Domain
       |
       +--> Dropped Payload
```

This correlation can significantly increase confidence in an investigation.

### OSINT Correlation

When internal or automated intelligence sources provide incomplete information, public reporting can provide additional context.

Analysts can search for:

- Unique file hashes.
- Malware names.
- IP addresses.
- Domains.
- Campaign identifiers.
- Distinctive execution techniques.
- Social engineering methods.

Public threat reports may reveal:

- Delivery mechanisms.
- Malware families.
- Threat actor behavior.
- Campaign timelines.
- Exploited platforms.
- Credential theft techniques.

### Social Engineering and Initial Access

Threat actors may abuse trusted services and familiar workflows to bypass user suspicion and technical controls.

Examples include:

- Phishing messages.
- Fake CAPTCHA prompts.
- Fake software update prompts.
- Malicious invitation links.
- Compromised or trusted collaboration platforms.

The presence of a legitimate service in an attack does not make the resulting activity legitimate.

## Practical Exercise

The practical exercise simulated a multi-stage malware investigation beginning with two primary indicators: a suspicious IP address and a SHA-256 file hash.

The investigation followed a relationship-based threat intelligence workflow:

1. **Initial Artifact Triage**
   - Investigated the provided SHA-256 hash.
   - Identified the associated binary and available file intelligence.

2. **Execution Chain Analysis**
   - Examined relationships between the suspicious executable and its parent process.
   - Investigated files or scripts that were created or executed as part of the chain.

3. **Dropped Payload Analysis**
   - Identified additional artifacts associated with the original execution.
   - Examined secondary scripts and payloads to understand the progression of the compromise.

4. **Infrastructure Pivoting**
   - Investigated the supplied IP address.
   - Reviewed files and malware samples communicating with or associated with the infrastructure.

5. **Malware Family Correlation**
   - Correlated host artifacts and network infrastructure to identify the likely malware family involved in the activity.

6. **OSINT Investigation**
   - Searched public threat intelligence reports using unique hashes, infrastructure indicators, malware terminology, and behavioral characteristics.
   - Used public reporting to establish campaign context.

7. **Attack Chain Reconstruction**
   - Connected the initial delivery method, execution chain, secondary payloads, credential-related activity, and network infrastructure into a unified investigation.

8. **Threat Intelligence Assessment**
   - Converted the collected indicators and relationships into actionable intelligence that could support detection, containment, and threat hunting.

The investigation demonstrated the importance of multi-source correlation. A single hash or IP provided only limited context; pivoting between host artifacts, infrastructure, and public reporting revealed the broader attack chain.

## Skills Acquired

- Performing multi-indicator threat intelligence investigations.
- Pivoting between hashes, files, processes, IPs, and domains.
- Reconstructing multi-stage malware execution chains.
- Identifying parent-child process relationships.
- Correlating host and network IOCs.
- Using OSINT to validate campaign context.
- Identifying malware families from multiple technical relationships.
- Translating threat intelligence into SOC hunting and detection opportunities.

## Interview Question

**Q: You are given a suspicious SHA-256 hash and an IP address from an alert. How would you correlate multiple threat intelligence sources to determine whether they belong to the same attack campaign?**

**A:** I would begin by treating the hash and IP as independent starting points and then pivot through their relationships.

For the SHA-256 hash, I would identify the associated file, metadata, malware classifications, related samples, parent processes, dropped files, and network connections using platforms such as VirusTotal or an internal malware repository.

For the IP, I would investigate its reputation, ASN, hosting provider, historical DNS relationships, associated domains, and known malicious activity using sources such as AbuseIPDB, Cisco Talos, VirusTotal, and passive DNS.

I would then compare the results. For example, if the suspicious file communicates with the investigated IP, or if multiple related samples communicate with the same infrastructure, that creates a strong relationship between the host artifact and network indicator.

I would continue pivoting through:

- File hashes
- Parent and child processes
- Dropped files
- Domains
- IP addresses
- DNS records
- URLs
- Certificates
- Malware families
- Public threat reports

I would also compare the observed behavior against MITRE ATT&CK techniques and known campaign characteristics.

Finally, I would assess confidence based on the quality, recency, and independence of the evidence. If multiple independent sources support the same relationship, I would consider the correlation significantly stronger than a single reputation hit.

As an L1/L2 analyst, the goal is not merely to identify that an IOC is malicious. The goal is to reconstruct enough of the attack chain to determine scope, identify additional systems or indicators that may be affected, and provide actionable information for containment and threat hunting.

## Key Takeaway

Threat intelligence becomes significantly more valuable when indicators are correlated into relationships. Effective SOC investigations pivot from one observable to another until the analyst can reconstruct the attack chain and identify additional opportunities for detection and containment.

---

# 📊 Section 12 Summary

## Topics Covered

- Cyber Threat Intelligence fundamentals.
- Data vs. information vs. intelligence.
- Strategic, Operational, Tactical, and Technical CTI.
- The six-phase CTI lifecycle.
- Intelligence requirements and collection.
- Threat intelligence processing and analysis.
- Intelligence dissemination and feedback.
- MITRE ATT&CK.
- STIX and TAXII.
- Lockheed Martin Cyber Kill Chain.
- Diamond Model.
- File and malware triage.
- Cryptographic hashing.
- MD5, SHA-1, and SHA-256.
- SSDEEP fuzzy hashing.
- Imphash.
- Filename and file path analysis.
- Static malware analysis.
- Dynamic malware analysis.
- VirusTotal.
- Hybrid Analysis.
- Malware sandboxing.
- Host-based IOC extraction.
- Network-based IOC extraction.
- IP reputation analysis.
- Domain reputation analysis.
- WHOIS.
- Passive DNS.
- ASN investigation.
- Domain age and registration analysis.
- Subdomain enumeration.
- Shodan and Censys.
- VPN, Tor, and proxy identification.
- Indicator defanging.
- IOC pivoting.
- Parent-child process analysis.
- Multi-stage malware delivery.
- Malware family identification.
- OSINT-based threat intelligence.
- Host and network IOC correlation.
- Threat hunting and attack-chain reconstruction.

---

## Key Terms Learned

- **CTI** — Cyber Threat Intelligence
- **IOC** — Indicator of Compromise
- **TTP** — Tactics, Techniques, and Procedures
- **OSINT** — Open-Source Intelligence
- **C2** — Command and Control
- **SIEM** — Security Information and Event Management
- **EDR** — Endpoint Detection and Response
- **MD5** — Message-Digest Algorithm 5
- **SHA-1** — Secure Hash Algorithm 1
- **SHA-256** — Secure Hash Algorithm 256
- **SSDEEP** — Fuzzy hashing algorithm used for file similarity analysis
- **Imphash** — Import hash used for PE file correlation
- **STIX** — Structured Threat Information Expression
- **TAXII** — Trusted Automated Exchange of Intelligence Information
- **MITRE ATT&CK** — Knowledge base of adversary tactics and techniques
- **Cyber Kill Chain** — Seven-stage intrusion model
- **Diamond Model** — Adversary, Victim, Infrastructure, and Capabilities model
- **WHOIS** — Domain registration information service
- **pDNS** — Passive DNS
- **ASN** — Autonomous System Number
- **C2 Infrastructure** — Infrastructure used by malware or threat actors for command and control
- **NRD** — Newly Registered Domain
- **IOC Pivoting** — Using one indicator to discover related indicators
- **Sandbox** — Isolated environment for safely analyzing suspicious files
- **Threat Hunting** — Proactive search for evidence of malicious activity
- **IOC Defanging** — Modifying indicators to prevent accidental interaction
- **VirusTotal** — Multi-source file, URL, domain, and IP intelligence platform
- **Hybrid Analysis** — Malware analysis and sandboxing platform
- **AbuseIPDB** — IP reputation and abuse-reporting platform
- **Cisco Talos** — Threat intelligence and reputation service
- **Shodan** — Internet-connected device and service discovery platform
- **Censys** — Internet infrastructure and host discovery platform

---

## Skills Acquired

- Analyze raw security data and convert it into actionable CTI.
- Apply the CTI lifecycle to security investigations.
- Classify intelligence according to operational requirements and audience.
- Understand and interpret STIX/TAXII-based intelligence sharing.
- Map observed adversary activity to MITRE ATT&CK.
- Analyze suspicious filenames, paths, metadata, and execution context.
- Generate and investigate cryptographic file hashes.
- Use SSDEEP and imphash for malware similarity and correlation.
- Perform file reputation analysis using VirusTotal.
- Perform behavioral malware analysis using sandbox intelligence.
- Extract host-based and network-based IOCs.
- Investigate suspicious IP addresses and domains.
- Perform WHOIS and passive DNS analysis.
- Investigate ASN and hosting-provider relationships.
- Assess IP and domain reputation using multiple intelligence sources.
- Investigate exposed infrastructure using Shodan and Censys.
- Identify potential VPN, proxy, and Tor infrastructure.
- Safely document and defang IOCs.
- Pivot between related indicators during investigations.
- Reconstruct parent-child execution chains.
- Analyze multi-stage malware delivery.
- Correlate host and network telemetry.
- Use OSINT to validate and expand technical findings.
- Identify relationships between malware samples and infrastructure.
- Convert threat intelligence findings into detection and hunting opportunities.
- Support evidence-based SOC escalation and containment decisions.

---

## Personal Reflection

Threat Intelligence has demonstrated to me that effective SOC analysis is not simply about recognizing whether an IP, domain, or file hash appears malicious. The real value comes from understanding the context and relationships surrounding an indicator. During daily SOC triage, a suspicious alert can be enriched using reputation services, endpoint telemetry, DNS history, malware intelligence, and OSINT to determine whether the activity represents a genuine threat or a false positive. IOC pivoting can also reveal additional compromised hosts, related infrastructure, dropped payloads, or previously unknown indicators. By combining multiple independent sources and validating the evidence before making a decision, a SOC analyst can reduce false positives, accelerate alert investigation, improve threat-hunting coverage, and provide incident responders with a much clearer picture of the attack chain. CTI therefore acts as a bridge between raw security telemetry and informed defensive action.
```
