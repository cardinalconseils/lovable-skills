---
name: revops
description: Use when the user wants to build or improve their revenue operations, lead scoring, sales funnel, CRM hygiene, or pipeline velocity. Also use when the user mentions 'RevOps', 'MQL', 'SQL', 'lead lifecycle', 'pipeline velocity', 'CRM hygiene', 'attribution', or 'sales and marketing alignment'.
---

# Revenue Operations (RevOps)

Expert knowledge for building a revenue engine: lead lifecycle, scoring, pipeline velocity, CRM hygiene, and attribution.

## Lead Lifecycle Stages

Every contact moves through these stages exactly once in sequence. Never skip stages or move backward without a documented reason.

| Stage | Definition | Owner |
|---|---|---|
| **Subscriber** | Opted into marketing (newsletter, content) | Marketing |
| **Lead** | Submitted a form or engaged with gated content | Marketing |
| **MQL** | Meets minimum score threshold; ready for sales review | Marketing |
| **SAL** | Sales-Accepted Lead: sales confirmed MQL meets ICP | Sales |
| **SQL** | Sales-Qualified Lead: discovery call confirmed budget, authority, need, timeline | Sales |
| **Opportunity** | Proposal sent or deal room opened | Sales |
| **Customer** | Contract signed, payment collected | Revenue |
| **Churned** | Contract ended | CS |

## MQL Scoring Model

**Principle:** Score on behavior (what they do) and fit (who they are). Behavior signals intent. Fit signals potential.

**Example scoring model:**

| Signal | Points |
|---|---|
| Requested a demo | +40 |
| Viewed pricing page (2+ times) | +20 |
| Opened 3+ emails in 30 days | +15 |
| Downloaded a case study | +10 |
| Attended a webinar | +10 |
| Company size matches ICP | +15 |
| Job title is decision-maker | +10 |
| Personal email domain (Gmail/Yahoo) | ‒15 |
| Competitor domain | −100 |

**Threshold:** ≥ 50 points = MQL. Calibrate after 90 days of data — the right threshold converts 25–40% of MQLs to SALs.

## Two-Way SLA

Align marketing and sales on mutual commitments:

| Party | Commitment |
|---|---|
| **Marketing** | Deliver X MQLs per month meeting agreed ICP criteria |
| **Sales** | Contact every MQL within 24 hours (business hours), provide SAL/reject decision within 48 hours |

Track SLA adherence monthly. If sales is rejecting > 40% of MQLs, the scoring model needs calibration, not the sales team.

## Pipeline Velocity Formula

```
Velocity = (Opportunities × Win Rate × Average Deal Value) / Sales Cycle Length (days)
```

This tells you: how many dollars move through your pipeline per day.

**Improving velocity:** Each lever has a different cost:
- Increase Opportunities: most expensive (CAC)
- Increase Win Rate: cheapest (sales process, enablement)
- Increase Average Deal Value: medium (packaging, upsell)
- Decrease Cycle Length: medium (urgency, process)

## Funnel Conversion Benchmarks (B2B SaaS)

| Stage | Benchmark |
|---|---|
| MQL → SAL | 50–70% |
| SAL → SQL | 60–80% |
| SQL → Opportunity | 70–85% |
| Opportunity → Customer | 20–30% |
| Lead → Customer (overall) | 1–3% |

Below benchmark at any stage signals a specific problem: scoring model (MQL→SAL), discovery process (SAL→SQL), or proposal quality (Opp→Customer).

## CRM Hygiene Rules

Dirty CRM data is the #1 cause of inaccurate pipeline reporting.

**Data quality gates:**
- Required fields before MQL conversion: company name, company size, job title, email, source
- Enrichment at Lead creation: use Clearbit or Apollo to fill missing firmographic fields automatically
- Duplicate check: run deduplication weekly on email domain + company name
- Stage dates: every stage transition must log a timestamp (used for cycle length calculation)
- Closed-lost reason: required before moving to Closed-Lost (used to improve conversion)

**Weekly hygiene tasks:**
- Leads with no activity in 30+ days: recycle or disqualify
- Open opportunities with no activity in 14+ days: flag for sales follow-up
- Contacts with personal emails at company accounts: merge or archive

## Attribution Models

| Model | Credits | Best for |
|---|---|---|
| **First Touch** | 100% to first touchpoint | Understanding awareness channels |
| **Last Touch** | 100% to last touchpoint before conversion | Understanding conversion-driving channels |
| **Linear** | Equal credit to all touchpoints | Understanding full journey |
| **W-Shaped** | 30% first touch, 30% lead creation, 30% opportunity creation, 10% spread | B2B with clear lifecycle stages |
| **Data-Driven** | ML-weighted based on actual conversion paths | High-volume, mature data set |

**Recommendation:** W-Shaped for B2B SaaS. Reflects that both awareness and conversion-moment touches matter. Revisit when you have > 500 deals closed for data-driven.

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "We'll clean the CRM data later" | Bad data compounds. Every week of delay multiplies the cleanup effort. |
| "Our scoring model feels right" | Score models must be calibrated against actual SAL/SQL conversion rates, not intuition. |
| "Marketing generates leads, sales closes them — they're separate" | Revenue is shared. A two-way SLA aligns incentives. Silos create handoff gaps. |
| "Attribution is too complex to set up" | Start with first-touch and last-touch in your CRM. Two models beat zero. |

## Verification

- [ ] Lead lifecycle stages defined and tracked in CRM
- [ ] MQL scoring model documented with point values
- [ ] MQL threshold calibrated against actual SAL conversion rate
- [ ] Two-way SLA documented and reviewed monthly
- [ ] Pipeline velocity calculated and tracked
- [ ] Funnel conversion rates by stage tracked weekly
- [ ] CRM required fields enforced at each lifecycle stage transition
- [ ] Attribution model selected and implemented in reporting
