---
name: pricing-strategy
description: Use when the user wants to set pricing, design packaging tiers, choose a pricing model, or increase revenue from existing users. Also use when the user mentions 'pricing,' 'how much should we charge,' 'packaging,' 'pricing tiers,' 'willingness to pay,' 'freemium pricing,' 'value-based pricing,' or 'pricing model.'
---

# Pricing Strategy

Pricing is positioning made tangible. The price signals who the product is for, what category it belongs to, and what the buyer is paying for.

## Pricing Models

| Model | Best for | Risk |
|---|---|---|
| **Per seat / per user** | Collaboration tools, team software | Penalizes growth; customers limit seats |
| **Usage-based** | APIs, infrastructure, communication | Unpredictable revenue; hard to budget |
| **Flat rate** | Simple, single-segment products | Leaves money on the table at scale |
| **Tiered flat** | Multiple segments with different needs | Tier design complexity |
| **Outcome-based** | Services, high-trust relationships | Hard to measure; requires data access |

Most SaaS products are either seat-based or usage-based. Hybrid models (seat + usage cap) are increasingly common.

## Value-Based Pricing

Price based on value delivered to the customer, not cost to produce.

1. Define the economic value of the job your product does
2. Quantify it: "Saves 4 hours/week per user × $50/hr loaded cost = $800/month value"
3. Capture a fraction of that value (typically 10-30%)
4. Anchor the price to the value metric, not your costs

**Value metric:** The unit of value customers pay for. Choose a metric that scales with the value the customer gets.
- Right: seats (value scales with team size), API calls (value scales with usage)
- Wrong: features (value doesn't scale; customers pay once and expect everything)

## Packaging Tiers

Three-tier architecture (Good / Better / Best):

| Tier | Target | Design principle |
|---|---|---|
| Starter | Individual / small team | Remove friction to entry; include core job |
| Growth | Growing team / mid-market | Add collaboration and admin features |
| Enterprise | Large org | Add SSO, audit logs, SLA, dedicated support |

**Anchoring rule:** Most buyers choose the middle tier. Price the middle tier at your target ACV. Use the top tier to make the middle look reasonable.

## Willingness to Pay Research

Van Westendorp Price Sensitivity Meter — ask these four questions:

1. At what price would this be **too cheap** (you'd question the quality)?
2. At what price would this be a **bargain** (great value for money)?
3. At what price would this be **starting to get expensive** (still worth it but hesitant)?
4. At what price would this be **too expensive** (you wouldn't consider it)?

Plot the intersections. The acceptable price range sits between the "too cheap" and "too expensive" curves.

## Price Testing

Never A/B test on existing customers. Test with new prospects only:
- Show different pricing pages to different traffic segments
- Measure: conversion rate, ACV, time-to-close
- Run for minimum 2 weeks, minimum 100 conversions per variant

## Expansion Revenue

Negative churn = expansion MRR > churned MRR. The goal of tier design.

Expansion triggers:
- Seat limit reached → upgrade to next tier
- Usage cap hit → upgrade to next tier
- Admin/security feature needed → upgrade to Enterprise

Every packaging decision should answer: "What will make a customer naturally want to upgrade?"

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "We'll figure out pricing later" | Pricing shapes positioning. Shipping free and repricing later is harder than getting it right first. |
| "We need to be cheaper than the competition" | Price is a signal. Cheap signals low quality to enterprise buyers. Price for the value you deliver. |
| "Per-seat pricing is simpler" | Per-seat penalizes growth. Evaluate usage-based if value scales with usage, not just headcount. |
| "We can't raise prices on existing customers" | Annual price increases with notice are standard and expected. Lock-in forever is not a strategy. |

## Verification

- [ ] Pricing model selected with rationale (not just "what competitors do")
- [ ] Value metric defined and maps to customer value delivery
- [ ] Three-tier packaging with clear upgrade triggers
- [ ] Willingness to pay researched (minimum 5 customer conversations)
- [ ] Expansion revenue path designed into tier structure
