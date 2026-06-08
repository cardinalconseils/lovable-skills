---
name: churn-prevention
description: Use when the user wants to reduce customer churn, improve retention, design a cancellation flow, or build a win-back strategy. Also use when the user mentions 'churn rate', 'customer health score', 'dunning', 'cancellation flow', 'win-back', or 'voluntary churn'.
---

# Churn Prevention

Expert knowledge for diagnosing churn, building health scores, designing cancellation flows, and recovering lost customers.

## Churn Taxonomy

**Voluntary churn** — customer actively cancels. Indicates perceived value failure or better alternative found.

**Involuntary churn** — payment fails, not recovered. 20-40% of churn in most SaaS is involuntary. Entirely preventable with good dunning.

**Silent churn** — customer stops using the product but doesn't cancel (common in freemium / annual plans). Dangerous because it's invisible until renewal. Detect via login frequency and feature usage.

**Churn rate formula:**
```
Monthly Churn Rate = Customers Lost This Month / Customers at Start of Month
Annual Churn Rate ≈ Monthly Rate × 12 (approx — compound for precision)
```

**Benchmarks (B2B SaaS):** < 5% annual = healthy. 5–10% = watch. > 10% = priority problem.

## Customer Health Score

A leading indicator of churn risk. Compute weekly per account.

**Formula:**
```
Health Score = (Login Frequency × 0.3)
             + (Feature Usage Depth × 0.4)
             + (Expansion Signal × 0.2)
             + (Support Sentiment × 0.1)
```

**Signal definitions:**
- **Login Frequency:** Sessions in last 30 days / expected sessions (based on plan type)
- **Feature Usage Depth:** Core features used / total core features available
- **Expansion Signal:** Has added users, upgraded, or integrated in last 90 days (binary: 1 or 0)
- **Support Sentiment:** Recent ticket sentiment score (positive = 1.0, neutral = 0.5, negative = 0.0)

**Score thresholds:**
- 0.7–1.0: Healthy
- 0.4–0.69: At Risk — trigger proactive outreach
- 0–0.39: Critical — escalate to CSM or automated save sequence

**Adapt weights to your product.** If login is a weak signal (e.g., API-first product), increase feature usage weight.

## Cancellation Flow Architecture

A good cancellation flow has one job: surface the right save offer at the right moment.

**Step 1 — Pause option**
Before asking for a reason, offer a pause (1–3 months). Resolves 15-25% of cancellations from customers who are temporarily budget-constrained.

**Step 2 — Reason collection**
Ask for one reason (not a long survey). Use it to route to the right save offer.

| Reason | Save Offer |
|---|---|
| Too expensive | Discount (20–30%) or downgrade |
| Not using it enough | Usage coaching / feature tour |
| Missing feature | Roadmap preview + feature request |
| Switching to competitor | Direct comparison + migration help |
| Business shutting down | Acknowledge + offboard gracefully |

**Step 3 — Save offer**
One specific offer, matched to the stated reason. Never offer a generic "here's a discount" — it signals you were overcharging.

**Step 4 — Confirm cancellation**
If they proceed: make it one click. A difficult cancellation creates a negative brand moment that gets shared.

## Dunning Sequence (Involuntary Churn)

| Day | Action | Channel |
|---|---|---|
| 0 | Payment failed — retry immediately (often transient) | Auto |
| 1 | Notify customer: payment failed, update card | Email |
| 3 | Retry payment | Auto |
| 7 | Reminder email: account at risk of suspension | Email |
| 10 | Final retry | Auto |
| 20 | Account suspended (access revoked, data retained) | Email |
| 30 | Account scheduled for deletion | Email |

**Smart retry:** Retry on different days of the week — many card failures are temporary credit limit or bank-hold issues.

**Offer:** On Day 7 email, include a one-click link to update payment. Remove every possible friction from the recovery path.

## Win-Back Strategy

**Timing:** Wait 60–90 days after cancellation before win-back outreach. Too soon feels desperate. After 90 days, they've often committed to an alternative.

**Win-back sequence (3 emails over 2 weeks):**
1. **Day 60:** "What brought you back" curiosity email — no hard sell. Ask what would make you reconsider.
2. **Day 67:** New feature announcement relevant to their cancellation reason.
3. **Day 74:** One-time offer (discount or extended trial) with clear expiration.

**Segmentation:** Customize by cancellation reason. A customer who left for a competitor needs a different message than one who left for budget reasons.

**Never win back:** Customers who cancelled due to a bad support experience without first resolving the support issue.

## NPS and Churn Correlation

NPS detractors (0–6) churn at 3–5× the rate of promoters (9–10).

**Action by segment:**
- **Promoters (9–10):** Ask for referrals, case studies, G2 reviews
- **Passives (7–8):** Identify the one thing that would move them to promoter and address it
- **Detractors (0–6):** Assign to CSM within 24 hours. Understand root cause. Detractor rescue before churn, not after.

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "Churn is normal, some customers just leave" | Voluntary churn above 5% annual signals a product or value-delivery problem, not natural attrition. |
| "We shouldn't make cancellation hard" | Easy cancellation ≠ no save attempt. A good save offer is helpful, not friction. |
| "Win-back doesn't work" | Win-back to previously happy customers converts at 20–40%. Ignore it at your expense. |
| "Involuntary churn isn't our fault" | It's entirely recoverable with dunning. Accepting it is choosing to lose revenue. |

## Verification

- [ ] Monthly and annual churn rate tracked in dashboard
- [ ] Customer health score computed weekly per account
- [ ] At-risk accounts (score < 0.4) trigger automated or CSM outreach
- [ ] Cancellation flow includes: pause option → reason → save offer → one-click confirm
- [ ] Dunning sequence configured with smart retry and payment update link
- [ ] Win-back sequence triggered at Day 60 post-cancellation
- [ ] NPS collected and detractors routed to CSM within 24 hours
