
# Home Detection Lab

## Overview
This home lab was built to simulate a small enterprise environment for hands-on cybersecurity practice, threat detection, incident investigation, and SOC automation. The goal is to gain practical experience with real-world tools, logging pipelines, network monitoring, endpoint telemetry, and security workflows.

---

## Lab Architecture

The environment includes:

- **pfSense** – Firewall, routing, and network segmentation  
- **Windows 10** – User endpoint for activity generation. AtomicRed team  attack simulations  
- **Windows Server / Active Directory** – Domain services, authentication, and Windows event logs  
- **Splunk** – Centralized log collection, search, dashboards, detections, and investigations  
- **Zeek & Suricata** – Network visibility, IDS alerts, protocol logs, and traffic analysis  
- **Kali Linux** – Attack simulation, enumeration, and adversary testing  
- **Layer 2 Switch** – Internal connectivity between systems

![Network Diagram](images/network-diagram.png)

---

## Skills Practiced

- Security monitoring and alert triage  
- Log analysis and correlation  
- Windows event investigations  
- Network traffic analysis  
- IDS alert review  
- Threat hunting  
- Phishing and IOC investigations  
- Malware behavior analysis  
- Detection engineering  
- Incident reporting  

---

## Detection & Logging Use Cases

Examples of scenarios tested in the lab:

- Failed login attempts / brute force behavior  
- Suspicious PowerShell execution  
- Lateral movement activity  
- DNS anomalies  
- Network scans  
- Malicious file execution  
- Privilege escalation events  
- Account lockouts  
- Unusual outbound connections  

---
