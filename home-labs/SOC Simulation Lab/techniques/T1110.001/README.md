
# Kali Brute Force Detection – SMB Authentication Attack Against Windows 10

## Overview

This project simulates a brute force authentication attack from a Kali Linux attacker machine against a Windows 10 endpoint over SMB and validates detection visibility in Splunk.

The objective was to generate failed logon activity, identify the source system, and analyze authentication telemetry using Windows Security Event Logs.

---

## Scenario Summary

An internal attacker host running Kali Linux performed multiple SMB authentication attempts against a Windows 10 system using invalid credentials. The resulting failed logons were successfully captured and investigated in Splunk.

---

## Lab Environment

| System | Role | IP Address |
|------|------|-----------|
| Kali Linux | Attacker | 192.168.99.240 |
| Windows 10 | Target Endpoint | 192.168.99.120 |
| Splunk Server | SIEM | 192.168.99.102 |
| pfSense | Firewall / Gateway | 192.168.99.100 |

---

## Attack Flow

### Phase 1 – Reconnaissance

The attacker first identified exposed services on the Windows 10 endpoint.

Discovered open ports:

- 135/tcp – MSRPC  
- 139/tcp – NetBIOS  
- 445/tcp – SMB

This confirmed SMB was available for authentication testing.

### Phase 2 – Brute Force Simulation

A controlled password spray / brute force test was executed from Kali using Hydra against the SMB service.

```bash id="hydra1"
hydra -l testuser -P pass.txt smb://192.168.99.120 -V
```
Multiple invalid password attempts were made against the target account:

Password1!

AdminPass123!

Pin1234

Greatpass234

## Detection Telemetry

### Windows Security Log

Event ID: 4625

Description: An account failed to log on

### Key Fields Observed

Source IP: 192.168.99.240

Target User: testuser

Logon Type: 3 (Network Logon)

Authentication Package: NTLM

Failure Reason: Unknown user name or bad password


## Splunk Detection Query

```index="vescyber-detect" EventCode=4625 192.168.99.240
| table _time src_ip Account_Name Logon_Type Logon_Process host
```

## Detection Evidence

Kali Attack Execution

Splunk Search Results

Failed Logon Event Details

## Investigation Findings

The SIEM confirmed multiple failed network logon attempts originating from the Kali attacker host.

Indicators consistent with brute force activity:

Repeated failed logons in a short period

Same source IP targeting authentication service

Network logon attempts over SMB

Invalid credential failures

## MITRE ATT&CK Mapping

T1110 – Brute Force

T1110.001 – Password Guessing
