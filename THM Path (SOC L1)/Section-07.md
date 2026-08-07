# Section 07: Network Security & Traffic Analysis

**Platform:** TryHackMe  
**Path:** SOC Level 1  
**Status:** Completed  
**Rooms Completed:** 5/5  

---

# Overview

This section focused on network architecture, perimeter defense, deep packet inspection, traffic investigation, and network forensic workflows. It examined how enterprise network components interact, how to analyze both host-centric and network-centric log sources, and how to detect common attack vectors like reconnaissance scans, ARP spoofing, covert tunneling, and exfiltration. Through hands-on labs with Wireshark and NetworkMiner, I gained practical experience in packet dissection, advanced display filtering, TLS decryption, and automated artifact extraction.

---

## Rooms Completed

1. Network Security Essentials
2. Wireshark: The Basics
3. Wireshark: Packet Operations
4. Wireshark: Traffic Analysis
5. NetworkMiner

---

# Room 01: Network Security Essentials

## Objective

Understand the core principles of enterprise network architecture, perimeter defense mechanisms, log source differentiation, and command-line traffic analysis.

## Key Learning Points

### Enterprise Network Architecture
- **Endpoints (Workstations):** Primary initial entry point for attackers via phishing or drive-by downloads.
- **Application Servers & DMZ:** Publicly exposed web, mail, and VPN servers placed in a buffer zone (Demilitarized Zone) to isolate internal network segments.
- **Active Directory (AD):** Enterprise identity backbone; primary target for privilege escalation and lateral movement.
- **Firewalls & Gateways:** Primary perimeter gatekeepers that enforce access policies and record inbound/outbound connections.

### Log Category Differentiation
- **Host-Centric Logs:** Ground-level view of local system activity (process creation, file modifications, registry edits, local authentication).
- **Network-Centric Logs:** High-level view of communication between systems (source/destination IPs, ports, protocols, bandwidth, action taken).

### Traffic Pattern Recognition
- **Scanning:** High-volume connections from a single source targeting multiple destination ports or hosts.
- **Brute-Forcing:** Spikes in failed authentication attempts targeting specific services or user accounts.
- **C2 Beaconing:** Regular, repeating outbound connections to external IP addresses at fixed or jittered time intervals.
- **Data Exfiltration:** Abnormally large file transfers or high-volume HTTP POST requests directed to external destinations.

## Practical Exercise

Investigated an enterprise perimeter breach across three log files (`firewall.log`, `ids_alerts.log`, `vpn_auth.log`) using Linux CLI parsing tools (`grep`, `cut`, `sort`, `uniq`):

- **Reconnaissance Detection:** Parsed blocked firewall entries to identify external source IPs conducting port scans against internal servers.
- **Initial Access & Credential Access:** Analyzed `vpn_auth.log` to track brute-force attempts (`FAILED_AUTH`) targeting service accounts (`svc_backup`) and identified the successful login assigned to internal IP `10.8.0.23`.
- **Lateral Movement Analysis:** Correlated the assigned VPN client IP across firewall and IDS logs to trace lateral movement targeting SSH (port 22), RDP (port 3389), and SMB (port 445).
- **C2 & Exfiltration:** Isolated Trojan beaconing alerts (`ET TROJAN Possible C2 Beaconing`) on port 4444 and identified large HTTP POST upload requests confirming data exfiltration.

## Skills Acquired

- Differentiated between host-centric and network-centric telemetry.
- Parsed perimeter logs using Linux CLI utilities (`grep`, `cut`, `sort`, `uniq`).
- Correlated VPN authentication logs with internal firewall and IDS alerts.
- Identified indicators of C2 beaconing and network data exfiltration.

## Interview Question

**Q: How do you correlate VPN logs with internal network activity during an incident?**

**A:** Match the timestamp of a successful VPN authentication event with the assigned internal IP address from the VPN log, then search firewall, proxy, and IDS logs for that assigned IP to trace all subsequent internal connection attempts and protocol activity during the session window.

## Key Takeaway

Perimeter logs provide the initial timeline for external threats; correlating remote access logs with internal network events allows analysts to map an attacker's path from initial entrance to lateral movement.

---

# Room 02: Wireshark: The Basics

## Objective

Master Wireshark basics, including navigation shortcuts, OSI layer packet dissection, stream reconstruction, and file object extraction.

## Key Learning Points

### Tool Definition
Wireshark is a passive packet analysis tool, **not** an Intrusion Detection System (IDS). It presents captured data for manual inspection and does not generate real-time alerts or block traffic automatically.

### Interface & Metadata Breakdown
- **Packet List Pane:** Summary view displaying packet number, timestamp, source/destination, protocol, length, and info.
- **Packet Details Pane:** Expandable breakdown mapping raw frame structures across OSI layers.
- **Packet Bytes Pane:** Raw data representation displayed in Hexadecimal and ASCII.
- **Capture File Properties (`Statistics -> Capture File Properties`):** Displays packet totals, capture duration, SHA256 hashes, and embedded notes.

### OSI Protocol Dissection
- **Layer 1 (Physical / Frame):** Arrival timestamps, frame number, and total frame length.
- **Layer 2 (Data Link / Ethernet):** Source and Destination MAC addresses.
- **Layer 3 (Network / IP):** IPv4/IPv6 Source and Destination addresses, Time to Live (TTL), and IP flags.
- **Layer 4 (Transport / TCP/UDP):** Source/Destination ports, TCP sequence/acknowledgment numbers, and payload size.
- **Layers 5–7 (Application):** Protocol headers and payloads (HTTP headers, DNS queries, SMB commands).

### Useful Navigation Shortcuts
- **Go to Packet (`Ctrl + G`):** Jump directly to a specific packet frame number.
- **Find Packet (`Ctrl + F`):** Search packet data across List, Details, or Bytes using String, Hex, Display Filters, or Regex.
- **Export Objects (`File -> Export Objects -> HTTP / SMB`):** Extract files transferred over unencrypted streams directly to disk.
- **Expert Info (`Analyze -> Expert Information`):** Summarizes protocol state anomalies across four severity levels: *Chat* (Blue), *Note* (Cyan), *Warning* (Yellow), and *Error* (Red).

## Practical Exercise

Inspected a sample capture file (`Exercise.pcapng`) to practice navigation, protocol dissection, and object recovery:

- Evaluated Capture File Properties to confirm packet totals, capture metadata, and SHA256 hashes.
- Executed string searches (`Ctrl + F`) and jumped to specific frame numbers (`Ctrl + G`) to inspect layer-by-layer packet properties (TTL, payload sizes, XML content).
- Reconstructed full client-server interactions using `Follow -> HTTP Stream` to inspect raw HTML responses.
- Recovered unencrypted files transferred over network streams using `File -> Export Objects -> HTTP`.

## Skills Acquired

- Navigated Wireshark’s multi-pane interface efficiently.
- Dissected raw packet frames across OSI layers.
- Reconstructed segmented TCP streams into application transcripts.
- Extracted transferred file objects from unencrypted captures.

## Interview Question

**Q: What is the difference between Capture Filters and Display Filters?**

**A:** Capture Filters (using BPF syntax) define what traffic is saved to disk *during* packet capture. Display Filters temporarily filter what packets are displayed in the GUI *after* a capture file has been loaded, without altering the raw capture data.

## Key Takeaway

Reconstructing streams and parsing protocol layers in Wireshark converts fragmented packet bytes into clear, human-readable transcripts of network conversations.

---

# Room 03: Wireshark: Packet Operations

## Objective

Apply advanced Wireshark features including traffic summary menus, logical display filter expressions, advanced comparison operators, string conversion functions, and custom configuration profiles.

## Key Learning Points

### Traffic Statistics & Summaries
- **Resolved Addresses (`Statistics -> Resolved Addresses`):** Converts MAC and IP addresses into DNS hostnames and vendor information.
- **Conversations (`Statistics -> Conversations`):** Summarizes bi-directional traffic streams between endpoint pairs across Ethernet, IPv4, IPv6, TCP, and UDP.
- **Endpoints (`Statistics -> Endpoints`):** Lists unique network endpoints and allows sorting by bandwidth usage, GeoIP data, and Autonomous System (AS) Organization.

### Filter Syntax & Advanced Operators

| Operator / Function | Purpose | Example Query |
| :--- | :--- | :--- |
| `==`, `!=`, `>`, `<` | Standard Comparison | `ip.ttl < 10` |
| `&&`, `\|\|`, `!` | Logical AND / OR / NOT | `http && !tcp.srcport == 80` |
| `contains` | Case-sensitive substring search | `http.server contains "Apache"` |
| `matches` | Case-insensitive Regex search | `http.host matches "\.(php\|html)$"` |
| `in` | Matches a field against a set of values | `tcp.port in {80 443 8080}` |
| `string()` | Converts numerical values to strings for Regex | `string(ip.ttl) matches "[24680]$"` |

### Configuration Profiles
Wireshark allows switching profiles (`Edit -> Configuration Profiles`) to instantly apply custom coloring rules, layout preferences, and pre-configured filter buttons (e.g., using *Checksum Control* to highlight bad TCP checksums).

## Practical Exercise

Used Wireshark's built-in statistical menus and advanced display filter expressions to isolate specific network activity:

- Evaluated **Conversations** and **Endpoints** to identify top-talking hosts, active connection streams, and geographic IP mappings.
- Built targeted multi-condition filters to isolate specific HTTP GET requests (`http.request.method == "GET" and tcp.dstport == 80`) and outgoing DNS A-record queries (`dns.qry.type == 1 and dns.flags.response == 0`).
- Applied the `in` operator to evaluate non-sequential ports (`tcp.port in {3333 4444 9999}`) and converted IP TTL values into strings to run Regex searches for even TTL numbers.
- Switched to the *Checksum Control* configuration profile to isolate and count corrupt TCP frames.

## Skills Acquired

- Used statistical tools to establish traffic baselines.
- Built complex display filters using logical operators and Regex functions.
- Used the `in` operator and `string()` conversions to streamline searches.
- Managed custom Wireshark configuration profiles.

## Interview Question

**Q: Why should an analyst review the Conversations menu before writing display filters?**

**A:** The Conversations menu provides a high-level summary of network activity. It quickly highlights top talkers, unexpected port usage, high-volume data transfers, and active protocol pairs before diving into deep packet inspection.

## Key Takeaway

Using statistical summary menus and concise display filter operators reduces analysis time when working with large PCAP files.

---

# Room 04: Wireshark: Traffic Analysis

## Objective

Use Wireshark to investigate attack scenarios, detect network reconnaissance, uncover ARP spoofing, identify covert tunneling channels, audit cleartext protocols, and decrypt TLS sessions.

## Key Learning Points

### Reconnaissance Detection (Nmap Scans)
- **TCP Connect Scan (`-sT`):** Completes the full 3-way handshake (`SYN` $\rightarrow$ `SYN/ACK` $\rightarrow$ `ACK`). Standard scans display larger window sizes ($>1024$ bytes).  
  *Filter:* `tcp.flags.syn==1 and tcp.flags.ack==0 and tcp.window_size > 1024`
- **TCP SYN Scan (`-sS`):** Half-open scan returning a `RST` upon receiving `SYN/ACK`. Typically uses smaller window sizes ($\le 1024$ bytes).  
  *Filter:* `tcp.flags.syn==1 and tcp.flags.ack==0 and tcp.window_size <= 1024`
- **UDP Scan (`-sU`):** Closed ports return ICMP "Destination Unreachable / Port Unreachable" packets.  
  *Filter:* `icmp.type==3 and icmp.code==3`

### ARP Spoofing & MITM Attacks
Attackers send unsolicited or duplicate ARP responses to map their MAC address to a target gateway's IP.
- *Key Filters:* `arp.opcode == 1` (Requests), `arp.opcode == 2` (Responses), `arp.duplicate-address-detected`
- *Analysis Tip:* When analyzing intercepted traffic during ARP spoofing, filter by the attacker's physical MAC address (`eth.addr == MAC`), not an IP address.

### Covert Tunneling Identification
- **ICMP Tunneling:** Ping echo requests normally carry small data payloads ($\le 64$ bytes). Tunneling tools embed encrypted payloads or interactive shell sessions inside oversized ping packets.  
  *Filter:* `data.len > 64 and icmp`
- **DNS Tunneling:** Encodes C2 commands or exfiltrated data into subdomains. Look for long, high-entropy query strings.  
  *Filter:* `dns.qry.name.len > 55 and !mdns`

### Protocol Auditing & HTTPS Decryption
- **Cleartext Credentials:** Built-in tool (`Tools -> Credentials`) automatically extracts plaintext credentials across FTP, HTTP Basic Auth, IMAP, and POP3.
- **TLS Decryption:** Importing a browser key log file (`SSLKEYLOGFILE`) via `Edit -> Preferences -> Protocols -> TLS -> Master-Secret log filename` decrypts TLS sessions into readable HTTP/HTTP2 frames.
- **Automated Firewall Rules:** `Tools -> Firewall ACL Rules` generates copy-paste blocking commands (iptables, Cisco IOS, Windows netsh, ipfw) directly from selected packets.

## Practical Exercise

Analyzed multiple PCAP files containing simulated attack traffic:

- Applied TCP flag and window size expressions to distinguish between TCP Connect and TCP SYN scans.
- Detected ARP spoofing by identifying duplicate address responses, then isolated intercepted HTTP forms by filtering for the attacker's MAC address (`eth.addr`).
- Discovered an SSH session encapsulated inside oversized ICMP payloads (`data.len > 64`).
- Extracted domain endpoints and usernames from DHCP (`dhcp.option.hostname`), NetBIOS (`nbns`), and Kerberos (`kerberos.CNameString`) traffic.
- Decrypted HTTPS PCAPs by importing an SSL key log file into Wireshark's TLS settings.
- Generated firewall ACL rules to block malicious source IPs directly from captured packets.

## Skills Acquired

- Identified Nmap scan signatures (Connect, SYN, UDP) using TCP flags and ICMP responses.
- Detected ARP poisoning and isolated intercepted traffic using MAC address filters.
- Identified ICMP and DNS covert channels used for exfiltration.
- Decrypted HTTPS traffic using TLS master secrets.
- Generated firewall ACL rules directly from network captures.

## Interview Question

**Q: Why should you filter by MAC address instead of IP address when analyzing an ARP poisoning attack?**

**A:** During ARP poisoning, the attacker tricks target hosts into mapping a legitimate IP address (like the default gateway) to the attacker's physical MAC address. Filtering by IP address mixes legitimate and malicious host traffic, whereas filtering by the attacker's MAC address (`eth.addr`) isolates all physical frames intercepted by the attacker.

## Key Takeaway

Analyzing network communications at the packet level reveals malicious behavior—such as half-open scans, spoofed ARP replies, and covert data tunneling—that evades high-level log monitoring.

---

# Room 05: NetworkMiner

## Objective

Understand the role of NetworkMiner as a passive Network Forensic Analysis Tool (NFAT), master its automated artifact extraction features, and execute host-centric forensic triage on packet captures.

## Key Learning Points

### Tool Definition & Role
- **Wireshark:** Packet-centric tool designed for deep packet inspection, custom display filtering, and manual frame analysis.
- **NetworkMiner:** Host-centric Network Forensic Analysis Tool (NFAT) designed for rapid forensic triage. It parses PCAPs to automatically extract transferred files, images, cleartext credentials, messages, and host profiles without requiring manual packet carving.

### Interface Capabilities & Tab Structure
- **Hosts Tab:** Groups traffic by IP address. Displays host details, MAC addresses, open ports, session counts, and passive OS fingerprinting results (via Satori/p0f).
- **Files & Images Tabs:** Automatically reconstructs transferred files (executables, PDFs, HTML) and images from unencrypted streams. Hovering over images reveals source IP and file path metadata.
- **Credentials Tab:** Automatically parses and extracts cleartext passwords (HTTP, FTP, IMAP, POP3) and authentication hashes (NTLM, Kerberos) for offline cracking.
- **Parameters & Messages Tabs:** Extracts web form parameters, email communications (SMTP/POP3/IMAP), and application data.

[ PCAP File Ingestion ]
│
├──> Hosts Tab (OS Fingerprinting, Open Ports, MAC Mappings)
├──> Files & Images Tabs (Reconstructed Transferred Assets)
├──> Credentials Tab (Extracted Plaintext Passwords & NTLM Hashes)
└──> Messages & Parameters Tabs (Emails, Chat Logs & HTTP Form Submissions)

## Practical Exercise

Loaded PCAP files into NetworkMiner to extract forensic artifacts and perform passive host triage:

- Evaluated host profiles, open ports, and operating system fingerprints using the **Hosts** tab.
- Recovered cleartext credentials and NetNTLMv2 authentication hashes directly from the **Credentials** tab.
- Extracted transferred software packages, web documents, and images using the **Files** and **Images** tabs.
- Parsed web form inputs in the **Parameters** tab and reconstructed email reset messages using the **Messages** tab.

## Skills Acquired

- Used NetworkMiner for passive forensic triage and artifact extraction.
- Reconstructed files, images, and email messages from network traffic.
- Extracted cleartext passwords and authentication hashes without writing manual packet filters.
- Identified target operating systems using passive network fingerprinting.

## Interview Question

**Q: What is the primary operational advantage of using NetworkMiner alongside Wireshark during an investigation?**

**A:** NetworkMiner automates artifact extraction. Instead of manually carving files or writing display filters in Wireshark, NetworkMiner automatically parses the PCAP to extract transferred files, images, email messages, cleartext passwords, and host profiles, providing a fast high-level summary during initial triage.

## Key Takeaway

NetworkMiner complements Wireshark by turning raw packet captures into structured forensic evidence, speeding up initial artifact triage during incident response.

---

# 📊 Section 07 Summary

## Topics Covered

- Enterprise Network Components (Endpoints, Application Servers, DMZ, Active Directory)
- Host-Centric vs. Network-Centric Log Sources
- Perimeter Defense & Network Traffic Patterns (Scanning, Brute-Force, C2, Exfiltration)
- Command-Line Log Parsing (`grep`, `cut`, `sort`, `uniq`)
- Wireshark Interface Layout & Capture Properties
- OSI Model Layer Dissection
- Stream Reconstruction (`Follow Stream`) & File Object Extraction
- Wireshark Statistical Menus (Conversations, Endpoints, Protocol Hierarchy)
- Advanced Display Filter Syntax (`contains`, `matches`, `in`, `string()`)
- Reconnaissance Scanning Signatures (TCP Connect, TCP SYN, UDP)
- ARP Spoofing & MITM Analysis
- Covert Channel Detection (ICMP & DNS Tunneling)
- Cleartext Protocol Auditing & TLS Decryption
- Automated Firewall Rule Generation
- Passive Network Forensics with NetworkMiner
- Passive OS Fingerprinting (Satori / p0f)
- Automated File, Image, Credential, and Message Extraction

---

## Key Terms Learned

- Host-Centric Logs
- Network-Centric Logs
- DMZ (Demilitarized Zone)
- C2 Beaconing
- Deep Packet Inspection (DPI)
- Display Filter
- Capture Filter (BPF)
- Stream Reconstruction
- TCP Connect Scan / SYN Scan
- ARP Poisoning / Spoofing
- ICMP / DNS Tunneling
- Cleartext Protocol
- `SSLKEYLOGFILE`
- Network Forensic Analysis Tool (NFAT)
- NetworkMiner
- Passive OS Fingerprinting

---

## Skills Acquired

- Parsed perimeter logs using Linux CLI tools to identify scanning, brute-forcing, and exfiltration.
- Correlated VPN authentication logs with internal firewall and IDS alerts to track lateral movement.
- Dissected raw packet captures across OSI layers using Wireshark.
- Constructed advanced Wireshark display filters using logical operators and Regex functions.
- Detected Nmap scanning profiles, ARP spoofing, and covert ICMP/DNS tunneling channels.
- Decrypted HTTPS traffic using TLS master secret logs.
- Extracted reconstructed files, images, cleartext passwords, and host profiles using NetworkMiner.

---

## Personal Reflection

This section expanded my network analysis skills by moving from basic log correlation to deep packet inspection and network forensics. Learning how to identify attacks across both perimeter logs and raw packet captures provided a much clearer picture of defensive operations. Reconstructing streams in Wireshark, decrypting TLS traffic, and extracting artifacts in NetworkMiner gave me practical tools for analyzing network-based threats and conducting effective incident response investigations.

