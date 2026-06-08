---
name: kpi-architect
description: Use when the user wants to define KPIs, build a metrics framework, identify leading indicators, create a risk register, or set up governance triggers. Also use when the user mentions 'KPIs', 'metrics framework', 'leading indicators', 'lagging indicators', 'risk register', 'OKRs', 'metric governance', or 'war-gaming'.
---

# KPI Architecture

Expert knowledge for designing a metrics framework that drives decisions rather than generates reports.

## The 2-3-2 Rule

The maximum practical KPI set for any team or product:

```
2 Leading Indicators  — predict future performance (you control these)
3 Operational Metrics — measure current health (you monitor these)
2 Lagging Indicators  — confirm past performance (you report these)
─────────────────────
7 metrics total
```

**Why max 7:** More than 7 metrics on a dashboard means no metric is a priority. When everything is a KPI, nothing is.

## Leading Indicator Validation

A leading indicator is only valuable if it predicts the lagging outcome you care about. Validate before enshrining.

**Test:**
1. Does a change in this metric precede a change in the lagging indicator?
2. Is the lag time short enough to be actionable?
3. Can the team actually influence this metric with their decisions?

If any answer is "no" or "uncertain" — it's a hypothesis, not a KPI. Label it as such and validate with 90 days of data.

**Common leading indicator mistakes:**
- Vanity metrics (page views, followers) that don't predict revenue or retention
- Metrics you can't influence (industry growth rate, competitor prices)
- Metrics with lag time > 90 days (by the time they lead, it's too late to act)

## Metric Structure

For each KPI, define all five fields:

| Field | Description |
|---|---|
| **Name** | Plain English name |
| **Formula** | Exact calculation |
| **Target** | Threshold for good/at risk/critical |
| **Frequency** | How often measured |
| **Owner** | Who is responsible for moving it |

**Example:**
```
Name:      Weekly Active Teams
Formula:   Unique team_ids with ≥1 session in rolling 7 days
Target:    Good: > 500 | At Risk: 400–500 | Critical: < 400
Frequency: Daily (reported weekly)
Owner:     Head of Product
```

## War-Gaming

War-gaming tests whether your strategy is robust to competitive attack.

**3-step process:**
1. **Name the attacker.** Pick your most dangerous competitor or a plausible new entrant.
2. **Describe their move.** What is the most damaging thing they could do in the next 90 days? (Price cut, feature launch, acquisition, partnership)
3. **Name your defense.** What is your pre-planned response? If you have no response, that is a strategic gap.

**Run war-gaming quarterly.** Markets change. A defense that was solid 6 months ago may be obsolete.

**Output format:**
```
Attacker: [company or archetype]
Move:     [specific action they could take]
Impact:   [what metric this affects and by how much]
Defense:  [your response and timeline to deploy it]
Gap:      [yes/no — do you currently have this defense ready?]
```

## Risk Register

A risk register captures the top threats to your plan. Keep it focused: top 3 risks only.

**Format:**

| Risk | Probability | Impact | Mitigation |
|---|---|---|---|
| Key engineer leaves | Medium | High | Cross-train second engineer on critical system |
| Competitor launches free tier | High | High | Accelerate feature differentiation roadmap |
| Regulatory change (GDPR enforcement) | Low | Medium | Compliance surface audit scheduled Q3 |

**Scoring:**
- **Probability:** High (> 50%), Medium (20–50%), Low (< 20%)
- **Impact:** High (threatens viability), Medium (affects quarterly target), Low (manageable)

**Review cadence:** Monthly. Remove risks that materialized (handle them in operations). Add new risks as they emerge.

## Governance Triggers

Governance triggers define when a metric change is significant enough to require a decision or escalation — not a scheduled meeting.

**Format:**
```
Metric:    [KPI name]
Threshold: [specific value or % change]
Trigger:   [automated alert or meeting called]
Owner:     [who reviews and acts]
```

**Examples:**
```
Metric:    Weekly Active Teams
Threshold: < 400 (critical) for 2 consecutive weeks
Trigger:   Emergency product review within 48 hours
Owner:     Head of Product + CEO

Metric:    Churn Rate
Threshold: > 5% monthly for any segment
Trigger:   CS + Product sync within 1 week
Owner:     Head of CS
```

**Rule:** Governance triggers fire on metric+threshold, not on calendar date. Weekly syncs that review the same green dashboard waste time.

## OKR Integration

KPIs measure the health of the business. OKRs drive change. They work together:

- **KPIs:** Are we healthy? (Leading, operational, lagging — always on)
- **OKRs:** Are we improving? (Quarterly, time-boxed, 70% achievement = success)

Don't put your North Star Metric as an OKR Key Result — the NSM is a KPI. Use OKRs for the specific initiatives that move the NSM.

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "We need more than 7 metrics to cover everything" | Coverage is not the goal. Decision-making is. More metrics = more noise = worse decisions. |
| "We'll add metrics as questions come up" | Metric sprawl is how dashboards die. Add only what has a defined owner and governance trigger. |
| "Our leading indicators are obvious" | Validate them against lagging outcomes with 90 days of data before treating them as facts. |
| "We review KPIs in our weekly meeting" | Scheduled reviews miss rapid changes. Governance triggers catch problems in 48 hours, not 7 days. |

## Verification

- [ ] Maximum 7 KPIs defined (2 leading + 3 operational + 2 lagging)
- [ ] Each KPI has name, formula, target, frequency, and owner
- [ ] Leading indicators validated against lagging outcomes (or labeled as hypotheses)
- [ ] War-gaming run for top 2 competitive threats
- [ ] Risk register has top 3 risks with probability, impact, and mitigation
- [ ] Governance trigger defined for each critical KPI
- [ ] OKRs distinct from KPIs (improvement initiatives vs. health monitoring)
