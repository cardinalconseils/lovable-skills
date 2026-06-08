---
name: signup-flow-cro
description: Use when the user wants to optimize their signup, registration, or trial activation flow. Also use when the user mentions 'signup conversions,' 'signup drop-off,' 'onboarding optimization,' 'trial activation,' 'registration friction,' or 'how do I get more people through my signup.'
---

# Signup Flow & Trial Activation CRO

Expert knowledge for optimizing signup, registration, and trial activation flows.

## The Two Goals of a Signup Flow

1. **Get them in:** Remove every barrier between intent and account creation
2. **Get them activated:** Guide them to the moment where the product's value becomes real

Most teams optimize for goal 1 and ignore goal 2. Activation is where retention starts.

## Signup Flow Audit

- How many steps from CTA click to active session?
- How many form fields total?
- Is email verification required before product access?
- What is the "aha moment" — the first moment of real value?
- How many steps separate signup from the aha moment?

Measure drop-off at each step. The highest drop-off step is your first priority.

## Field Reduction

**What to ask at signup (strict minimum):**
- Email address
- Password (or offer social login to skip this entirely)

**What NOT to ask at signup:** Full name, company name, phone number, role or job title, company size or industry.

**Progressive disclosure:** Request additional fields at the moment they become necessary, not upfront.

## Social Login

Social login (Google, LinkedIn, Microsoft) reduces signup friction by 50-70%.

- For B2B: Prioritize Google and Microsoft (work accounts)
- Position "Sign up with Google" as the primary CTA
- Keep email+password as a fallback, not the default

## Email Verification Friction

Traditional email verification loses 20-40% of signups.

**Better approaches:**
- **Access first, verify later:** Immediate access; gate only specific features behind verification
- **Magic link:** User enters email → receives magic link → clicks → signed in
- **Social login:** OAuth = pre-verified email

## Progress Indicators

- Show current step number and total ("Step 2 of 4")
- Don't show progress on the first step
- Don't exceed 5 steps for any signup flow

## The Activation Milestone

Activation = the moment the user experiences the core value of your product.

**Define precisely:** Not "logged in for the second time" — specific to the product's value: "created their first report," "connected their first integration."

**Test your definition:**
- Does it correlate with 90-day retention?
- Do activated users churn at < 2× the rate of non-activated users?
- Is activation achievable in a single session?

## Onboarding Email Sequence (First 7 Days)

| Email | When | Goal |
|---|---|---|
| Welcome | Immediately | Confirm account, set expectation |
| Activation | Day 1 | Drive first key action if not completed |
| Value reminder | Day 2 | Remind why they signed up + success tip |
| Social proof | Day 3 | Case study from similar user |
| Check-in | Day 5 | Did they hit activation? Offer help if not |
| Feature discovery | Day 7 | If activated: introduce next value layer |

**Behavioral triggers beat time-based:** If user completes activation, skip Day 1 email and send value reminder instead.

## A/B Testing Priority

1. Social login vs email+password (highest impact)
2. Number of form fields
3. CTA button copy
4. Email verification timing
5. Empty state design

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "We need all those fields to qualify leads" | Enrich after signup. Users won't fill 8 fields when your competitor asks for 2. |
| "Email verification is required for security" | Immediate access with background verification maintains security while reducing drop-off by 20-40%. |
| "Our signup is already one page — it's simple" | One page with 10 fields is not simple. Count fields, not pages. |

## Verification

- [ ] Drop-off measured at each step in the signup flow
- [ ] Form fields reduced to strict minimum
- [ ] Social login implemented (Google minimum for B2B)
- [ ] Email verification deferred until after first product use
- [ ] Progress indicator present for multi-step flows
- [ ] Inline validation implemented on all form fields
- [ ] Activation milestone defined with retention correlation confirmed
- [ ] 7-day onboarding email sequence with behavioral triggers configured
