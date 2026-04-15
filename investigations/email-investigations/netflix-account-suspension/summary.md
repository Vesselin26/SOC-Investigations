
# Email Phishing Investigation – Netflix Account Suspension

## Analyst

Veselin Dimitrov

## Overview
This project documents the investigation of a suspicious phishing email impersonating Netflix. The analysis focused on sender infrastructure, email authentication failures, Unicode obfuscation, redirect chains, and phishing indicators commonly observed in credential theft and malware delivery campaigns.

This project demonstrates practical skills in phishing detection, IOC extraction, infrastructure analysis, and MITRE ATT&CK mapping.

---

## Objectives
- Analyze suspicious inbound email activity
- Identify phishing indicators
- Extract Indicators of Compromise (IOCs)
- Investigate redirect infrastructure
- Assess attacker intent and risk
- Provide remediation recommendations
- Map findings to MITRE ATT&CK techniques

---

## Investigation Summary
A suspicious email was received on **2026-06-04 16:55:18 UTC** with the subject:

**Nеtfliх Аccоunt Suspеnѕіоn**

The message impersonated Netflix and used homoglyph obfuscation, urgency, and redirect infrastructure to increase credibility and encourage user interaction.

---

## Key Findings

- Sender domain: `lna.io` (not associated with Netflix)
- Randomized sender address suggests automated campaign
- Return-Path linked to attacker-controlled cloud infrastructure
- Invalid DKIM signature
- Subject used Cyrillic homoglyph substitutions
- Shortened URL via trusted infrastructure (`t.co`)
- Redirect chain identified:
  `t.co → aidangadgets.com → adorama.com`
- Final destination unrelated to Netflix
- Suspicious IP infrastructure observed
- Email used urgency to pressure recipient action
- Likely phishing or malware delivery attempt

---

## Indicators of Compromise (IOCs)

### Domains
- lna.io
- aidangadgets.com
- adorama.com
- t.co
- nxcli.io

### IP Addresses
- 185.199.110.153
- 185.199.109.153
- 185.199.108.153
- 185.199.111.153
- 172.237.149.231
- 172.66.0.227

### Email Address
- y3sox4wm@lna.io

---

## Investigation Process

1. Reviewed sender identity and message headers
2. Validated email authentication results (DKIM)
3. Assessed domain legitimacy and impersonation indicators
4. Identified homoglyph obfuscation in subject line
5. Investigated redirect chain behavior
6. Reviewed infrastructure reputation indicators
7. Assessed phishing and malware delivery risk
8. Produced remediation recommendations

---

## MITRE ATT&CK Mapping

| Technique ID | Technique Name | Relevance |
|-------------|----------------|-----------|
| T1566 | Phishing | Malicious email used as delivery vector |
| T1566.002 | Spearphishing Link | Embedded link redirects through multiple domains |
| T1036 | Masquerading | Impersonation of Netflix brand |
| T1027 | Obfuscated Files or Information | Cyrillic homoglyph substitutions used to evade detection |
| T1583.001 | Acquire Infrastructure: Domains | Suspicious domains used in campaign |
| T1583.003 | Acquire Infrastructure: Virtual Private Server | Cloud-hosted infrastructure used for delivery |
| T1204 | User Execution | Requires recipient interaction |
| T1105 | Ingress Tool Transfer | Possible malware download via redirect chain |

---

## Risk Assessment

**Severity:** High  
**Likelihood:** High  
**Type:** Credential Harvesting / Malware Delivery / Brand Impersonation

---

## Recommendations

- Search mail logs for similar emails
- Investigate web requests to identified domains
- Review proxy, DNS, and firewall logs
- Scan endpoints for newly downloaded files
- Block suspicious domains and IP addresses
- Conduct phishing awareness training focused on homoglyph attacks
- Monitor for follow-up activity

---

## Skills Demonstrated

- Email Header Analysis
- IOC Extraction
- Infrastructure Analysis
- URL Redirection Analysis
- Threat Hunting
- Phishing Detection
- MITRE ATT&CK Mapping
- Security Reporting

---

## Full Report

[View Full PDF Report](investigations/email-investigations/netflix-account-suspension/email-4-investigation.pdf)
