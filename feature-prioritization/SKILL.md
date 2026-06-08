---
name: feature-prioritization
description: Use when the user wants to decide what to build next, rank features, cut scope, or make trade-offs between competing requests. Also use when the user mentions 'what should we build,' 'prioritization,' 'RICE,' 'ICE,' 'backlog grooming,' 'MVP scope,' 'feature ranking,' 'what to cut,' or 'MoSCoW.'
---

# Feature Prioritization

Prioritization is not about ranking what the loudest voice asked for. It is about maximizing value delivered per unit of effort against a defined outcome.

## RICE Scoring

Score each candidate feature on four dimensions:

| Dimension | Definition | Scale |
|---|---|---|
| **Reach** | How many users affected per quarter? | Raw number |
| **Impact** | How much does it move the target metric per user? | 3=massive / 2=high / 1=medium / 0.5=low / 0.25=minimal |
| **Confidence** | How confident in reach and impact estimates? | 100% / 80% / 50% |
| **Effort** | Person-months to build? | Raw number (lower = better) |

**RICE score = (Reach × Impact × Confidence) / Effort**

Higher score = higher priority.

## ICE Scoring (Faster Variant)

For rapid prioritization without hard data:

| Dimension | Question | Score 1-10 |
|---|---|---|
| **Impact** | How much does this move the goal if it works? | — |
| **Confidence** | How confident are we it will work? | — |
| **Ease** | How easy is it to implement? | — |

**ICE score = (Impact + Confidence + Ease) / 3**

ICE is a gut-check tool. RICE is a data tool. Don't confuse them.

## Kano Model

Classify features before scoring:

- **Must-haves** — Expected. No delight if present; strong dissatisfaction if absent.
- **Performance** — More = better. Direct correlation with satisfaction.
- **Delighters** — Not expected but create delight. No dissatisfaction if absent.

**Rule:** Ship all must-haves first. Then performance features. Then delighters.

## MoSCoW for Scope-Cutting

| Category | Definition |
|---|---|
| **Must have** | MVP fails without this |
| **Should have** | High value, can launch without if pressed |
| **Could have** | Nice to have, first to cut |
| **Won't have** | Explicitly out of scope this cycle |

MoSCoW works best when "Won't have" is as long as "Must have."

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "The customer asked for it" | Customer requests are signals, not specifications. Score them like everything else. |
| "This is a quick win" | "Quick" is effort, not impact. Score impact first — low-effort, low-impact work is still waste. |
| "We should do everything on the list" | Every yes is a no to something else. The list must have a cut line. |
| "RICE is too complex" | Show the scoring. Invisible prioritization produces invisible trade-offs. |

## Verification

- [ ] Every candidate feature scored on at least one framework
- [ ] Must-haves separated before any scoring
- [ ] Cut line established — items below it are explicitly parked
- [ ] Scoring assumptions documented
- [ ] Roadmap reflects final prioritization, not feature-request log
