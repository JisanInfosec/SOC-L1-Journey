# Section 06: Network Traffic Analysis

**Platform:** TryHackMe  
**Path:** SOC Level 1  
**Status:** Completed  
**Rooms Completed:** 5/5

## Overview

This section introduced the fundamentals of Network Traffic Analysis (NTA) and the tools SOC analysts use to monitor, inspect, and investigate network communications. It covered network traffic visibility, packet analysis using Wireshark, advanced filtering techniques, traffic investigation, and network forensics using NetworkMiner. Through multiple hands-on labs, I learned how to analyze packet captures (PCAPs), detect suspicious network activity, investigate common attack techniques, and extract valuable forensic evidence from captured network traffic.

## Rooms Completed

- Network Traffic Basics
- Wireshark: The Basics
- Wireshark: Packet Operations
- Wireshark: Traffic Analysis
- NetworkMiner

---

## Room 01: Network Traffic Basics

### Objective

Understand the fundamentals of Network Traffic Analysis (NTA), why it is important in a Security Operations Center (SOC), and how analysts capture and analyze network traffic to identify malicious activity.

### Key Learning Points

#### What is Network Traffic Analysis?

Network Traffic Analysis (NTA) is the process of capturing, inspecting, and analyzing network traffic to understand how devices communicate and to identify suspicious or malicious activity.

Unlike traditional log analysis, NTA provides visibility into actual network packets and communication patterns.

#### Why Network Traffic Analysis is Important

Network traffic analysis helps SOC analysts:

- Validate security alerts.
- Detect malicious communications.
- Identify malware downloads.
- Detect command-and-control (C2) traffic.
- Investigate data exfiltration.
- Build incident timelines.

#### Traffic Collection Methods

Common methods for collecting network traffic include:

##### Network TAP

A hardware device that copies network traffic to a monitoring system without interrupting communication.

##### Port Mirroring (SPAN)

A switch feature that duplicates traffic from selected ports for monitoring and analysis.

##### Network Logs and Flow Data

Logs and technologies such as NetFlow and IPFIX provide network metadata that helps identify communication patterns and anomalies.

#### Understanding Traffic Across the TCP/IP Model

Network traffic analysis requires understanding communication across different layers:

##### Application Layer

Contains application protocols and user data such as HTTP, DNS, SMTP, and FTP.

##### Transport Layer

Uses TCP and UDP to manage communication between devices.

##### Internet Layer

Handles IP addressing and packet routing.

##### Link Layer

Responsible for communication between devices on the same local network using technologies such as Ethernet and ARP.

#### Network Traffic Flow

##### North-South Traffic

Traffic entering or leaving the organization's network.

Examples:

- HTTPS
- DNS
- VPN
- SSH

##### East-West Traffic

Internal communication between devices within the network.

Examples:

- SMB
- Kerberos
- LDAP
- Internal database traffic

### Practical Exercise

Analyzed captured network traffic to understand how data flows across a network.

Activities included:

- Examined HTTP network traffic.
- Investigated DNS communications.
- Identified suspicious traffic patterns.
- Inspected packet payloads.
- Analyzed north-south and east-west traffic.

### Skills Acquired

- Understood Network Traffic Analysis concepts.
- Identified different traffic collection methods.
- Distinguished between network traffic flows.
- Examined network communications across TCP/IP layers.

### Interview Question

**Q: Why is Network Traffic Analysis important for SOC analysts?**

**A:** Network Traffic Analysis provides visibility into network communications, allowing analysts to detect suspicious activity, investigate security incidents, identify malware communications, and reconstruct attack timelines.

### Key Takeaway

Network Traffic Analysis provides deeper visibility than traditional logs by allowing analysts to inspect actual packet data and identify malicious communications that may not appear in standard security logs.

---

## Room 02: Wireshark: The Basics

### Objective

Learn the fundamentals of Wireshark, understand how packet captures (PCAPs) are analyzed, and use its core features to investigate network traffic and security events.

### Key Learning Points

#### What is Wireshark?

Wireshark is an open-source network protocol analyzer used to capture and inspect network traffic. It allows analysts to examine packets in detail, troubleshoot network issues, and investigate suspicious activity.

Unlike an Intrusion Detection System (IDS), Wireshark does not detect or block attacks automatically. Instead, it provides detailed visibility into network communications for manual analysis.

#### Wireshark Interface

Wireshark consists of three main panels:

##### Packet List

Displays a summary of all captured packets, including timestamps, source and destination addresses, protocols, and packet information.

##### Packet Details

Breaks down the selected packet into its protocol layers and header fields.

##### Packet Bytes

Displays the raw packet data in hexadecimal and ASCII format.

#### Packet Dissection

Wireshark analyzes each packet layer by layer, allowing analysts to inspect communication throughout the OSI model.

Key information includes:

- Frame information
- Source and destination MAC addresses
- Source and destination IP addresses
- TCP or UDP ports
- Application protocols such as HTTP, DNS, FTP, and SMB

#### Packet Navigation

Wireshark provides several features that simplify investigations:

- Search for specific packets.
- Jump directly to a packet number.
- Mark important packets.
- Add analyst comments.
- Export transferred files from supported protocols.

#### Display Filters

Display filters allow analysts to focus only on relevant traffic without modifying the original capture.

Common filtering techniques include:

- Filtering by protocol
- Filtering by IP address
- Filtering by port number
- Following communication streams
- Filtering conversations between hosts

### Practical Exercise

Analyzed multiple packet capture (PCAP) files using Wireshark.

Activities included:

- Explored the Wireshark interface.
- Examined packet details across different protocol layers.
- Navigated large packet captures using search features.
- Applied display filters to isolate specific traffic.
- Followed TCP and HTTP streams to reconstruct network conversations.
- Exported files transferred through network sessions.

### Skills Acquired

- Navigated the Wireshark interface.
- Inspected packet headers and payloads.
- Applied display filters during investigations.
- Reconstructed network conversations.
- Extracted transferred files from packet captures.

### Interview Question

**Q: What is the purpose of display filters in Wireshark?**

**A:** Display filters allow analysts to focus on specific packets that match defined criteria without modifying the original packet capture, making investigations faster and more efficient.

### Key Takeaway

Wireshark is one of the most important tools used by SOC analysts for network investigations. Understanding its interface, packet structure, and filtering capabilities enables analysts to efficiently investigate network communications and identify suspicious activity.

# Room 03: Wireshark: Packet Operations

## Objective

Learn advanced Wireshark features used by SOC analysts to investigate network traffic, analyze communication patterns, apply advanced display filters, and efficiently locate suspicious network activity.

## Key Learning Points

### Traffic Statistics

Wireshark provides built-in statistical tools that help analysts understand overall network activity before performing detailed packet analysis.

Common statistics include:

#### Endpoints

Displays all devices communicating within the capture, including IP addresses, MAC addresses, transmitted bytes, and traffic volume.

#### Conversations

Shows communication sessions between two endpoints, helping analysts identify the most active connections.

#### Protocol Statistics

Summarizes network protocols such as HTTP, DNS, TCP, and UDP to understand protocol usage and communication trends.

### Capture Filters vs Display Filters

Understanding the difference between these filters is essential during investigations.

#### Capture Filters

Capture filters determine which packets are recorded during packet capture. Packets that do not match the filter are not captured.

#### Display Filters

Display filters are applied after traffic has been captured. They simply hide or display packets without modifying the original capture file.

### Advanced Display Filters

Wireshark supports advanced filtering techniques to narrow investigations.

Common operators include:

- Equal (==)
- Not Equal (!=)
- Greater Than (>)
- Less Than (<)
- AND
- OR
- NOT

Additional filtering functions include:

- **contains** – Searches for specific text within a field.
- **matches** – Uses regular expressions (Regex) to search for patterns.
- **in** – Matches multiple values within a single filter.
- **string()** – Converts numerical values into strings for advanced searches.

### Protocol-Based Filtering

Analysts commonly filter traffic based on specific protocols.

Examples include:

- IP traffic
- TCP and UDP traffic
- HTTP requests and responses
- DNS queries and responses

These filters help isolate suspicious communications and reduce unnecessary network noise.

### Configuration Profiles

Wireshark allows analysts to create different configuration profiles containing customized layouts, coloring rules, and display filters for different investigation scenarios.

## Practical Exercise

Used advanced Wireshark features to investigate packet captures.

Activities included:

- Reviewed endpoint and conversation statistics.
- Analyzed protocol distribution.
- Applied advanced display filters.
- Filtered HTTP, DNS, TCP, and UDP traffic.
- Used logical operators to refine searches.
- Investigated communication patterns between hosts.
- Switched between configuration profiles during analysis.

## Skills Acquired

- Analyzed network conversations.
- Used Wireshark statistical tools.
- Applied advanced display filters.
- Investigated protocol-specific traffic.
- Customized Wireshark profiles for investigations.

## Interview Question

**Q: What is the difference between a Capture Filter and a Display Filter?**

**A:** A Capture Filter determines which packets are collected during packet capture, while a Display Filter only controls which packets are shown after the traffic has already been captured.

## Key Takeaway

Advanced filtering and statistical analysis allow SOC analysts to quickly identify suspicious communication patterns, reduce investigation time, and focus on the most relevant network traffic during incident investigations.
# Room 04: Wireshark: Traffic Analysis

## Objective

Apply Wireshark to investigate real-world network attacks, identify suspicious traffic patterns, analyze common attack techniques, and perform network-based incident investigations.

## Key Learning Points

### Detecting Network Reconnaissance

Wireshark can be used to identify common network scanning techniques by analyzing TCP flags, packet behavior, and protocol characteristics.

Common reconnaissance activities include:

- TCP Connect Scans
- TCP SYN Scans
- UDP Scans

These scans often indicate that an attacker is attempting to discover active hosts and open services before launching an attack.

### Detecting ARP Spoofing

ARP spoofing is a Man-in-the-Middle (MITM) attack where an attacker sends forged ARP messages to associate their MAC address with another device's IP address.

Indicators of ARP spoofing include:

- Duplicate ARP responses
- Conflicting MAC-to-IP mappings
- Unexpected ARP traffic

### Detecting Network Tunneling

Attackers may abuse legitimate protocols to hide malicious communication or exfiltrate data.

Common tunneling techniques include:

#### ICMP Tunneling

Uses ICMP packets to transfer data or establish covert communication channels.

#### DNS Tunneling

Encodes commands or sensitive information inside DNS queries and responses to bypass network security controls.

### Investigating Cleartext Protocols

Protocols that transmit data without encryption may expose sensitive information during investigations.

Common protocols include:

- HTTP
- FTP
- DHCP
- NetBIOS
- Kerberos

Analyzing these protocols can reveal:

- User credentials
- File transfers
- Authentication attempts
- Suspicious commands

### HTTPS Decryption

Encrypted HTTPS traffic can be decrypted when the appropriate TLS session keys are available.

This allows analysts to inspect encrypted communications during incident investigations.

### Firewall Rule Generation

Wireshark can generate firewall rules based on suspicious traffic, helping analysts quickly block malicious hosts during incident response.

## Practical Exercise

Investigated multiple packet capture (PCAP) files to identify different attack scenarios.

Activities included:

- Identified network reconnaissance scans.
- Investigated ARP spoofing attacks.
- Analyzed DNS and ICMP tunneling activity.
- Examined FTP and HTTP communications.
- Investigated cleartext credentials.
- Decrypted HTTPS traffic using TLS session keys.
- Generated firewall rules to block malicious network activity.

## Skills Acquired

- Identified common network scanning techniques.
- Investigated ARP spoofing attacks.
- Detected DNS and ICMP tunneling.
- Analyzed cleartext network protocols.
- Investigated encrypted network traffic.
- Generated firewall rules from packet captures.

## Interview Question

**Q: Why is DNS tunneling considered a security risk?**

**A:** DNS tunneling allows attackers to hide command-and-control (C2) communications or exfiltrate sensitive data inside DNS traffic, making malicious activity more difficult to detect using traditional security controls.

## Key Takeaway

Network traffic analysis enables SOC analysts to identify attacker behavior by examining communication patterns rather than relying only on security alerts. Wireshark provides powerful capabilities for detecting reconnaissance, lateral movement, tunneling, credential exposure, and other suspicious network activities during incident investigations.
# Room 05: NetworkMiner

## Objective

Learn how to use NetworkMiner to perform network forensic investigations by analyzing packet capture (PCAP) files, extracting network artifacts, and identifying valuable evidence during security incidents.

## Key Learning Points

### What is NetworkMiner?

NetworkMiner is an open-source Network Forensic Analysis Tool (NFAT) used to analyze captured network traffic. Unlike Wireshark, which focuses on detailed packet inspection, NetworkMiner automatically extracts useful artifacts from PCAP files, making investigations faster and more efficient.

### Network Forensics

Network forensics is the process of investigating captured network traffic to identify malicious activity, reconstruct events, and collect digital evidence during incident response.

It helps analysts answer key investigation questions such as:

- Who initiated the communication?
- What data was transferred?
- Where was the traffic sent?
- When did the activity occur?
- Why did the incident happen?

### NetworkMiner Features

NetworkMiner automatically organizes extracted information into several categories.

#### Hosts

Displays information about communicating devices, including:

- IP addresses
- MAC addresses
- Operating systems
- Open ports
- Network sessions

#### Sessions

Provides a summary of communication sessions between network hosts.

#### Files & Images

Automatically reconstructs files and images transferred across the network without requiring manual extraction.

#### Credentials

Extracts cleartext credentials and authentication hashes from supported protocols, helping analysts identify exposed user accounts.

#### Parameters & Messages

Displays web parameters, email communications, and other useful application-level information collected from network traffic.

### Passive OS Fingerprinting

NetworkMiner identifies operating systems by analyzing network traffic rather than actively scanning the target. This allows analysts to gather system information without generating additional network activity.

### NetworkMiner vs. Wireshark

Although both tools analyze packet captures, they serve different purposes.

**Wireshark**

- Deep packet inspection
- Manual protocol analysis
- Advanced filtering
- Detailed traffic investigations

**NetworkMiner**

- Automated artifact extraction
- Host profiling
- File reconstruction
- Credential extraction
- Fast forensic triage

## Practical Exercise

Performed forensic analysis on multiple packet capture (PCAP) files using NetworkMiner.

Activities included:

- Identified communicating hosts and network sessions.
- Reviewed host operating system information.
- Extracted transferred files and images.
- Investigated cleartext credentials and authentication hashes.
- Examined web parameters and application data.
- Reconstructed email communications.
- Reviewed network anomalies to support forensic investigations.

## Skills Acquired

- Performed network forensic investigations.
- Identified hosts and communication sessions.
- Extracted files and network artifacts from PCAPs.
- Investigated exposed credentials.
- Used passive OS fingerprinting.
- Performed rapid forensic triage using NetworkMiner.

## Interview Question

**Q: What is the difference between Wireshark and NetworkMiner?**

**A:** Wireshark is designed for detailed packet inspection and protocol analysis, while NetworkMiner focuses on network forensics by automatically extracting artifacts such as files, credentials, host information, and network sessions from packet captures.

## Key Takeaway

NetworkMiner complements Wireshark by providing automated forensic analysis of packet captures. Instead of manually inspecting every packet, analysts can quickly identify hosts, reconstruct transferred files, recover credentials, and collect valuable evidence that supports incident response and forensic investigations.
# Section 06 Summary

## Topics Covered

- Network Traffic Analysis (NTA)
- TCP/IP Model
- Network Traffic Collection
- North-South & East-West Traffic
- Wireshark Fundamentals
- Packet Analysis
- Display Filters
- Packet Statistics
- Protocol Analysis
- Network Reconnaissance Detection
- ARP Spoofing
- DNS Tunneling
- ICMP Tunneling
- HTTPS Decryption
- Firewall Rule Generation
- Network Forensics
- NetworkMiner
- Passive OS Fingerprinting
- Artifact Extraction

---

## Key Terms Learned

- NTA (Network Traffic Analysis)
- PCAP
- TCP/IP
- DPI (Deep Packet Inspection)
- SPAN
- Network TAP
- NetFlow
- IPFIX
- North-South Traffic
- East-West Traffic
- Wireshark
- Display Filter
- Capture Filter
- TCP Stream
- ARP Spoofing
- DNS Tunneling
- ICMP Tunneling
- MITM (Man-in-the-Middle)
- NFAT (Network Forensic Analysis Tool)
- NetworkMiner
- Passive OS Fingerprinting

---

## Skills Acquired

- Understood the fundamentals of Network Traffic Analysis.
- Analyzed packet captures (PCAPs) using Wireshark.
- Applied display filters to investigate network traffic.
- Examined network conversations and protocol statistics.
- Investigated HTTP, DNS, TCP, and UDP communications.
- Identified network reconnaissance and scanning activities.
- Detected ARP spoofing and Man-in-the-Middle attacks.
- Investigated DNS and ICMP tunneling techniques.
- Analyzed cleartext network protocols and encrypted traffic.
- Generated firewall rules from suspicious network activity.
- Performed network forensic investigations using NetworkMiner.
- Extracted files, credentials, and network artifacts from packet captures.
- Used passive OS fingerprinting to identify communicating hosts.

---

## Personal Reflection

This section significantly improved my understanding of network traffic analysis and the role it plays in a Security Operations Center. I learned how SOC analysts investigate packet captures using Wireshark, identify suspicious communication patterns, detect common network attacks, and perform network forensic investigations with NetworkMiner. The practical labs gave me hands-on experience analyzing real network traffic, extracting valuable evidence, and applying investigation techniques commonly used during incident response. These skills have strengthened my ability to analyze network-based threats and will be valuable for future SOC investigations.


