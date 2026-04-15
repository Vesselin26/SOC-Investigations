
# C2 Beaconing Investigation – Active Command & Control on KCD-Web

## Overview
This project documents the investigation of a critical security incident involving unauthorized access, credential dumping, persistence mechanisms, and active command-and-control (C2) communications on the host **KCD-Web**.

The analysis identified attacker activity consistent with resource hijacking, malware deployment, and post-compromise persistence using valid credentials and malicious tooling.

This project demonstrates practical skills in incident investigation, IOC analysis, attack chain reconstruction, and remediation planning.

---

## Objectives
- Investigate suspicious C2 beaconing activity
- Identify attacker persistence mechanisms
- Detect credential theft behavior
- Extract Indicators of Compromise (IOCs)
- Reconstruct the attack timeline
- Provide remediation recommendations
- Map attacker behavior to MITRE ATT&CK techniques

---

## Incident Summary

A compromised administrator account was used to access **KCD-Web** via SMB logon. The attacker created a new administrative user (`ftp$`), executed malicious scripts, deployed multiple binaries, accessed `lsass.exe` for credential dumping, disabled defenses, and established persistence via services and registry keys.

Malware activity included execution of **Peer2Profit** proxy/cryptomining software and repeated outbound network communications.

---

## Key Findings

- Valid administrator credentials used for access
- New user account created: `ftp$`
- Malicious batch script executed from temp directory
- Kernel drivers loaded using `sc.exe`
- `lsass.exe` accessed by suspicious processes
- Mimikatz and LaZagne artifacts identified
- Malicious binaries downloaded externally
- Firewall rule created to allow malicious access
- Persistence via registry Run key
- Malicious service created: `WindowsDefend`
- Security services disabled
- Repeated outbound communications observed
- Activity consistent with resource hijacking / attacker persistence

---

## Indicators of Compromise (IOCs)

### Users
- administrator
- ftp$

### Domains
- api[.]peer2profit[.]global
- alabajsobaka[.]tk
- node0[.]waspace[.]net
- api[.]iproyal[.]com
- api6[.]my-ip[.]io

### IP Addresses
- 141[.]98[.]80[.]88
- 82[.]147[.]85[.]6
- 172[.]67[.]164[.]91
- 104[.]21[.]58[.]202

### Files
- client.exe
- HRSword.exe
- p64.exe
- usysdiag.exe
- mimikatz.exe
- lazagne.exe

---

## Attack Timeline

| Time (UTC) | Event |
|-----------|-------|
| 2026-04-07 00:34:56 | Initial compromise on DESKTOP-Q1HN49 |
| 2026-04-08 04:41:01 | Admin login to KCD-Web via SMB |
| 08:50:00 | New user `ftp$` created |
| 08:51:11 | HRSword.bat executed |
| 08:52:45 | Additional binaries retrieved externally |
| 08:54:00 | Firewall rule created |
| 08:54:01 | client.exe executed |
| 08:54:07 | Registry persistence added |
| 08:54:26 | WindowsDefend service installed |

---

## MITRE ATT&CK Mapping

| Technique ID | Technique Name | Relevance |
|-------------|----------------|-----------|
| T1078 | Valid Accounts | Attacker used compromised administrator credentials |
| T1136 | Create Account | Unauthorized user `ftp$` created |
| T1059 | Command and Scripting Interpreter | Batch scripts executed |
| T1543 | Create or Modify System Process | Malicious service installed |
| T1547.001 | Registry Run Keys / Startup Folder | Persistence via Run key |
| T1003.001 | LSASS Memory | Credential dumping behavior |
| T1562 | Impair Defenses | Security services disabled |
| T1071 | Application Layer Protocol | Outbound C2 communications |
| T1496 | Resource Hijacking | Peer2Profit cryptomining/proxy abuse |

---

## Risk Assessment

**Severity:** Critical  
**Confidence:** High  
**Type:** Credential Theft / Persistence / Resource Hijacking / Active Intrusion

---

## Recommendations

- Remove unauthorized account `ftp$`
- Reset credentials across affected systems
- Delete malicious files and scripts
- Remove malicious services and registry keys
- Re-enable and update security tools
- Reimage host if possible
- Block malicious domains and IPs
- Review environment for additional compromise
- Investigate persistence across other hosts

---

## Skills Demonstrated

- Incident Response
- Threat Hunting
- IOC Analysis
- Windows Forensics
- Attack Chain Reconstruction
- MITRE ATT&CK Mapping
- Security Reporting
- Root Cause Analysis

---

## Full Report

[View Full Investigation Report](report-c2.docx)
