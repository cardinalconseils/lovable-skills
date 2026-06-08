---
name: activation-strategy
description: Use when the user wants to define the aha moment, reduce time to first value, improve new user onboarding, or increase the percentage of signups who become active users. Also use when the user mentions 'activation,' 'aha moment,' 'time to value,' 'onboarding flow,' 'new user experience,' 'first session,' or 'users signing up but not doing anything.'
---

# Activation Strategy

Activation is the moment a new user experiences the core value of your product for the first time. Everything before activation is cost. Everything after activation is value. The job of the onboarding experience is to reach activation as fast as possible.

## Defining Your Aha Moment

The aha moment is the specific action that correlates most strongly with long-term retention.

**How to find it:**
1. Take a cohort of users who are still active after 90 days
2. Find the action(s) they all completed in their first 7 days
3. Compare against users who churned — what did churned users NOT do?
4. That gap = your aha moment

**Good aha moment definitions:**
- "Created their first campaign and saw results data" (Specific action + outcome)
- "Connected 2+ integrations" (Specific action with threshold)
- "Invited a teammate who logged in" (Social action with confirmation)

**Bad aha moment definitions:**
- "Logged in twice" (Not value-delivering)
- "Completed their profile" (Setup, not value)
- "Visited the dashboard" (Passive, not active)

## Activation Funnel

Map every step from signup to aha moment. Measure drop-off at each:

```
Signup → Email verified → Profile created → First [core action] → Aha moment
100%      68%              54%              41%                    28%
```

The biggest drop is the highest-leverage fix. Start there, not at the top.

## Reducing Time to Value

**Tactics by barrier type:**

| Barrier | Tactic |
|---|---|
| Too many steps before value | Cut every step that isn't necessary for the first value moment |
| Blank state (nothing to interact with) | Pre-populate with sample data / templates |
| Required configuration before use | Offer sensible defaults; let users configure later |
| Email verification gate | Grant access immediately; verify in background |
| Complex setup | Interactive setup wizard with progress indicator |
| Users don't know what to do first | Single, prominent "start here" CTA in empty state |

## Progressive Onboarding

Teach features at the moment they become relevant — not all upfront.

- **Day 0:** Reach the aha moment. Nothing else.
- **Day 1-3:** Introduce the second most valuable feature
- **Day 3-7:** Introduce collaboration / sharing features
- **Day 7-14:** Surface power features for engaged users

Do not front-load a 12-step product tour before the user has experienced any value.

## Onboarding Checklist Pattern

A visible checklist of activation steps works because:
- Zeigarnik effect: unfinished tasks create psychological pull
- Progress visibility increases completion rates
- Clear next action eliminates "what do I do now?"

**Checklist rules:**
- Maximum 5 items
- Each item = one specific action, not a concept
- First item must be completable in < 2 minutes
- Show progress: 3/5 complete, not just checkboxes

## Behavioral Email Sequence

| Trigger | Email | Goal |
|---|---|---|
| Signup, aha not reached in 24h | Activation nudge | Drive the one missing action |
| Aha reached | Value reinforcement | Confirm they got the value, introduce next step |
| Day 3, aha not reached | Personal offer of help | Reduce friction with direct support offer |
| Day 7, still inactive | Re-engage or qualify | Determine if they're a fit or should be nurtured differently |

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "Our product is complex — onboarding takes time" | Complexity is a design problem. Every product has a first valuable action. Find it and make it fast. |
| "We have a product tour, that covers it" | Product tours tell users about features. Activation is doing the first valuable thing. These are different. |
| "Users should explore and find their own aha moment" | Users who don't find value quickly leave. Guide them. Exploration comes after activation, not before. |
| "Our aha moment requires setup" | Reduce the setup. Pre-fill data. Use templates. The setup is not the aha moment — the result is. |

## Verification

- [ ] Aha moment defined with a specific action + threshold, validated against 90-day retention data
- [ ] Activation funnel measured with drop-off at each step
- [ ] Highest drop-off step identified and improvement in progress
- [ ] Empty state design includes a single clear first action
- [ ] Behavioral email triggers set (not just time-based)
