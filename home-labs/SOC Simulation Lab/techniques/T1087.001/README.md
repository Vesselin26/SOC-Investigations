
# T1087.001 – Local Account Discovery

## Overview

This scenario simulates adversary reconnaissance behavior aligned with **MITRE ATT&CK T1087.001 – Local Account Discovery** using Atomic Red Team in a home SOC lab environment.

The objective was to generate account enumeration activity on a Windows 10 endpoint and validate detection visibility through Sysmon logs in Splunk.

---

## Technique Information

- **Technique ID:** T1087  
- **Sub-technique:** T1087.001  
- **Name:** Local Account Discovery  
- **Tactic:** Discovery

---

## Lab Systems

- **Windows 10 Endpoint:** `192.168.99.120`
- **Splunk Server:** `192.168.99.102`
- **pfSense Firewall:** `192.168.99.100`

---

## Attack Simulation

Atomic Red Team was used to emulate local account enumeration activity.

```powershell
Invoke-AtomicTest T1087.001
```

## Executed sub-tests generated multiple account discovery commands including:

```net user```
```Get-LocalUser```
```Get-LocalGroup```
```Get-LocalGroupMember```
```dir C:\Users```

## Telemetry Source

Sysmon Event ID 1 – Process Creation

## Splunk query
```
index="vescyber-detec" EventCode=1
(CommandLine="*net user*" 
| table _time host User Image CommandLine
```

## Detection Evidence

Search Results

Event Details

Key Observations

## Observed account discovery behavior included:

Enumeration of local users
Enumeration of local groups
Queries for group membership
Directory listing of user profiles
Multiple discovery commands chained in PowerShell

A notable event showed:

```powershell.exe```

```net user```

```Get-LocalUser```

```Get-LocalGroupMember```

This type of activity may indicate attacker reconnaissance prior to privilege escalation or lateral movement.

## MITRE ATT&CK Mapping

T1087 – Account Discovery

T1087.001 – Local Account Discovery
