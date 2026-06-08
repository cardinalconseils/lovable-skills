---
name: product-metrics
description: Use when the user wants to define how to measure product success, identify a north star metric, build a metrics framework, or understand what to track. Also use when the user mentions 'north star metric,' 'KPIs,' 'OKRs,' 'what should we measure,' 'product analytics,' 'leading indicators,' or 'success metrics.'
---

# Product Metrics

Metrics tell you if your product is working. The wrong metrics tell you the wrong story and optimize for the wrong outcomes.

## North Star Metric

One metric that best captures the value your product delivers to customers. When it goes up, customers are winning. When it goes up sustainably, the business is winning.

**Good north star metrics:**
- Spotify: time spent listening
- Airbnb: nights booked
- Slack: messages sent
- HubSpot: contacts touched

**Bad north star metrics:**
- Revenue (output, not value delivered)
- Registered users (not engaged users)
- App downloads (not activated users)

**Test:** Does this metric go up when customers are getting genuine value? Can it go up in ways that don't reflect real value? (If yes, it's gameable — find a better metric.)

## Metric Tree

Decompose the north star into its drivers:

```
North Star Metric
├── Driver 1 (acquisition)
│   ├── Input metric A
│   └── Input metric B
├── Driver 2 (activation)
│   ├── Input metric C
│   └── Input metric D
└── Driver 3 (retention)
    ├── Input metric E
    └── Input metric F
```

Input metrics are what teams can influence directly. The north star is what you're optimizing for.

## AARRR Funnel Metrics

| Stage | What it measures | Example metric |
|---|---|---|
| Acquisition | How users find you | CAC, traffic by channel |
| Activation | First value moment | % completing onboarding, time to aha moment |
| Retention | Do they come back? | D7, D30 retention; churn rate |
| Revenue | Do they pay? | MRR, ARPU, trial-to-paid conversion |
| Referral | Do they tell others? | NPS, viral coefficient, referral rate |

Most products have a weak activation stage. Fix activation before optimizing acquisition.

## OKR Format

**Objective:** Qualitative, inspiring, time-bound direction.
**Key Results:** 2-4 measurable outcomes that define what achieving the objective looks like.

- O: Make our onboarding the fastest in the category
  - KR1: Reduce median time to first value from 4 days to 1 day
  - KR2: Increase D7 retention for new users from 32% to 45%
  - KR3: Achieve NPS > 40 for users in their first 30 days

**OKR anti-patterns:**
- Key results that are outputs ("launch feature X") not outcomes
- More than 4 KRs per objective (dilutes focus)
- KRs that can be achieved without the objective being real

## Leading vs Lagging Indicators

| Type | Definition | Example |
|---|---|---|
| Lagging | What already happened | Revenue, churn, NPS |
| Leading | Predicts what will happen | Feature adoption rate, daily active usage, support ticket volume |

Manage with leading indicators. Report on lagging indicators.

## Counter-Metrics

For every primary metric, define a counter-metric that prevents gaming.

- Primary: messages sent / Counter: messages that get replies (quality)
- Primary: sessions per day / Counter: task completion rate (not just opening the app)
- Primary: trial signups / Counter: trial-to-paid conversion (not just top of funnel)

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "We track everything" | Tracking everything is the same as tracking nothing. Name the one that matters. |
| "Revenue is the north star" | Revenue is the result of value delivered. Measure the value. |
| "Our NPS is great so the product is working" | NPS is a lagging satisfaction signal. It doesn't tell you what to fix. |
| "We don't have enough data yet" | Define the metrics now. You'll never have "enough" data if you haven't defined what you're measuring. |

## Verification

- [ ] North star metric defined and passes the value test
- [ ] Metric tree maps north star to driver metrics to input metrics
- [ ] Each team/squad owns specific input metrics
- [ ] OKRs have measurable key results (not output-based)
- [ ] Counter-metric defined for every primary metric
