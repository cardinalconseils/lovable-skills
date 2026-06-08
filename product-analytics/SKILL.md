---
name: product-analytics
description: Use when the user wants to analyze product usage data, design an event tracking plan, run A/B tests, understand funnel drop-off, or make data-driven product decisions. Also use when the user mentions 'product analytics,' 'event tracking,' 'funnel analysis,' 'A/B testing,' 'cohort analysis,' 'data-driven,' or 'instrumentation.'
---

# Product Analytics

Product analytics turns user behavior into product decisions. The goal is not dashboards — it is decisions.

## Event Taxonomy

Before tracking anything, design a taxonomy:

**Naming convention:** `[Object]_[Action]` (noun_verb)
- `project_created`, `invite_sent`, `subscription_upgraded`, `report_exported`
- Not: `click_button_3`, `user_action`, `misc_event`

**Properties on every event:**
- `user_id`, `session_id`, `timestamp`
- `plan`, `account_age_days`, `cohort`

**Properties on relevant events:**
- `project_created`: `template_used`, `source` (blank / imported / template)
- `subscription_upgraded`: `from_plan`, `to_plan`, `trigger` (what page they were on)

Track sparingly. 20 well-designed events beat 200 noisy ones.

## Funnel Analysis

Map the conversion path and measure drop-off at each step:

```
Signup → Profile setup → First project created → Invited teammate → Subscribed
100%      82%             61%                     34%               18%
```

- The biggest drop = the highest-leverage fix
- Fix funnel order matters: fix activation before optimizing top of funnel
- Compare funnels across cohorts: do users from channel X convert better than channel Y?

## Cohort Analysis

Group users by the week/month they signed up. Track their behavior over time.

**Retention cohort:** What % of users from cohort X are still active after N days?
**Revenue cohort:** What is the cumulative revenue per user from cohort X over time?

Cohort analysis tells you if the product is improving over time for new users — aggregate metrics hide this.

## A/B Test Design

Before running any test:

1. **Hypothesis:** "If we [change], then [metric] will [change] because [mechanism]."
2. **Primary metric:** One metric that determines the winner
3. **Counter-metric:** One metric that would signal harm if it moves
4. **Minimum detectable effect:** How big must the change be to matter?
5. **Sample size:** Calculate before starting — use a significance calculator
6. **Duration:** Minimum 1 full week; minimum 2 business cycles; stop only when sample size is hit

**Pitfalls:**
- Peeking and stopping early (false positives)
- Testing too many variants at once
- No counter-metric (win on primary, lose on revenue)
- Running tests on < 100 events per variant per day

## Instrumentation Checklist

For any new feature:
- [ ] Activation event (first meaningful use)
- [ ] Engagement event (repeated use)
- [ ] Conversion event (paid action or key business outcome)
- [ ] Error event (failure state)
- [ ] Page/screen view (for funnel continuity)

## Common Analytics Mistakes

| Mistake | Fix |
|---|---|
| Tracking pageviews instead of actions | Track what users do, not where they go |
| No properties on events | Add segment-able properties at instrumentation time |
| Looking at averages | Segment by cohort, plan, acquisition channel |
| Dashboard without decisions | Every dashboard should have a linked question it answers |
| Testing before baseline | Establish a 2-week baseline before running any test |

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "We'll add analytics later" | Data you didn't track can never be recovered. Instrument at launch. |
| "Our sample is too small to test" | Then don't A/B test — do qualitative research until you have enough traffic. |
| "Engagement is up, the feature is working" | Define what "working" means in advance, with a specific metric and threshold. |

## Verification

- [ ] Event taxonomy documented with naming convention
- [ ] Core funnel instrumented end-to-end
- [ ] Retention cohort analysis running for every new signup cohort
- [ ] A/B test hypotheses documented with primary and counter-metrics
- [ ] Analytics reviewed weekly, tied to a product decision
