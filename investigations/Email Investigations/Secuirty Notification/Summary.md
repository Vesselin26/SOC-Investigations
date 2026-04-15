
# Email Phishing Investigation – MetaMask Impersonation

## Analyst

Veselin Dimitrov

## Overview
This project documents the investigation of a suspicious phishing email impersonating MetaMask. The analysis focused on email headers, sender infrastructure, domain reputation, obfuscation techniques, and social engineering indicators commonly used in real-world phishing campaigns.

This project demonstrates practical skills in phishing detection, IOC extraction, threat analysis, and MITRE ATT&CK mapping.

---

## Objectives
- Analyze suspicious inbound email activity
- Identify phishing indicators
- Extract Indicators of Compromise (IOCs)
- Assess attacker techniques and intent
- Provide remediation recommendations
- Map findings to MITRE ATT&CK techniques

---

## Investigation Summary
A suspicious inbound email was received on **2025-06-04 22:28:12 UTC** with the subject:

**Security Notification**

The message attempted to impersonate **MetaMask** and used urgency, obfuscation, and shortened links to increase credibility and encourage user interaction.

---

## Key Findings

- Suspicious sender domain: `cfx.io`
- Domain not associated with MetaMask
- Domain flagged for phishing activity
- Failed DKIM validation
- Matched known spam samples via Pyzor
- Used Cyrillic characters in display name ("MetaMask")
- Included shortened URL via `t.co`
- Created urgency to trigger user action
- Sent using automated PHP script
- Likely credential harvesting attempt

---

## Indicators of Compromise (IOCs)

### Domains
- cfx.io
- t.co

### Email Address
- 82p8n1e3@cfx.io

### Email ID
- +PVMBrwAQWgPowoAe4cVtA

### URL
- hxxps://t[.]co/hdSGlufCET?id=7833882918994061387-6045

---

## Investigation Process

1. Reviewed sender identity and headers
2. Checked authentication results (DKIM)
3. Assessed sender domain legitimacy
4. Investigated phishing reputation indicators
5. Identified Unicode obfuscation techniques
6. Reviewed email content for urgency and social engineering
7. Classified threat and recommended actions

---

## MITRE ATT&CK Mapping

| Technique ID | Technique Name | Relevance |
|-------------|----------------|-----------|
| T1566 | Phishing | Malicious email used as delivery vector |
| T1566.002 | Spearphishing Link | Embedded shortened link to attacker-controlled destination |
| T1036 | Masquerading | Impersonation of MetaMask brand |
| T1027 | Obfuscated Files or Information | Unicode/Cyrillic character substitution used to evade detection |
| T1583.001 | Acquire Infrastructure: Domains | Suspicious domain used in phishing campaign |
| T1204 | User Execution | Relies on user clicking link / interacting |
| T1059.006 | Command and Scripting Interpreter: PHP | Header suggests automated PHP script used to send email |

---

## Risk Assessment

**Severity:** High  
**Likelihood:** High  
**Type:** Credential Harvesting / Brand Impersonation / Phishing

---

## Recommendations

- Search email logs for related messages
- Block `cfx.io` at email and network level
- Investigate endpoint interaction with the URL
- Review proxy / DNS logs for connections
- Conduct phishing awareness training
- Monitor for follow-up activity

---

## Skills Demonstrated

- Email Header Analysis
- IOC Extraction
- Threat Hunting
- Phishing Detection
- Brand Impersonation Analysis
- MITRE ATT&CK Mapping
- Security Reporting
- Analytical Investigation

---

:contentReference[oaicite:0]{index=0}
