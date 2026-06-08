---
name: ab-test-setup
description: Use when the user wants to run an A/B test, design an experiment, set up feature flags for testing, or measure the impact of a change. Also use when the user mentions 'A/B test', 'split test', 'experiment design', 'hypothesis', 'statistical significance', 'sample size', or 'conversion test'.
---

# A/B Test Setup

Expert knowledge for designing, running, and interpreting A/B tests that produce trustworthy results.

## Hypothesis Template

Every A/B test starts with a written hypothesis:

```
Because [observation or data point],
we believe that [change description]
will cause [primary metric] to [direction]
for [target audience].
We will know this is true when [measurement method] shows [significance threshold].
```

**Example:**
> Because 60% of signup abandonment happens on the password step,
> we believe that adding Google Sign-In as the primary CTA
> will cause signup completion rate to increase
> for new visitors from paid channels.
> We will know this is true when a two-tailed z-test shows p < 0.05 with ≥ 95% power.

## Metric Structure

Every test needs exactly these three metric categories:

**Primary metric:** The one number the test is designed to move. Success or failure is defined solely by this metric.

**Secondary metrics:** Supporting indicators that help explain *why* the primary metric moved.

**Guardrail metrics:** Metrics that must NOT regress. If a guardrail fails, the test fails — even if the primary metric wins.

| Category | Example |
|---|---|
| Primary | Signup completion rate |
| Secondary | Time-to-complete, social login adoption rate |
| Guardrail | 7-day activation rate, spam account rate |

## Sample Size Calculation

Never start a test without a pre-calculated sample size. Stopping early invalidates the result.

**Formula:**
```
n = (16 × σ²) / δ²
```
Where:
- `σ²` = variance of the metric (for proportions: `p × (1 - p)` where p = baseline rate)
- `δ` = minimum detectable effect (the smallest improvement worth acting on)
- `16` = constant for 80% power, 5% significance (two-tailed)

**Online calculators:** Evan Miller's sample size calculator, statsig.com

**Practical rule:** For conversion rates around 5–15%, detecting a 10% relative improvement requires ~10,000 users per variant. Detecting 5% requires ~40,000. Know your traffic before committing.

## Frequentist vs Bayesian

| | Frequentist | Bayesian |
|---|---|---|
| Output | p-value + confidence interval | Probability of winning + expected loss |
| When to stop | Only at pre-set sample size | Can incorporate early stopping |
| Interpretability | "If null is true, P(data) = X" | "P(variant wins) = X%" |
| Best for | Regulated decisions, high stakes | Fast iteration, product experiments |
| Tooling | Standard stats libraries | Optimizely, LaunchDarkly, Statsig |

**Recommendation:** Bayesian for product experiments (more intuitive, handles early stopping). Frequentist for pricing, billing, and legally sensitive tests.

## Test Types

**A/B test:** One control, one variant. Test one thing.

**MVT (Multivariate Test):** Multiple elements varied simultaneously. Requires much larger sample size. Use only when you need to understand interaction effects.

**Multi-Armed Bandit:** Dynamically shifts traffic to better-performing variants. Use for optimization, not for learning (it exploits, it doesn't explain).

## The Peeking Problem

Checking results before the pre-set sample size is reached inflates the false positive rate from 5% to ~40%. This means 40% of "winners" are actually noise.

**Rule:** Set your end date before you start. Do not look at results until the end date. Lock the dashboard if needed.

**Exceptions:** Guardrail metric failures. If a guardrail (e.g., error rate) is spiking, stop the test immediately — safety trumps purity.

## PIE Prioritization Framework

Use PIE to decide which tests to run first:

```
PIE Score = (Potential + Importance + Ease) / 3
```

- **Potential:** How much improvement is possible? (1–10, based on current performance gap)
- **Importance:** How much traffic / revenue does this page/flow get? (1–10)
- **Ease:** How complex is the implementation? (10 = easy, 1 = hard)

Run highest PIE score first.

## Testing Matrix

Test one dimension at a time. Each dimension is a separate test:

| Dimension | Variants to Test |
|---|---|
| Hook / headline | Problem-first vs outcome-first vs social proof |
| Offer framing | Free trial vs demo vs case study |
| CTA copy | "Start free" vs "See how it works" vs "Get a demo" |
| Social login | Google only vs Google + LinkedIn vs email only |
| Form length | 2 fields vs 4 fields vs 6 fields |

Testing multiple dimensions simultaneously makes it impossible to know which change caused the result.

## Implementation Checklist

- [ ] Hypothesis written using the template
- [ ] Primary, secondary, and guardrail metrics defined
- [ ] Sample size calculated before test starts
- [ ] Test duration set (minimum 1 business week, typically 2)
- [ ] Traffic split set (50/50 unless risk management requires smaller variant)
- [ ] Segmentation applied if testing on a specific audience
- [ ] Test locked — results reviewed only at end date
- [ ] Statistical method (frequentist / Bayesian) chosen and documented

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "We'll stop when it's clearly winning" | Peeking inflates false positive rate to ~40%. Set an end date. |
| "Let's test 5 things at once to go faster" | You can't isolate causation. One thing at a time. |
| "We don't need a hypothesis — let's just see what happens" | Without a hypothesis, you can't distinguish signal from noise. |
| "Statistical significance is enough" | Practical significance matters too. A 0.1% lift that's statistically significant may not be worth shipping. |
| "Our traffic is too low to A/B test" | Then use qualitative methods (user testing, session recording, surveys) until you have enough traffic. |

## Verification

- [ ] Written hypothesis exists before test launch
- [ ] Sample size calculated and end date set
- [ ] One primary metric, secondary metrics, and at least one guardrail metric defined
- [ ] No early peeking — results reviewed only at end date
- [ ] Statistical method documented and appropriate for the decision type
- [ ] Winning variant shipped within 1 week of test conclusion (or test invalidated)
