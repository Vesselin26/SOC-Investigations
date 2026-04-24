
# AI SOC Automation Lab – Overview

## Executive Summary

The **AI SOC Automation Lab** is an end-to-end security operations project designed to simulate a modern automated SOC environment. It combines SIEM monitoring, workflow automation, AI-powered alert enrichment, collaboration tooling, and incident case management into a single integrated pipeline.
The goal of the project is to demonstrate how security teams can reduce manual triage effort, improve response speed, and enhance alert context through intelligent automation.
This lab was built in a virtualized environment using Windows and Linux systems, with real detections, automated workflows, and multi-platform integrations.

---

## Core Architecture

```text
Windows 10 Endpoint
   ↓
Security Logs / Attack Telemetry
   ↓
Splunk Enterprise
   ↓
Detection Rules / Alerts
   ↓
n8n Automation Engine
   ↓
AI Enrichment (OpenAI + Claude)
   ↓
Slack Notifications
   ↓
DFIR-IRIS Case Management
```

---

## Infrastructure Components

| Component                 | Purpose                                                 |
| ------------------------- | ------------------------------------------------------- |
| Windows 10 Target Machine | Attack simulation, telemetry generation, event logging  |
| Splunk Enterprise         | Centralized log ingestion, detection, search, alerting  |
| Ubuntu Server (n8n)       | Workflow automation and alert orchestration             |
| Ubuntu Server (DFIR-IRIS) | Incident ticketing and case management                  |
| Slack                     | Real-time SOC notifications                             |
| OpenAI                    | Alert summarization, triage assistance, recommendations |
| Claude Desktop + MCP      | Natural language threat hunting and Splunk interaction  |

---

## Key Capabilities

### SIEM Monitoring & Detection

Splunk is used as the central analytics platform for:

* Windows event log monitoring
* Security detections
* Search and investigation
* Alert generation
* Incident correlation

### Security Automation

n8n automates response workflows by:

* Receiving Splunk alerts
* Processing event data
* Sending data to AI models
* Triggering Slack notifications
* Creating IRIS tickets

### AI-Powered Alert Enrichment

OpenAI and Claude are integrated to provide:

* Alert summaries
* Severity suggestions
* Analyst-friendly explanations
* Recommended next steps
* Natural language investigation support

### Incident Management

DFIR-IRIS is used to automatically generate cases from alerts, allowing structured incident handling and documentation.

---

## Detection Scenarios

### 1. Security Tool Tampering

Detection of attempts to disable or weaken security controls such as Windows Defender.

### 2. Data Exfiltration Behavior

Detection of suspicious archive creation, large outbound transfers, or unusual data movement patterns.

### 3. Unauthorized Remote Access

Detection of anomalous RDP logins, brute force success patterns, or suspicious remote access behavior.

---

## Skills Demonstrated

* Security Automation & SOAR
* SIEM Detection Engineering
* Threat Hunting
* Incident Response Workflows
* AI Integration for Security Operations
* Alert Triage Optimization
* Windows Log Analysis
* Workflow Orchestration
* Security Platform Integrations
* Case Management Automation
* Troubleshooting & Debugging
* Security Operations Architecture

---

## Why This Project Matters

Traditional SOC workflows often suffer from alert fatigue, repetitive manual triage, and slow escalation processes.

This project demonstrates how modern security teams can combine SIEM platforms, automation engines, AI tooling, and case management systems to create a faster and more scalable detection and response process.

It reflects real-world security engineering concepts used in enterprise environments.

---

## Example Workflow

1. Suspicious activity occurs on endpoint
2. Logs are forwarded to Splunk
3. Detection rule triggers alert
4. n8n receives webhook
5. AI analyzes the alert context
6. Slack receives formatted notification
7. DFIR-IRIS case is created automatically
8. Analyst begins investigation

---

## Screenshots

* Splunk detection dashboard
* n8n workflow
* Slack alert example
* DFIR-IRIS ticket
* Claude natural language query
* Overall architecture diagram

---

## Future Enhancements

* Threat intelligence enrichment (VirusTotal / AbuseIPDB)
* Automatic containment actions
* Risk scoring engine
* Executive reporting dashboard
* MITRE ATT&CK mapping
* Multi-host correlation detections


## Credit

The overall architecture and project inspiration were influenced by **Steven Mah** and the [MYDFIR Forge community](https://www.skool.com/mydfir), whose practical labs and shared knowledge helped shape the direction of this build.

This project was independently implemented, customized, and extended as a hands-on learning environment focused on modern SOC workflows, automation, and detection engineering.
