# T1547.001 – Registry Run Keys / Startup Folder

## Overview

This scenario simulates adversary persistence behavior aligned with **MITRE ATT&CK T1547.001 – Registry Run Keys / Startup Folder** using Atomic Red Team in a home SOC lab environment.

The objective was to generate registry-based persistence activity on a Windows 10 endpoint and validate detection visibility through Sysmon logs in Splunk.

---

## Technique Information

- **Technique ID:** T1547  
- **Sub-technique:** T1547.001  
- **Name:** Registry Run Keys / Startup Folder  
- **Tactic:** Persistence

---

## Lab Systems

- **Windows 10 Endpoint:** `192.168.99.120`
- **Splunk Server:** `192.168.99.102`
- **pfSense Firewall:** `192.168.99.100`

---

## Attack Simulation

Atomic Red Team was used to emulate persistence techniques through registry and startup mechanisms.

```powershell
Invoke-AtomicTest T1547.001
```
Executed sub-tests included:

Run Keys
RunOnce Keys
Startup Folder persistence
Winlogon Shell modifications
Context Menu persistence
Registry execution path changes

## Telemetry Source

Sysmon Event ID 12 – Registry Object Create/Delete

Sysmon Event ID 13 – Registry Value Set

Sysmon Event ID 14 – Registry Rename

## Splunk Detection Query
```
index="vescyber-detection-lab"
(EventCode=12 OR EventCode=13 OR EventCode=14)
(TargetObject="*Run*" OR TargetObject="*Winlogon*" OR TargetObject="*shell\\open\\command*")
| table _time host user EventID TargetObject Details
```

## Detection Evidence

Search Results

Event Details

## Key Observations

Observed persistence behavior included:

```Registry value modifications```

```Shell command path changes```

```Run key persistence attempts```

```Startup execution mechanisms```

```Multiple registry-based autorun methods```

A notable event showed:
```
TargetObject: ...shell\open\command\(Default)
action: modified
```
This type of behavior may indicate an attempt to achieve persistence or hijack execution flow through registry keys.

## MITRE ATT&CK Mapping

T1547 – Boot or Logon Autostart Execution

T1547.001 – Registry Run Keys / Startup Folder
