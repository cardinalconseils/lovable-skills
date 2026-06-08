---
name: compliance
description: Use when the user wants to understand compliance requirements, identify regulatory triggers, or know what artifacts are required for GDPR, PCI, HIPAA, or SOC 2. Also use when the user mentions 'compliance', 'GDPR', 'PCI DSS', 'HIPAA', 'SOC 2', 'data privacy', 'regulatory requirements', or 'audit artifacts'.
---

# Compliance

Expert knowledge for identifying regulatory triggers, classifying data, and producing the required compliance artifacts for web applications handling sensitive data.

## Regulatory Trigger Table

Read this table first. If your project matches any row, the listed requirements apply.

| Regulation | Trigger | Core Requirements |
|---|---|---|
| **GDPR** | Processing personal data of EU residents | Consent management, right to erasure, data processing records, DPA with processors, breach notification (72h) |
| **CCPA** | California users, > $25M revenue or > 100K records | Privacy policy, opt-out of sale, do-not-sell link, data deletion on request |
| **PCI DSS** | Accepting, storing, or transmitting card data | Never store raw card numbers, use PCI-certified payment processor, quarterly scans, SAQ completion |
| **HIPAA** | Storing or transmitting US health information | PHI encryption at rest + in transit, BAA with all vendors handling PHI, audit logs, minimum-necessary access |
| **SOC 2** | Selling B2B SaaS to enterprises | Trust Service Criteria (Security mandatory; Availability, Confidentiality, Privacy, Processing Integrity optional), annual audit |
| **Quebec Law 25** | Processing personal data of Quebec residents | Privacy impact assessment, explicit consent, privacy officer designation, 72h breach notification |

## Data Classification

Classify every data element before storing it. Classification drives encryption, access control, and retention requirements.

| Class | Examples | Requirements |
|---|---|---|
| **Public** | Marketing copy, published prices | No special handling |
| **Internal** | Employee names, internal docs | Access control, no external sharing |
| **Confidential** | Customer PII, business financials | Encryption at rest, access logs, retention policy |
| **Regulated** | Card data, health records, biometrics | Regulation-specific requirements (see table above) |

## Required Artifacts by Regulation

### GDPR
- [ ] Privacy Policy (plain language, all processing activities listed)
- [ ] Cookie consent banner (granular consent, pre-ticked boxes prohibited)
- [ ] Data Processing Agreement (DPA) with every data processor (e.g., Stripe, SendGrid, analytics)
- [ ] Record of Processing Activities (ROPA) — internal document
- [ ] Data deletion workflow (right to erasure — must execute within 30 days)
- [ ] Breach notification procedure (72-hour window to supervisory authority)

### PCI DSS
- [ ] Never store: full card number (PAN), CVV, PIN block — not in any form, not in logs
- [ ] Use a PCI-certified payment processor (Stripe, Braintree, Adyen) for all card handling
- [ ] Self-Assessment Questionnaire (SAQ A for redirected card entry, SAQ D for stored card data)
- [ ] TLS 1.2+ on all card-data paths
- [ ] Quarterly vulnerability scans (ASV scan for public-facing systems)

### HIPAA
- [ ] Business Associate Agreement (BAA) with every vendor that may handle PHI
- [ ] PHI encrypted at rest (AES-256) and in transit (TLS 1.2+)
- [ ] Audit logs: who accessed PHI, when, from where (retained 6 years)
- [ ] Minimum necessary access: staff see only the PHI required for their role
- [ ] Breach notification: patients within 60 days, HHS annually or within 60 days if > 500 affected

### SOC 2 (Security Criterion — mandatory)
- [ ] Access control policy documented
- [ ] Change management process documented
- [ ] Incident response plan documented and tested
- [ ] Vendor risk management documented (assess each vendor's security)
- [ ] Annual penetration test (for Type II)
- [ ] Monitoring and alerting in place for security events

## Privacy by Design Checklist

Apply at project start, not as a retrofit:

- [ ] Collect only data you have a specific use for (data minimization)
- [ ] Set a retention period for every data type (default: delete after 90 days unless required)
- [ ] Default to opt-in, not opt-out, for non-essential processing
- [ ] Encrypt all Confidential and Regulated data at rest
- [ ] Log access to sensitive data (who, when, what)
- [ ] Plan the deletion workflow before storing the data

## Phase Gate Behavior

**At project start:** Scan the data model and integration list for regulatory triggers. Produce a compliance surface map listing triggered regulations and required artifacts.

**Before production launch:** Verify all required artifacts exist and are current. A missing DPA or incomplete privacy policy is a launch blocker at Candidate/Production maturity.

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "We don't store sensitive data" | Logs, analytics, and support tools often store more than you think. Audit your data flows. |
| "We're too small for GDPR to apply" | GDPR applies based on where your users are located, not your company's size. |
| "We'll add compliance later" | Retrofitting consent management and deletion workflows into a live system is expensive. Design it in from day one. |
| "Stripe handles PCI for us" | Stripe handles card storage. You handle your own network, logs, and key management. SAQ still required. |

## Verification

- [ ] Regulatory trigger table reviewed and matching regulations identified
- [ ] Data classification applied to all stored data
- [ ] Required artifacts listed for each triggered regulation
- [ ] Privacy Policy published and covers all processing activities
- [ ] DPA signed with all data processors handling personal data
- [ ] Data deletion workflow tested end-to-end
- [ ] No raw card data stored in any system, log, or database
