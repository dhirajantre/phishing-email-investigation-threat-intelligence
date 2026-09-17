# 📧 Phishing Email Investigation & Threat Intelligence

A hands-on SOC Analyst project focused on phishing email investigation, email header analysis, IOC extraction, threat intelligence enrichment, phishing risk assessment, MITRE ATT&CK mapping, and SOC response recommendations.

The project demonstrates the following investigation workflow:

**Phishing Email → Header Analysis → IOC Extraction → Threat Intelligence → Risk Assessment → MITRE ATT&CK → Analyst Assessment → SOC Response**

---

## 📌 Project Overview

This project simulates a phishing email investigation in a controlled cybersecurity laboratory environment.

A synthetic phishing email was created containing multiple indicators commonly associated with phishing activity.

The email was analyzed from a SOC Analyst perspective to identify suspicious characteristics, extract indicators, assess authentication results, enrich those indicators against real threat-intelligence platforms, map the observed behavior to MITRE ATT&CK, and document the investigation.

The project was intentionally designed using synthetic test data. No real person, organization, production system, or real attacker infrastructure was targeted.

---

## 🎯 Objectives

- Analyze a phishing email
- Investigate email headers
- Analyze sender information
- Analyze Reply-To and Return-Path fields
- Review SPF, DKIM, and DMARC results
- Identify suspicious email characteristics
- Extract Indicators of Compromise (IOCs)
- Analyze URLs, domains, and IP addresses
- Enrich indicators against real threat-intelligence platforms
- Assess phishing risk
- Map the observed behavior to MITRE ATT&CK
- Determine whether evidence of compromise exists
- Document recommended SOC response actions
- Produce professional investigation documentation

---

## 🛠️ Technologies & Tools

| Technology / Tool | Purpose |
| --- | --- |
| **Windows 11** | Investigation environment |
| **Email Header Analysis** | Sender, routing, and authentication analysis |
| **VirusTotal** | Domain and URL reputation lookup |
| **urlscan.io** | Live URL scan and DNS resolution check |
| **AbuseIPDB** | IP reputation analysis |
| **Google Admin Toolbox (Messageheader)** | Tool-based SPF/DKIM/DMARC header verification |
| **MITRE ATT&CK** | Adversary behavior mapping |
| **Notepad** | Synthetic `.eml` sample creation and analysis |

---

## 📧 Synthetic Phishing Email

A synthetic phishing email was created specifically for this controlled laboratory exercise.

**Subject**
```
URGENT: Your Microsoft 365 Account Will Be Suspended
```

**Sender**
```
Microsoft Security <security@micr0soft-security.example>
```

**Reply-To**
```
account-verification@micr0soft-security.example
```

**Return-Path**
```
account-verification@micr0soft-security.example
```

**Verification URL**
```
https://login-micr0soft-security.example/verify
```

**Source IP**
```
192.0.2.10
```

---

## 🔎 Email Header Analysis

The email header was analyzed to identify sender identity, routing information, reply handling, and email authentication results.

### Header Findings

| Header / Indicator | Observed Value |
| --- | --- |
| **From** | security@micr0soft-security.example |
| **To** | employee@example.test |
| **Subject** | URGENT: Your Microsoft 365 Account Will Be Suspended |
| **Reply-To** | account-verification@micr0soft-security.example |
| **Return-Path** | account-verification@micr0soft-security.example |
| **Received** | mail.example.test (192.0.2.10) |
| **SPF** | FAIL |
| **DKIM** | FAIL |
| **DMARC** | FAIL |

---

## 🚩 Phishing Indicators Identified

The investigation identified multiple suspicious characteristics in the synthetic email.

**1. Brand Impersonation** — the sender domain contains `micr0soft`, using the number `0` in place of the letter `o`. This creates a brand-impersonation / typosquatting-style indicator.

**2. Sender and Reply-To Analysis** — the visible sender (`security@...`) and the Reply-To address (`account-verification@...`) use different mailboxes on the same suspicious-looking domain. This mismatch was treated as a suspicious indicator requiring investigation.

**3. SPF Failure** — the synthetic authentication result shows `SPF = FAIL`, meaning the sending server is not authorized for the domain.

**4. DKIM Failure** — the synthetic authentication result shows `DKIM = FAIL`, an additional suspicious authentication indicator.

**5. DMARC Failure** — the synthetic authentication result shows `DMARC = FAIL`. The combination of SPF, DKIM, and DMARC failures increases the suspicion associated with the sample.

**6. Urgent Account-Suspension Language** — the email claims the account will be suspended unless verification is completed within 24 hours. This urgency was treated as a phishing indicator.

**7. Suspicious Verification URL** — the email links to `https://login-micr0soft-security.example/verify`, using the same suspicious `micr0soft` domain and presenting an account-verification action.

---

## 📸 Email Header Evidence

![Phishing Email Header Analysis](https://github.com/dhirajantre/phishing-email-investigation-threat-intelligence/raw/main/01-phishing-email-header.png)

The screenshot shows the synthetic phishing email, sender information, routing fields, authentication results, source IP, and verification URL used during the investigation.

---

## 🔍 IOC Extraction

| IOC Type | Indicator |
| --- | --- |
| **Sender Email** | `security@micr0soft-security.example` |
| **Reply-To** | `account-verification@micr0soft-security.example` |
| **Domain** | `micr0soft-security.example` |
| **URL** | `https://login-micr0soft-security.example/verify` |
| **Source IP** | `192.0.2.10` |

---

## 🛡️ Threat Intelligence Enrichment

Each extracted indicator was checked against real threat-intelligence platforms, following the same enrichment workflow a SOC analyst would use in production triage.

### Domain & URL Reputation — VirusTotal

Both the domain and the full verification URL were searched in VirusTotal. Neither returned a match, since the `.example` domain has never resolved on the public internet and carries no scan history. An absent VirusTotal record is the expected result for a synthetic, non-resolving domain — it confirms the indicator has no real-world footprint, rather than indicating the check was skipped.

![VirusTotal URL lookup](https://github.com/dhirajantre/phishing-email-investigation-threat-intelligence/raw/main/03-virustotal-url-lookup.png)

*VirusTotal search for the verification URL — no matching item found.*

![VirusTotal domain lookup](https://github.com/dhirajantre/phishing-email-investigation-threat-intelligence/raw/main/02-virustotal-domain-lookup.png)

*VirusTotal search for the sender domain — no community data on record.*

### URL Scan — urlscan.io

The verification URL was submitted to urlscan.io for a live scan. The scan failed with a DNS resolution error, confirming the domain does not resolve to any IPv4/IPv6 address — independent confirmation that the domain is a non-existent, synthetic value rather than live phishing infrastructure.

![urlscan.io result](https://github.com/dhirajantre/phishing-email-investigation-threat-intelligence/raw/main/04-urlscan-dns-error.png)

*urlscan.io scan result — DNS error, domain could not be resolved.*

### IP Reputation — AbuseIPDB

The source IP `192.0.2.10` was checked against AbuseIPDB, which returned 11 historical abuse reports from 5 sources, categorized as brute-force, SSH, DNS compromise, DDoS, and web-app attack activity.

AbuseIPDB's own page flags this address as reserved for documentation/testing (RFC 5737 TEST-NET-1) and states explicitly that any abuse reports against it come from internal misconfiguration or other users' testing, not real attacker infrastructure — the platform presents these particular reports **"for entertainment and testing purposes only."** This independently corroborates the lab's synthetic framing: the report count reflects other analysts and tools exercising the same test address, not a real malicious actor.

![AbuseIPDB lookup](https://github.com/dhirajantre/phishing-email-investigation-threat-intelligence/raw/main/05-abuseipdb-ip-check.png)

*AbuseIPDB lookup for 192.0.2.10, including the platform's own reserved-address notice.*

### Header Verification — Google Admin Toolbox (Messageheader)

The raw headers were run through Google's Messageheader tool as an independent, tool-based check on the manual review above. The tool confirmed all three authentication results as failed, matching the synthetic `Authentication-Results` header exactly: **SPF fail (IP Unknown)**, **DKIM fail (domain Unknown)**, **DMARC fail**. The "Unknown" values occur because the tool attempts to resolve `mail.example.test` against the live internet and cannot — a further, independent confirmation that the sample is synthetic rather than a real delivered message.

![Header analyzer result](https://github.com/dhirajantre/phishing-email-investigation-threat-intelligence/raw/main/06-header-analyzer-result.png)

*Google Admin Toolbox Messageheader output for the corrected header block.*

### Threat Intelligence Summary

| Indicator | Platform | Result |
| --- | --- | --- |
| Verification URL | VirusTotal | No record found |
| Sender domain | VirusTotal | No record found |
| Verification URL | urlscan.io | DNS resolution failed |
| Source IP 192.0.2.10 | AbuseIPDB | Flagged reserved/test address; 11 reports, not attributable to real attacker infrastructure |
| Email headers | Google Admin Toolbox | SPF/DKIM/DMARC fail confirmed; source host unverifiable (consistent with synthetic domain) |

---

## ⚠️ Phishing Risk Assessment

| Indicator | Assessment |
| --- | --- |
| Brand impersonation | Suspicious |
| Sender domain | Suspicious |
| Sender / Reply-To context | Suspicious |
| SPF | FAIL |
| DKIM | FAIL |
| DMARC | FAIL |
| Urgent account-suspension language | Suspicious |
| Account verification request | Suspicious |
| Verification URL | Suspicious-looking |

---

## 🧑‍💻 Analyst Assessment

### Final Classification

**PHISHING EMAIL — HIGH SUSPICION**

The email contains multiple phishing indicators, including:
- Brand impersonation
- Suspicious sender domain
- Sender / Reply-To concerns
- Failed SPF, DKIM, and DMARC authentication
- Urgent account-suspension language
- Suspicious account-verification URL

Threat-intelligence enrichment against VirusTotal, urlscan.io, AbuseIPDB, and header verification found no real-world reputation data tied to a live attacker, consistent with a lab-generated sample.

This sample is synthetic and was created specifically for controlled SOC training and investigation practice.

---

## 🚫 Compromise Assessment

No evidence was collected showing that:
- The URL was clicked
- Credentials were submitted
- Malware was executed
- An endpoint was compromised
- An account was successfully compromised

```
NO CONFIRMED COMPROMISE IDENTIFIED
```

The investigation remains limited to the evidence available from the synthetic email sample.

---

## 🎯 MITRE ATT&CK Mapping

**Tactic:** Initial Access
**Technique:** T1566 – Phishing
**Sub-technique:** T1566.002 – Phishing: Spearphishing Link

### Mapping Rationale

The synthetic email contains a link presented as an account-verification action, supporting the mapping to `T1566.002 – Phishing: Spearphishing Link`.

`T1204.001 – User Execution: Malicious Link` was not asserted because there is no evidence in this laboratory exercise that the recipient clicked the link.

---

## 🛡️ Recommended SOC Response

If similar activity were detected in a production environment, a SOC Analyst could:

1. Quarantine or flag the email according to the organization's email-security procedures.
2. Review the sender, Reply-To, domain, URL, and authentication results.
3. Search the environment for other emails containing the same indicators.
4. Identify other recipients who received the message.
5. Determine whether any recipient interacted with the URL.
6. Check whether credentials were submitted.
7. If user interaction occurred, investigate the affected endpoint and account.
8. Block confirmed malicious indicators according to organizational procedures.
9. Monitor affected accounts and endpoints for follow-on activity.
10. Document and escalate the incident according to the organization's incident-response process.

---

## 🔄 SOC Investigation Workflow

```
Phishing Email
      ↓
Email Header Analysis
      ↓
Sender / Reply-To Analysis
      ↓
SPF / DKIM / DMARC Analysis
      ↓
IOC Extraction
      ↓
URL / Domain / IP Analysis
      ↓
Threat Intelligence
      ↓
Risk Assessment
      ↓
MITRE ATT&CK Mapping
      ↓
Analyst Assessment
      ↓
SOC Response
      ↓
Incident Documentation
```

---

## 📁 Project Files

```
phishing_email_sample.eml
01-phishing-email-header.png
02-virustotal-domain-lookup.png
03-virustotal-url-lookup.png
04-urlscan-dns-error.png
05-abuseipdb-ip-check.png
06-header-analyzer-result.png
Dhiraj_Antre_Phishing_Email_Investigation_Report.pdf
README.md
```

---

## 📄 Documentation Report

A detailed project documentation report is included in the repository.

**Report:** `Dhiraj_Antre_Phishing_Email_Investigation_Report.pdf`

The report documents:
- Executive Summary
- Project Objective
- Synthetic Email Sample
- Email Header Analysis
- IOC Extraction
- Threat Intelligence Enrichment (VirusTotal, urlscan.io, AbuseIPDB, header verification)
- Phishing Risk Assessment
- MITRE ATT&CK Mapping
- Analyst Assessment
- Recommended SOC Response
- SOC Workflow
- Skills Demonstrated
- Project Status
- Conclusion

---

## 📚 Skills Demonstrated

**Email Security**
- Phishing Email Analysis
- Email Header Analysis
- Sender / Reply-To / Return-Path Analysis
- SPF, DKIM, and DMARC Analysis

**Threat Intelligence**
- IOC Extraction
- URL Analysis (VirusTotal, urlscan.io)
- Domain Analysis (VirusTotal)
- IP Reputation Analysis (AbuseIPDB)
- Header Verification (Google Admin Toolbox)
- Indicator Enrichment

**SOC Operations**
- Phishing Triage
- Security Investigation
- Risk Assessment
- Incident Documentation
- SOC Response Recommendations

**Security Framework**
- MITRE ATT&CK
- T1566 – Phishing
- T1566.002 – Spearphishing Link

---

## 💡 Key Learning Outcomes

Through this project, I practiced:
- Investigating phishing email headers and authentication results
- Understanding Reply-To and Return-Path fields
- Extracting email-based IOCs
- Applying a real threat-intelligence enrichment workflow across multiple platforms
- Interpreting an absent or reserved-address result as a meaningful finding, not a dead end
- Assessing phishing risk
- Mapping phishing activity to MITRE ATT&CK
- Distinguishing suspicious activity from confirmed compromise
- Documenting SOC investigation findings
- Developing a structured incident-response workflow

---

## ⚠️ Lab Disclaimer

This project uses synthetic phishing-email data for cybersecurity learning and SOC investigation practice.

The `.example` domain and documentation IP address are used as non-production test indicators. No real person, organization, production system, or real attacker infrastructure was targeted. No real credentials were collected or submitted. No real phishing campaign was conducted.

---

## 🚀 Project Status

**Completed ✅**

### Completed Components

- [x] Synthetic Phishing Email Creation
- [x] Email Header Analysis
- [x] Sender Analysis
- [x] Reply-To Analysis
- [x] Return-Path Analysis
- [x] SPF Analysis
- [x] DKIM Analysis
- [x] DMARC Analysis
- [x] Phishing Indicator Identification
- [x] IOC Extraction
- [x] URL Analysis
- [x] Domain Analysis
- [x] IP Analysis
- [x] Threat Intelligence Enrichment
- [x] Phishing Risk Assessment
- [x] MITRE ATT&CK Mapping
- [x] Analyst Assessment
- [x] SOC Response Recommendations
- [x] Evidence Screenshots
- [x] Documentation Report

---

## 👨‍💻 Project Summary

This project demonstrates a practical phishing email investigation workflow from initial email analysis through IOC extraction, threat-intelligence enrichment, phishing risk classification, MITRE ATT&CK mapping, analyst assessment, and recommended SOC response.

The investigation was performed using synthetic laboratory data and focuses on demonstrating practical entry-level SOC Analyst skills in:

**Email Security → Threat Intelligence → Phishing Investigation → MITRE ATT&CK → Incident Response**
