---
name: signup-flow-cro
description: Use when the user wants to optimize their signup, registration, or trial activation flow. Also use when the user mentions 'signup conversions,' 'signup drop-off,' 'onboarding optimization,' 'trial activation,' 'registration friction,' or 'how do I get more people through my signup.'
---

# Signup Flow & Trial Activation CRO

Expert knowledge for optimizing signup, registration, and trial activation flows.

## The Two Goals

1. **Get them in:** Remove every barrier between intent and account creation
2. **Get them activated:** Guide them to the moment where value becomes real

Most teams optimize for goal 1 and ignore goal 2. Activation is where retention starts.

## Signup Flow Audit

- How many steps from CTA click to active session?
- How many form fields total?
- Is email verification required before product access?
- What is the first screen after signup?
- What is the aha moment?
- How many steps separate signup from the aha moment?

Measure drop-off at each step. The step with the highest drop-off is your first priority.

## Field Reduction

**Ask at signup (strict minimum):**
- Email address
- Password (or social login to skip this)

**Do NOT ask at signup:**
- Full name, company name, phone number, role, company size, industry

Request additional fields at the moment they become necessary, not upfront.

## Social Login

Social login removes the password field and reduces friction by 50-70%.

- For B2B: Prioritize Google and Microsoft (work accounts)
- Position "Sign up with Google" as the primary CTA, not secondary
- OAuth benefits: verified email, often provides name + photo, fewer forgotten passwords

## Email Verification Friction

Traditional email verification loses 20-40% of signups.

**Better approaches:**
- **Access first, verify later:** User accesses product immediately; gate only specific features
- **Magic link:** Email → magic link → signed in. Eliminates password creation.
- **Social login:** OAuth = pre-verified email.

## The Activation Milestone

Activation = the moment the user experiences the core value.

**Define precisely:**
- Not "logged in for the second time"
- Specific to product value: "created their first report," "connected their first integration"

**Test:** Does it correlate with 90-day retention? Do activated users churn at < 2× the rate?

If activation is not achievable in a single session, users churn before they see the value. Redesign.

## Empty State Design

- Show what it will look like when full
- One clear action ("Create your first [X]")
- Pre-fill defaults, use templates
- Describe the outcome, not the action

## Onboarding Email Sequence (First 7 Days)

| Email | When | Goal |
|---|---|---|
| Welcome | Immediately | Confirm account, set expectation |
| Activation | Day 1 | Drive first key action if not completed |
| Value reminder | Day 2 | Remind why they signed up + tip |
| Social proof | Day 3 | Case study from similar user |
| Check-in | Day 5 | Did they activate? Offer help if not |
| Feature discovery | Day 7 | If activated: introduce next value layer |

Behavioral triggers beat time-based triggers.

## A/B Testing Priority Order

1. Social login vs email+password (highest impact)
2. Number of form fields
3. CTA button copy
4. Email verification timing
5. Empty state design

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "We need all those fields to qualify leads" | Enrich after signup. Users won't fill 8 fields when your competitor asks for 2. |
| "Email verification is required for security" | Access first + background verification maintains security while reducing drop-off 20-40%. |
| "Our signup is already one page — it's simple" | One page with 10 fields is not simple. Count fields, not pages. |
| "Onboarding emails are annoying" | Users who don't activate churn. Activation emails are valuable, not annoying. |

## Verification

- [ ] Drop-off measured at each step
- [ ] Form fields reduced to strict minimum
- [ ] Social login implemented (Google minimum for B2B)
- [ ] Email verification deferred until after first product use
- [ ] Progress indicator for multi-step flows (> 2 steps)
- [ ] Inline validation on all form fields
- [ ] Activation milestone defined with retention correlation confirmed
- [ ] Empty state has single clear action with outcome-oriented copy
- [ ] 7-day onboarding email sequence with behavioral triggers configured
