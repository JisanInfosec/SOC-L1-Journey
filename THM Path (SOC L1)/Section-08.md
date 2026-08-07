# Section 08: Network Defense & Intrusion Detection

**Platform:** TryHackMe  
**Path:** SOC Level 1  
**Status:** Completed  
**Rooms Completed:** 6/6  

---

# Overview

This section focused on defensive network security strategies, network discovery detection, data exfiltration analysis, Man-in-the-Middle (MITM) attack chain investigation, and Intrusion Detection/Prevention Systems (IDS/IPS) using Snort. Through command-line log analysis, Wireshark packet inspection, Splunk SPL queries, and custom Snort rule development, I gained practical experience in identifying reconnaissance, protocol abuse, covert tunneling, session hijacking, and signature-based detection.

---

## Rooms Completed

1. Network Security Essentials
2. Network Discovery Detection
3. Data Exfiltration Detection
4. Man-in-the-Middle Detection
5. IDS Fundamentals
6. Snort

---

# Room 01: Network Security Essentials

## Objective

Understand defensive network security principles, enterprise asset roles, the distinction between host-centric and network-centric logs, and perimeter threat detection across Firewalls, WAFs, VPNs, and IDSs.

## Key Learning Points

### Enterprise Network Assets
- **Endpoints:** Primary initial entrance vector for attackers via phishing or drive-by downloads.
- **DMZ & Application Servers:** Buffer zone hosting public-facing services (Web, VPN) to prevent direct external access to internal systems.
- **Active Directory (AD):** Central identity and access backbone; top target for privilege escalation and lateral movement.
- **Firewalls & Gateways:** Primary perimeter enforcement points for network traffic filtering and access policy enforcement.

### Network Log Sources Matrix

| Log Type | Device Source | Key Data Captured | Defensive Focus |
| :--- | :--- | :--- | :--- |
| **Firewall** | Edge Firewalls | Source/Dest IP, Ports, Action (ALLOW/BLOCK) | Port scans & unauthorized connection attempts |
| **WAF** | Web Application Firewalls | HTTP/HTTPS Requests, Payloads, Rule IDs | Web attacks (SQLi, XSS, Directory Traversal) |
| **IDS/IPS** | Snort / Suricata | Signatures, Anomaly Alerts, Severity | Exploitation, C2 beacons, Lateral movement |
| **VPN** | VPN Gateways | Usernames, Timestamps, Assigned Internal IPs | Brute-force & unauthorized remote access |

### Traffic Pattern Recognition
- **Scanning:** High-volume connection attempts from one source to many destination ports or hosts.
- **Brute-Forcing:** High-frequency authentication failures targeting specific services or user accounts.
- **C2 Beaconing:** Outbound traffic sent at regular, periodic time intervals to external IP addresses.
- **Data Exfiltration:** Abnormally large or frequent outbound file transfers/HTTP POST requests.

## Practical Exercise

Investigated a full perimeter breach across three log sources (`firewall.log`, `ids_alerts.log`, `vpn_auth.log`) using Linux CLI parsing tools (`grep`, `cut`, `sort`, `uniq`):

- **Reconnaissance:** Parsed blocked firewall entries to isolate scanning external source IPs.
- **Credential Access:** Filtered `vpn_auth.log` for `FAILED_AUTH` events to identify brute-force attacks and trace the attacker's assigned internal IP (`10.8.0.23`).
- **Lateral Movement:** Correlated traffic originating from `10.8.0.23` across firewall and IDS logs to detect internal SSH (22), SMB (445), and RDP (3389) probing.
- **C2 & Exfiltration:** Isolated `ET TROJAN Possible C2 Beaconing` alerts on port 4444 and correlated them with large HTTP POST requests on port 80/8080 to confirm data exfiltration.

## Skills Acquired

- Differentiated between host-centric and network-centric log sources.
- Analyzed firewall, WAF, IDS, and VPN telemetry.
- Correlated remote access logs with internal network activity during an incident.
- Used Linux CLI utilities to parse perimeter security logs.

## Interview Question

**Q: What is the primary difference between host-centric and network-centric logs?**

**A:** Host-centric logs (e.g., Windows Event Logs, Syslog, EDR) capture local execution details on a specific endpoint, such as process creation, registry edits, and local logons. Network-centric logs (e.g., firewall, WAF, IDS) record transit metadata between devices, showing source/destination IPs, ports, protocols, and bandwidth usage.

## Key Takeaway

Correlating perimeter log sources (VPN, Firewall) with internal detection systems (IDS) allows analysts to track an attacker's progression from initial access through lateral movement and exfiltration.

---

# Room 02: Network Discovery Detection

## Objective

Understand network discovery fundamentals from a SOC perspective, distinguish between external and internal reconnaissance, categorize scanning geometry, and detect discovery campaigns using Linux CLI tools and SIEM platforms (Kibana/Elasticsearch).

## Key Learning Points

### External vs. Internal Discovery
- **External Scanning:** Targets public-facing perimeter assets to identify initial access vectors before network access is gained. Lower immediate operational severity; mitigated via edge firewall blocks.
- **Internal Scanning:** Initiated from private IP space. High severity indicating an active breach where an adversary is mapping internal subnet assets for lateral movement. Requires immediate incident response.

### Scanning Typologies & Typology Matrix

| Scan Type | Source IP | Destination IP(s) | Destination Port(s) | Key Indicator / State |
| :--- | :--- | :--- | :--- | :--- |
| **Ping Sweep** | Single | Entire Subnet | N/A (ICMP) | High volume of ICMP Type 8 (Echo Request) |
| **Horizontal** | Single | Subnet Range (/24) | Single Port (e.g., 445) | One-to-many IP connection attempts on a uniform port |
| **Vertical** | Single | Single Target Host | Multiple Ports (1000+) | Single-to-single IP connections across diverse ports |
| **TCP SYN** | Single | Target IP(s) | Variable | High frequency of `conn_state: S0` (SYN sent, no response) |

## Practical Exercise

Analyzed Zeek connection logs (`log-session-0.csv`, `log-session-1.csv`, `log-session-2.csv`) using Linux command-line parsing tools and Kibana:

- **Scan Locality Analysis:** Parsed CSV logs using `cut` and `uniq -c` to identify `203.0.113.25` as the primary external scanner and `192.168.230.127` as the internal compromised host performing scanning.
- **Scan Geometry Unmasking:** Used `awk`, `sort`, and `uniq` to expose a horizontal subnet sweep and a vertical scan from `192.168.230.127` probing 1,001 distinct ports on target `192.168.230.145`.
- **SIEM Connection State Analysis (Kibana):** Evaluated connection states using KQL. Confirmed an ICMP ping sweep and identified TCP SYN half-open scanning based on the dominant `zeek.conn.conn_state: S0` flag.

## Skills Acquired

- Identified external reconnaissance vs. internal discovery attempts.
- Differentiated horizontal subnet scans from vertical host footprinting scans.
- Analyzed Zeek connection states (`S0`, `REJ`, `SF`) to identify scan types.
- Executed KQL queries in Kibana to detect network discovery campaigns.

## Interview Question

**Q: Why is an internal network scan considered significantly higher severity than an external perimeter scan?**

**A:** An external scan represents internet noise or initial reconnaissance against perimeter defenses. An internal scan originates from inside the private network, indicating that an attacker has already gained a foothold and is actively mapping internal assets for lateral movement.

## Key Takeaway

Analyzing connection states (`S0`) and scan geometry (horizontal vs. vertical) allows analysts to distinguish automated scanning noise from targeted internal discovery.

---

# Room 03: Data Exfiltration Detection

## Objective

Identify data exfiltration vectors across common network protocols (DNS, FTP, HTTP, ICMP), detect covert tunneling channels, and construct Wireshark display filters and Splunk SPL detection queries.

## Key Learning Points

### Protocol Abuse & Detection Matrix

| Protocol | Attack Vectors | Primary Network Indicators | Wireshark Filter / Splunk SPL |
| :--- | :--- | :--- | :--- |
| **DNS** | Subdomain tunneling, TXT record exfiltration | High query volume, long subdomains (>60 chars), high entropy, frequent NXDOMAIN | `dns && frame.len > 70`<br>`\| where len(query) > 30` |
| **FTP** | Anonymous uploads, PASV data transfers | Plaintext `STOR` commands, large data transfers on ephemeral ports | `ftp contains "STOR"`<br>`ftp.request.command == "USER"` |
| **HTTP** | Large POST payloads, Base64 strings | High outbound byte counts, unusual User-Agents, multipart POST uploads | `http.request.method == "POST" and frame.len > 750`<br>`method=POST bytes_sent > 600` |
| **ICMP** | Payload tunneling via Echo Request (Type 8) | Abnormally large payload size (>64 bytes), steady beaconing periodicity | `icmp.type == 8 and frame.len > 100` |

### Exfiltration Staging & Multi-Source Correlation
Adversaries aggregate, compress, and encode (Base64, Hex) sensitive data before transport to blend in with legitimate traffic. Effective detection requires correlating network volume spikes and payload anomalies with host-level EDR/Sysmon execution events.

## Practical Exercise

Investigated exfiltration campaigns across packet captures (`.pcap`) and Splunk log index datasets:

- **DNS Tunneling Analysis:** Used Wireshark filters (`dns && frame.len > 70`) to isolate oversized subdomains and executed Splunk SPL queries (`where len(query) > 30`) to identify top querying internal hosts.
- **FTP Exfiltration Triage:** Inspected plaintext FTP control streams for `STOR` commands, identified unauthenticated guest sessions, and extracted transferred file artifacts via TCP stream following.
- **HTTP POST Payload Inspection:** Isolated outbound POST requests in Splunk (`bytes_sent > 600`) to flag high transfer volumes and reconstructed HTTP streams in Wireshark to locate exfiltrated data.
- **ICMP Tunneling Extraction:** Filtered network captures for ICMP Type 8 Echo Request frames exceeding standard ping sizes (`icmp.type == 8 and frame.len > 100`) to extract embedded Base64/Hex payloads.

## Skills Acquired

- Detected DNS, FTP, HTTP, and ICMP data exfiltration techniques.
- Built Wireshark display filters to identify protocol abuse and covert channels.
- Written Splunk SPL queries to isolate high-volume outbound network transfers.
- Reconstructed network payloads to inspect exfiltrated file contents.

## Interview Question

**Q: How do you detect DNS tunneling during a threat investigation?**

**A:** Look for a high volume of DNS queries sent to a single authoritative domain containing unusually long, high-entropy subdomains (>50–60 characters), frequent `TXT` or `NULL` record requests, and a high frequency of `NXDOMAIN` responses.

## Key Takeaway

Monitored diagnostic protocols (ICMP) and core infrastructure services (DNS) for payload size anomalies and high query entropy to detect hidden exfiltration channels.

---

# Room 04: Man-in-the-Middle Detection

## Objective

Understand multi-stage Man-in-the-Middle (MITM) attack chains on local area networks, examine ARP Cache Poisoning, DNS Spoofing, and SSL Stripping mechanics, and detect session hijacking using Wireshark and SIEM log correlation.

## Key Learning Points

### MITM Attack Stages & Detection Matrix

| Attack Phase | Technique | Underlying Mechanism | Primary Wireshark Filter |
| :--- | :--- | :--- | :--- |
| **Interception** | ARP Spoofing | Floods unsolicited `is-at` replies claiming the gateway IP address | `arp.duplicate-address-detected \|\| arp.duplicate-address-frame` |
| **Redirection** | DNS Spoofing | Sends forged DNS replies mapping target domains to the attacker's IP | `dns.flags.response == 1 && ip.src != <resolver_ip>` |
| **Downgrade** | SSL Stripping | Intercepts HTTPS requests and relays unencrypted HTTP sessions | `http.request.method == "POST"` |

### Attack Chain Mechanics
Attackers use ARP spoofing for physical traffic interception, DNS spoofing for target redirection, and SSL stripping for session downgrading. This sequence converts secure HTTPS interactions into cleartext HTTP streams, allowing attackers to harvest credentials in transit.

## Practical Exercise

Analyzed a multi-stage corporate network breach capture (`network-traffic.pcap`) and corresponding log files:

- **ARP Spoofing Analysis:** Isolated ARP traffic using `arp.duplicate-address-detected` to locate duplicate MAC mappings claiming gateway `192.168.10.1`. Uncovered 14 spoofed ARP packets originating from attacker MAC `02:fe:fe:fe:55:55`.
- **Unmasking DNS Spoofing:** Analyzed DNS responses for internal domain `corp-login.acme-corp.local`. Used `dns.flags.response == 1 && ip.src != 8.8.8.8` to identify 2 forged responses sent by rogue server `192.168.10.55`.
- **Detecting SSL Stripping & Credential Theft:** Tracked traffic flows post-redirection to confirm the downgrade to unencrypted HTTP. Filtered HTTP requests between the victim and attacker IP (`ip.src == 192.168.10.10 && ip.dst == 192.168.10.55`) to capture a cleartext HTTP POST request containing plaintext user credentials (`Secret123!`).

## Skills Acquired

- Identified ARP spoofing and duplicate IP-to-MAC associations.
- Uncovered rogue DNS server responses and domain redirection.
- Detected SSL stripping and HTTPS-to-HTTP downgrade attacks.
- Extracted cleartext credentials intercepted during MITM attacks.

## Interview Question

**Q: What network indicators point to an active ARP spoofing attack?**

**A:** Key indicators include a high volume of unsolicited (gratuitous) ARP replies, multiple distinct MAC addresses claiming ownership of a single IP address (especially the default gateway), and sudden spikes in ARP packet frequency across the subnet.

## Key Takeaway

MITM attacks rely on abusing trust at the local network layer; monitoring duplicate ARP claims and unrequested protocol downgrades helps detect interception before credentials are stolen.

---

# Room 05: IDS Fundamentals

## Objective

Understand Intrusion Detection System (IDS) core principles, contrast Host-Based (HIDS) vs. Network-Based (NIDS) architectures, compare signature vs. anomaly detection modes, and configure custom Snort rules for PCAP investigation.

## Key Learning Points

### IDS Deployment & Detection Matrix

| Concept | Type | Scope / Mechanism | Key Advantage | Primary Limitation |
| :--- | :--- | :--- | :--- | :--- |
| **Deployment** | **HIDS** | Host-level execution & log monitoring | Deep host forensic insight | High management overhead |
| | **NIDS** | Network-wide traffic inspection | Centralized network visibility | Cannot inspect encrypted payloads |
| **Detection** | **Signature** | Pattern matching against a database | High accuracy for known attacks | Blind to zero-day exploits |
| | **Anomaly** | Baseline behavioral deviation | Detects zero-day / unknown threats | Prone to false positive alerts |
| | **Hybrid** | Signature + Anomaly fusion | Balanced coverage across threat spectrum | Higher processing overhead |

### Snort Rule Structure & Anatomy
Snort rules follow a structured syntax divided into a Rule Header and Rule Options:

alert icmp any any -> 127.0.0.1 any (msg:"Loopback Ping Detected"; sid:10003; rev:1;)


* **Header:** `[action] [protocol] [src_ip] [src_port] -> [dst_ip] [dst_port]`
* **Options:** `msg` (Alert Description), `sid` (Unique Signature ID), `rev` (Rule Revision Number).
* **Configuration:** Main configuration file located at `/etc/snort/snort.conf`; custom user rules are placed in `/etc/snort/rules/local.rules`.

## Practical Exercise

Configured Snort rule sets and executed historical packet analysis on `Intro_to_IDS.pcap`:

* **Custom Rule Testing:** Added a loopback ICMP detection rule to `/etc/snort/rules/local.rules` and executed Snort in real-time detection mode (`snort -q -l /var/log/snort -i lo -A console -c /etc/snort/snort.conf`).
* **Historical PCAP Analysis:** Ran the Snort engine against recorded traffic (`snort -q -r /etc/snort/Intro_to_IDS.pcap -c /etc/snort/snort.conf`).
* **Forensic Alert Investigation:** Filtered Snort execution output for SSH alerts (`:22`) to identify attacker IP `10.11.90.211` attempting unauthorized connections, and verified signature rule ID (`sid: 1000002`).

## Skills Acquired

* Differentiated between HIDS vs. NIDS and Signature vs. Anomaly detection.
* Structured Snort rule headers and option metadata fields.
* Executed Snort against live network interfaces and recorded PCAP files.
* Analyzed generated Snort alert logs to identify malicious source IPs.

## Interview Question

**Q: What is the main difference between an IDS and an IPS?**

**A:** An IDS (Intrusion Detection System) operates passively, monitoring traffic via a TAP or SPAN port to generate alerts without interrupting network flow. An IPS (Intrusion Prevention System) is deployed inline, allowing it to actively drop malicious packets or reset connections in real time when a rule matches.

## Key Takeaway

IDS engines provide automated signature matching across unencrypted traffic; writing clean, well-scoped rules minimizes false positives while ensuring reliable alert generation.

---

# Room 06: Snort

## Objective

Master Snort operational modes (Sniffer, Packet Logger, NIDS/NIPS, PCAP Investigation), validate rule configuration syntax, configure network variables, and write custom rules to inspect application payloads and flags.

## Key Learning Points

### Snort Operational Command-Line Flags

| Flag | Purpose | Operational Context |
| --- | --- | --- |
| `-v` | Verbose mode | Displays TCP/IP header details in the console |
| `-d` | Display payload | Dumps application-layer payload data |
| `-e` | Link-layer headers | Displays MAC addresses and Data Link headers |
| `-X` | Full payload dump | Displays entire packet contents in HEX and ASCII |
| `-l` | Log directory | Specifies the destination folder for log output |
| `-K ASCII` | ASCII logging format | Logs traffic into IP-categorized plain-text directories |
| `-r` | Read PCAP / Binary log | Processes pre-recorded log or PCAP files |
| `-c` | Configuration file | Specifies active ruleset configuration path |
| `-T` | Self-test configuration | Validates rule syntax without starting live capture |

### Advanced Rule Anatomy & Options

Custom rules in `/etc/snort/rules/local.rules` evaluate traffic headers and options:

```text
alert tcp $EXTERNAL_NET any ->$HOME_NET 80 (msg:"HTTP GET Request Detected"; flags:PA; content:"GET"; sid:1000001; rev:1;)

```

* **`flags`:** Matches specific TCP flags (`S` = SYN, `PA` = PSH-ACK).
* **`content`:** Performs payload string or hex matching (`content:"GET"` or `content:"|90 90 90|"`).
* **`sameip`:** Evaluates self-referential traffic where source IP matches destination IP.

## Practical Exercise

Executed Snort CLI flags and authored custom rules to analyze sample traffic datasets:

* **Configuration Validation:** Verified system configurations and rule sets using `snort -c /etc/snort/snort.conf -T`.
* **Binary Log Processing:** Inspected logged traffic using packet limits and protocol filters (`snort -r snort.log.1640048004 -n 10 -X` and `snort -r snort.log.1640048004 tcp and port 80`).
* **PCAP Investigation Mode:** Processed multiple PCAP files simultaneously against default rulesets (`sudo snort -c /etc/snort/snort.conf -q --pcap-list="mx-2.pcap mx-3.pcap" -A console`).
* **Custom Rule Authoring:** Written rules in `/etc/snort/rules/local.rules` to filter ICMP traffic by IP ID (`id:35369;`), detect TCP SYN (`flags:S;`) and Push-ACK (`flags:PA;`) packets, and flag loopback traffic using the `sameip` keyword (`alert ip any any -> any any (sameip; sid:1000004; rev:1;)`).

## Skills Acquired

* Operated Snort across Sniffer, Logger, NIDS, and PCAP modes.
* Tested and validated Snort configuration files using `-T`.
* Written custom Snort rules using payload string matching, TCP flags, and IP ID filters.
* Processed binary log files and PCAPs using CLI flags.

## Interview Question

**Q: How do you validate a Snort configuration file before deploying it to production?**

**A:** Run the Snort executable with the `-T` flag alongside the path to the configuration file (e.g., `snort -c /etc/snort/snort.conf -T`). This executes a self-test that validates syntax, checks rule dependencies, and confirms variable declarations without capturing live traffic.

## Key Takeaway

Understanding Snort modes and rule syntax enables SOC analysts to write precise detection rules, filter out noise, and execute post-incident forensic analysis on packet captures.

---

# 📊 Section 08 Summary

## Topics Covered

* Defensive Network Security Architecture & Asset Roles
* Host-Centric vs. Network-Centric Log Sources
* Perimeter Logs (Firewall, WAF, IDS, VPN)
* External Reconnaissance vs. Internal Network Discovery
* Horizontal Subnet Sweeps vs. Vertical Host Scans
* Zeek Connection State Analysis (`S0`, `REJ`, `SF`)
* Protocol Abuse & Data Exfiltration (DNS, FTP, HTTP, ICMP)
* Wireshark Filters & Splunk SPL for Exfiltration Detection
* Multi-Stage MITM Attack Chains (ARP Spoofing, DNS Spoofing, SSL Stripping)
* Cleartext Credential Interception
* HIDS vs. NIDS Deployment Models
* Signature vs. Anomaly Detection Paradigms
* Snort Architecture & Rule Syntax
* Snort Operational Modes (Sniffer, Logger, NIDS, PCAP Investigation)
* Custom Snort Rule Authoring (Flags, Content, IP ID, SameIP)

---

## Key Terms Learned

* Host-Centric / Network-Centric Logs
* DMZ (Demilitarized Zone)
* C2 Beaconing
* Horizontal / Vertical Scanning
* Zeek Connection State (`S0`)
* DNS Tunneling
* ICMP Payload Exfiltration
* ARP Cache Poisoning
* DNS Spoofing
* SSL Stripping
* HIDS / NIDS
* Signature-Based / Anomaly-Based Detection
* Snort NIDS / NIPS
* `local.rules`
* `sid` / `rev`
* BPF Filters

---

## Skills Acquired

* Parsed perimeter logs using Linux CLI tools to correlate VPN logins with internal lateral movement.
* Differentiated horizontal subnet scans from vertical host scans using Zeek logs and Kibana.
* Detected data exfiltration across DNS, FTP, HTTP, and ICMP protocols using Wireshark and Splunk SPL.
* Uncovered multi-stage MITM attacks involving ARP spoofing, DNS redirection, and SSL stripping.
* Configured and executed Snort across Sniffer, Logger, NIDS, and PCAP investigation modes.
* Authored custom Snort rules using TCP flags, payload content matching, and same-IP detection.

---

## Personal Reflection

This section tied together defensive architecture, protocol investigation, and active detection engineering. Investigating exfiltration attempts and MITM attack chains across Wireshark and Splunk demonstrated how adversaries abuse standard protocols to evade edge controls. Writing custom Snort rules and executing packet investigations reinforced the importance of clear signature design for detecting internal scanning, covert channels, and unauthorized access attempts inside a Security Operations Center.


