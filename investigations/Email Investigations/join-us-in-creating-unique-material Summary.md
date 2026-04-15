

# Email Phishing Investigation

## Analyst
Veselin Dimitrov

## Overview
This project documents the investigation of a suspicious email identified as a likely phishing and brand impersonation attempt. The analysis focused on email headers, sender infrastructure, domain indicators, and phishing characteristics commonly observed in real-world campaigns.

This project demonstrates practical skills in phishing detection, IOC analysis, threat assessment, and security reporting.

---

## Objectives
- Analyze suspicious email activity
- Identify phishing indicators
- Extract Indicators of Compromise (IOCs)
- Assess potential attacker intent
- Provide remediation recommendations
- Map observed behavior to MITRE ATT&CK techniques

---

## Investigation Summary
A suspicious email was received on **2025-07-13 11:50:03 UTC** with the subject:

**Join Us in Creating Unique Material**

The message used a freemail sender address while directing replies to a newly registered external domain. Several indicators suggested phishing / social engineering intent.

---

## Key Findings

- Sender used freemail domain: `libero.it`
- Reply-To domain did not match sender domain
- Suspicious external domain: `duolingo-team.com`
- Domain recently registered
- Missing Date header
- Multiple IP addresses involved
- Generic content with no personalization
- No professional signature or verifiable company details
- Likely brand impersonation attempt

---

## Indicators of Compromise (IOCs)

### Domains
- libero.it
- duolingo-team.com

### IP Addresses
- 5.9.230.8
- 90.160.50.35

### Email Addresses
- duolingo.ads@libero.it
- info@duolingo-team.com

---

## Investigation Process

1. Reviewed sender, Reply-To, and message headers
2. Validated SPF / DKIM results
3. Checked sender legitimacy
4. Investigated domain age and trust indicators
5. Assessed IP reputation
6. Reviewed email body for phishing traits
7. Classified risk and recommended actions

---

## MITRE ATT&CK Mapping

| Technique ID | Technique Name | Relevance |
|-------------|----------------|-----------|
| T1566 | Phishing | Suspicious email used for social engineering delivery |
| T1566.002 | Spearphishing Link | Reply-To encourages communication with attacker-controlled domain |
| T1583.001 | Acquire Infrastructure: Domains | Newly registered domain used in phishing activity |
| T1583.005 | Acquire Infrastructure: Botnet / VPS / Hosting | Suspicious IP infrastructure used to obscure origin |
| T1036 | Masquerading | Attempted impersonation of a known brand |
| T1204 | User Execution | Relies on user interaction and response |

---

## Risk Assessment

**Severity:** Medium  
**Likelihood:** High  
**Type:** Phishing / Brand Impersonation / Social Engineering

---

## Recommendations

- Search for similar emails across the environment
- Block suspicious domains and IPs
- Review email gateway detections
- Conduct phishing awareness training
- Monitor for follow-up activity
- Report abuse to relevant providers

---

## Skills Demonstrated

- Email Header Analysis
- IOC Extraction
- Threat Hunting
- Phishing Detection
- MITRE ATT&CK Mapping
- Security Reporting
- Analytical Investigation
- Communication of Findings

---

## Full Report

See detailed report in:

`report/Email_Investigation.pdf`
