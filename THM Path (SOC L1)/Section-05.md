# Section 05: Phishing Analysis & Email Security

**Platform:** TryHackMe  
**Path:** SOC Level 1  
**Status:** Completed  
**Rooms Completed:** 6/6

---

# Overview

This section focused on phishing attacks, email security, and the techniques SOC analysts use to investigate malicious emails. It covered email protocols, email header analysis, phishing analysis tools, email authentication mechanisms, and practical phishing investigations. Through multiple hands-on labs, I learned how to identify phishing attempts, analyze suspicious attachments and links, verify email authenticity, and investigate attacker infrastructure using industry-standard tools.

---

## Rooms Completed

1. Phishing Analysis Fundamentals
2. Phishing Analysis Tools
3. Phishing Prevention
4. The Greenholt Phish
5. Snapped Phish-ing Line

---

# Room 01: Phishing Analysis Fundamentals

## Objective

Understand how phishing attacks work, how email communication functions behind the scenes, and how to analyze email headers and content to identify malicious activity.

## Key Learning Points

### Email Structure

An email address follows the format:

**username@domain.com**

- Username (Mailbox): Identifies the recipient.
- Domain: Identifies the mail server or organization hosting the email account.

### Email Protocols

#### SMTP (Simple Mail Transfer Protocol)

- Used to send emails.
- Secure Port: **465**

#### POP3 (Post Office Protocol)

- Downloads emails to a single device.
- Messages are usually removed from the server after download.
- Secure Port: **995**

#### IMAP (Internet Message Access Protocol)

- Synchronizes emails across multiple devices.
- Messages remain stored on the mail server.
- Secure Port: **993**

### Email Components

Every email contains two primary sections:

#### Email Header

Contains metadata including:

- From
- Reply-To
- Return-Path
- X-Originating-IP
- Authentication results
- Mail server routing information

#### Email Body

Contains the actual message content in plain text or HTML.

### Email Attachments

Important attachment-related headers include:

- **Content-Type** – Specifies the file type.
- **Content-Disposition** – Indicates that the content is an attachment.
- **Content-Transfer-Encoding** – Shows how the attachment is encoded (commonly Base64).

### Types of Phishing

- Spam / MalSpam
- Phishing
- Spear Phishing
- Whaling
- Smishing
- Vishing

### Defanging

Defanging modifies malicious indicators so they can be shared safely without accidental interaction.

Example:

```
http://malicious-site.com
↓

hxxp[://]malicious-site[.]com
```

## Practical Exercise

Investigated multiple suspicious email samples.

Activities included:

- Viewed raw email source code.
- Examined email headers.
- Compared From and Reply-To addresses.
- Identified sender IP addresses.
- Decoded a Base64-encoded PDF attachment.
- Investigated a phishing email impersonating a trusted organization.
- Extracted and safely defanged malicious URLs.

## Skills Acquired

- Read raw email headers.
- Identified email spoofing indicators.
- Investigated sender information.
- Decoded encoded attachments.
- Safely handled malicious URLs.

## Interview Question

**Q: Why are email headers important during phishing investigations?**

**A:** Email headers contain metadata such as sender information, routing details, authentication results, and originating IP addresses, helping analysts verify whether an email is legitimate or malicious.

## Key Takeaway

Effective phishing analysis begins with examining email headers rather than trusting the visible sender information. Header analysis often reveals spoofing attempts, suspicious routing, and other indicators of malicious activity.

---

# Room 02: Phishing Analysis Tools

## Objective

Learn how SOC analysts use specialized tools to investigate phishing emails, analyze suspicious links and attachments, verify reputations, and safely inspect malware.

## Key Learning Points

### Information Collected During Investigations

Analysts commonly collect:

- Sender email address
- Sender IP address
- Subject line
- Recipient information
- Embedded URLs
- Attachment names
- File hashes

### Email Header Analysis Tools

- Google Message Header Analyzer
- IPinfo
- Cisco Talos Reputation Center

These tools help analyze routing information, identify sender IP locations, and verify domain or IP reputation.

### URL Analysis Tools

- CyberChef
- URLScan.io
- URL2PNG

These tools safely extract and inspect URLs without directly visiting malicious websites.

### Malware Analysis Tools

- VirusTotal
- Cisco Talos File Reputation
- ANY.RUN
- Hybrid Analysis
- Joe Sandbox

These platforms allow analysts to verify file reputation and observe malware behavior inside isolated environments.

### PhishTool

PhishTool is a phishing investigation platform that combines email parsing, threat intelligence lookups, attachment analysis, and case management into a single workflow.

## Practical Exercise

Completed multiple phishing investigations.

Activities included:

- Investigated phishing email headers.
- Extracted malicious URLs.
- Checked IP and domain reputation.
- Reviewed malware sandbox reports.
- Verified file hashes using VirusTotal.
- Investigated malicious Office documents.
- Identified exploitation attempts against Microsoft Office.

## Skills Acquired

- Used phishing investigation tools.
- Verified IP and domain reputation.
- Investigated suspicious attachments.
- Performed file hash lookups.
- Reviewed malware sandbox reports.

## Interview Question

**Q: Why should analysts use sandbox environments when investigating attachments?**

**A:** Sandboxes allow analysts to safely observe the behavior of suspicious files without risking infection of production systems.

## Key Takeaway

Phishing investigations rely on multiple tools working together to validate email authenticity, inspect attachments, analyze URLs, and gather threat intelligence before making investigation decisions.

---

# Room 03: Phishing Prevention

## Objective

Understand how email authentication protocols and technical security controls help prevent phishing and email spoofing.

## Key Learning Points

### Sender Policy Framework (SPF)

SPF verifies whether the sending mail server is authorized to send emails on behalf of a domain.

### DomainKeys Identified Mail (DKIM)

DKIM uses digital signatures to verify that an email has not been modified during transmission.

### Domain-based Message Authentication, Reporting and Conformance (DMARC)

DMARC combines SPF and DKIM to validate sender authenticity and define how failed emails should be handled.

Common policies include:

- None
- Quarantine
- Reject

### S/MIME

S/MIME provides:

- Digital signatures
- Email encryption
- Authentication
- Data integrity
- Non-repudiation

### Technical Defenses

Organizations commonly use:

- Secure Email Gateways (SEG)
- Link rewriting
- Attachment sandboxing
- Email filtering

## Practical Exercise

Analyzed SMTP traffic using Wireshark.

Activities included:

- Filtered SMTP traffic.
- Investigated SMTP response codes.
- Examined email headers.
- Identified suspicious attachments.
- Reviewed encoded attachment data.

## Skills Acquired

- Understood email authentication protocols.
- Investigated SMTP traffic.
- Analyzed email authentication failures.
- Examined email attachments using packet captures.

## Interview Question

**Q: What is the purpose of DMARC?**

**A:** DMARC combines SPF and DKIM to verify sender authenticity and tells receiving mail servers how to handle emails that fail authentication checks.

## Key Takeaway

Email authentication protocols significantly reduce phishing by verifying sender identity and preventing attackers from spoofing trusted domains.

---

# Room 04: The Greenholt Phish

## Objective

Apply phishing investigation techniques to analyze a realistic phishing email and identify indicators of compromise.

## Key Learning Points

- Reviewed a reported phishing email.
- Investigated sender information.
- Examined raw email headers.
- Identified spoofed reply addresses.
- Verified SPF and DMARC results.
- Investigated sender IP ownership.
- Analyzed suspicious attachments.
- Verified attachment reputation using VirusTotal.

## Investigation Workflow

- Reviewed the phishing email.
- Examined raw email headers.
- Verified email authentication.
- Investigated sender IP.
- Analyzed attachment metadata.
- Calculated the attachment hash.
- Checked file reputation.
- Documented investigation findings.

## Practical Exercise

Activities included:

- Investigated sender information.
- Reviewed SPF and DMARC records.
- Examined suspicious attachments.
- Calculated SHA256 hashes.
- Verified file reputation using VirusTotal.

## Skills Acquired

- Investigated phishing emails.
- Performed email authentication analysis.
- Verified sender infrastructure.
- Investigated suspicious attachments.

## Interview Question

**Q: Why is SPF validation important during phishing investigations?**

**A:** SPF helps determine whether the sending server is authorized to send emails for a particular domain, making it useful for identifying spoofed emails.

## Key Takeaway

A complete phishing investigation combines header analysis, authentication checks, attachment analysis, and threat intelligence to determine whether an email is malicious.

---

# Room 05: Snapped Phish-ing Line

## Objective

Perform an end-to-end phishing investigation by analyzing phishing emails, malicious websites, phishing kits, and harvested credentials.

## Key Learning Points

### Email Investigation

- Header analysis
- Recipient identification
- Sender verification

### Phishing Website Analysis

- URL extraction
- Brand impersonation analysis
- Landing page investigation

### Threat Intelligence

- SHA256 hashing
- VirusTotal lookups
- Threat categorization

### Credential Harvesting

- Investigated exposed phishing logs.
- Identified compromised users.
- Reviewed captured credentials.

### Phishing Kit Analysis

- Inspected phishing kit source files.
- Located attacker infrastructure.
- Identified credential collection methods.

## Investigation Workflow

- Reviewed phishing emails.
- Extracted malicious URLs.
- Investigated phishing websites.
- Retrieved exposed phishing kits.
- Calculated file hashes.
- Performed VirusTotal lookups.
- Analyzed captured credentials.
- Investigated attacker infrastructure.
- Documented investigation findings.

## Practical Exercise

Activities included:

- Investigated phishing emails.
- Extracted phishing URLs.
- Analyzed phishing websites.
- Retrieved exposed phishing kits.
- Performed threat intelligence lookups.
- Investigated harvested credentials.
- Examined phishing kit source code.

## Skills Acquired

- Performed complete phishing investigations.
- Investigated phishing infrastructure.
- Analyzed credential harvesting attacks.
- Used threat intelligence during investigations.
- Examined phishing kits.

## Interview Question

**Q: Why is phishing kit analysis valuable during an investigation?**

**A:** Analyzing phishing kits helps identify attacker infrastructure, credential collection methods, and additional indicators that can improve detection and incident response.

## Key Takeaway

Real phishing investigations require analysts to combine email analysis, threat intelligence, web analysis, and infrastructure investigation to understand the full scope of an attack.

---

# Section 05 Summary

## Topics Covered

- Phishing Analysis
- Email Headers
- Email Protocols
- Email Authentication
- SPF
- DKIM
- DMARC
- S/MIME
- Email Spoofing
- URL Analysis
- Malware Sandboxing
- VirusTotal
- Threat Intelligence
- Wireshark
- Phishing Kit Analysis

## Key Terms Learned

- SMTP
- POP3
- IMAP
- Email Header
- Reply-To
- Return-Path
- Base64
- Defanging
- SPF
- DKIM
- DMARC
- S/MIME
- VirusTotal
- Sandbox
- SHA256
- IOC
- Phishing Kit

## Skills Acquired

- Investigated phishing emails.
- Analyzed raw email headers.
- Identified email spoofing attempts.
- Used phishing investigation tools.
- Verified IP, domain, and file reputation.
- Investigated suspicious attachments.
- Performed malware sandbox analysis.
- Investigated SMTP traffic using Wireshark.
- Verified email authentication mechanisms.
- Conducted complete phishing investigations using threat intelligence.

## Personal Reflection

This section strengthened my understanding of phishing investigations and email security. I learned how SOC analysts examine email headers, verify sender authenticity, analyze malicious attachments, investigate phishing infrastructure, and use threat intelligence throughout an investigation. The practical labs provided hands-on experience with real phishing scenarios and demonstrated the structured workflow analysts follow when investigating phishing incidents in a Security Operations Center.
