
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


## Investigation Evidence

### 1. Account Logon Activity
Reviewed successful account logons on KCD-Web to identify suspicious authentication activity, source IP addresses, and newly created accounts. The analysis confirmed logons from multiple external sources and activity involving the unauthorized user `ftp$`.

![Account Logon Activity](images/3%20account%20logons%20%2B%20logon%20type%20%2B%20src%20ip.png)

---

### 2. Malicious Process Execution
Investigated process creation events related to `client.exe` to understand how the binary was launched and which parent processes were involved. Findings showed execution through batch scripts and suspicious child process behavior.

![Malicious Process Execution](images/4%20-%20we%20investigate%20process%20creatations%20from%20the%20alerted%20binary.png)

---

### 3. Batch Script Activity (`go.bat`)
Analyzed commands executed by `go.bat` to identify attacker actions after compromise. Evidence included firewall rule creation, service manipulation, process execution, and deployment of additional binaries.

![Batch Script Activity](images/5-%20investigating%20what%20go.bat%20file%20does.png)

---

### 4. Network Communications Analysis
Reviewed outbound network connections to identify external infrastructure contacted by malicious processes. Repeated communications to suspicious destinations supported active command-and-control behavior.

![Network Communications Analysis](images/6%20-%20network%20analysis%20to%20see%20which%20ips%20are%20communicated%20to.png)

---

### 5. Credential Dumping Evidence
Investigated access attempts to `lsass.exe` and identified suspicious processes requesting high-privilege memory access. This behavior is strongly associated with credential dumping activity.

![Credential Dumping Evidence](images/7%20-%20lsass%20communicating%20processes.png)

---

### 6. Archive Tool Check
Performed a search for archive utilities (`rar`, `7z`, `zip`, `tar`) to assess possible data staging or exfiltration preparation. No evidence of archive tool usage was identified.

![Archive Tool Check](images/8%20no%20archiving%20tools.png)

---

### 7. Domain Controller Authentication Review
Reviewed authentication events on the domain controller to determine whether administrator credentials were used across the environment. No suspicious brute-force behavior was identified.

![Domain Controller Authentication Review](images/9%20-%20no%20brute%20force%20on%20the%20addc%20from%20administrator.png)

---

### 8. Lateral Movement Check
Analyzed successful logons and host-to-host activity to identify possible lateral movement. No clear evidence of additional host compromise was observed during the investigation window.

![Lateral Movement Check](images/10%20-%20No%20evidence%20of%20lateral%20movement.png)

---

### 9. Failed Logon Attempts
Reviewed failed authentication attempts to identify password spraying, brute force attempts, or account targeting. Multiple invalid username/password attempts were observed against common usernames.

![Failed Logon Attempts](images/failed%20logon%20users.png)

---

### 10. Successful Logon Summary
Summarized successful authentication events by user and logon type to establish a baseline and compare normal versus suspicious activity.

![Successful Logon Summary](images/successful%20account%20logons.png)
