
# Email Threat Intelligence Report – Duolingo Brand Impersonation

## Summary
An inbound email received on **2025-07-31 08:15:51 UTC** appears to be a **brand impersonation attempt** using the Duolingo name.  
Although the message passed SPF and DKIM validation and was not flagged as spam, multiple indicators suggest it is **not an official business outreach** and may be part of a broader phishing or social engineering campaign.

---

# Key Indicators of Compromise (IOCs)

| Type | Value |
|------|------|
| Sender | duolingo-pr@sfr.fr |
| Reply-To | collab@duolingo-team.com |
| Return-Path | duolingo-pr@sfr.fr |
| Subject | Here’s a Concept We Love for You |
| Suspicious Domain | duolingo-team.com |
| Source IP | 156.146.55.242 |
| Email ID | mFAqKXdei2iByhUAe4cVtA |

---

# Technical Findings

## Authentication
- SPF: Passed
- DKIM: Passed
- Spam Filter: Not flagged

This confirms the email was legitimately sent through the sender’s mail provider, but **does not validate the sender’s identity as Duolingo**. 

---

# Red Flags Identified

## 1. Brand Mismatch
The sender used the address:

duolingo-pr@sfr.fr

This domain belongs to **SFR (French ISP)** and not to Duolingo.

---

## 2. Reply-To Mismatch
Replies are redirected to:

collab@duolingo-team.com

This is a separate domain designed to appear related to Duolingo.

---

## 3. Newly Registered Domain
The domain **duolingo-team.com** was registered on:

2025-07-10

The email was received only **21 days later**, which is highly suspicious for a legitimate corporate outreach domain. 

---

## 4. No Website Presence
DNS analysis showed:

- No A record
- No AAAA record
- No CNAME

The domain has **no active website** and appears to exist solely for email activity.

---

## 5. Hosted Mail Infrastructure
MX records point to MessagingEngine / Fastmail infrastructure:

- in1-smtp.messagingengine.com
- in2-smtp.messagingengine.com

This indicates a hosted mailbox setup, commonly used for low-cost email operations.

---

## 6. Generic Social Engineering Content
The email used:

- Generic greeting (“Hello, inquiry”)
- Flattering language
- No campaign specifics
- No named representative
- No official signature

These are common traits in mass outreach phishing campaigns.

---

## 7. Suspicious Source IP Reputation
Source IP:

156.146.55.242

External reputation data shows prior abuse reports associated with this IP range, increasing overall risk confidence. 

---

# Risk Assessment

| Category | Rating |
|---------|--------|
| Malware Risk | Low |
| Credential Theft Risk | Medium |
| Brand Impersonation | High |
| Social Engineering | High |
| Trustworthiness | Low |

---

# Verdict

**Likely brand impersonation email.**  
The message was technically valid but socially deceptive, using a newly registered lookalike domain and generic outreach tactics to build trust.

Most likely scenarios:

1. Fake collaboration scam  
2. Lead generation funnel  
3. Credential harvesting follow-up  
4. Affiliate fraud campaign

---

# Recommended Actions

## Immediate
- Do not reply
- Do not click links
- Do not share personal information
- Block `duolingo-team.com`

## Investigation
- Search logs for additional emails from:
  - sfr.fr
  - duolingo-team.com
- Review if any users interacted with sender
- Monitor future registrations using similar brand names

## Reporting
- Consider reporting:
  - abuse@sfr.fr
  - Domain registrar abuse contact
 

