---
name: analytics-tracking
description: Use when the user wants to set up, audit, or improve their analytics and tracking. Also use when the user mentions 'analytics,' 'tracking,' 'GA4,' 'Google Analytics,' 'GTM,' 'events,' 'conversions,' 'pixels,' 'UTMs,' 'attribution,' or 'what data should we track.'
---

# Analytics Tracking

Expertise in instrumentation that connects user behavior to business outcomes. Bad tracking is noise. Good tracking answers: where do users drop off, which channels actually convert, and what does activation look like before churn?

## Event Priority by Product Stage

**MVP/Prototype:** Instrument `sign_up`, `page_view`, and one activation event specific to the product.

**Early Users/Pilot:** Add `feature_used` for each key feature and `upgrade_intent`.

**Growth/Candidate:** Full event taxonomy. Add `churn_signal`.

**Production:** Add server-side events for purchase confirmation.

## Core Event Taxonomy

### Universal Events

| Event | When to fire | Key properties |
|---|---|---|
| `page_view` | Every page load | page_path, page_title, referrer |
| `sign_up` | Account created | method (google/email/github), plan |
| `login` | User authenticates | method |
| `sign_out` | User logs out | session_duration |

### Activation Events (Pilot stage+)

| Event | When to fire | Key properties |
|---|---|---|
| `feature_used` | First time user uses a key feature | feature_name, user_id |
| `onboarding_complete` | User finishes setup flow | steps_completed, time_to_complete |
| `aha_moment` | User reaches the product's core value | specific to product |
| `upgrade_intent` | User views pricing/upgrade page | source, plan_viewed |

### Revenue Events (Candidate stage+)

| Event | When to fire | Key properties |
|---|---|---|
| `purchase` | Successful payment | revenue, currency, plan, billing_cycle |
| `subscription_started` | Trial converts to paid | plan, trial_duration |
| `subscription_cancelled` | User cancels | plan, reason, tenure |
| `churn_signal` | User hasn't activated in N days | days_inactive, plan |

## GTM vs Direct Implementation

**Without GTM:** Every new pixel means a code deploy.
**With GTM:** New pixel = 10-minute GTM configuration, no code change.

**Setup order:**
1. Install GTM snippet (one code deploy)
2. Route all events through `dataLayer.push()`
3. Add destinations (GA4, Meta Pixel, LinkedIn Insight Tag) as GTM tags

```javascript
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  event: 'sign_up',
  method: 'google',
  plan: 'free'
});
```

## GA4 Setup

- [ ] GA4 property created and connected to GTM
- [ ] Enhanced measurement enabled
- [ ] Conversion events marked (sign_up, purchase, activation event)
- [ ] Data retention set to 14 months (default is 2 months — change immediately)
- [ ] Audiences created (signed in users, paying customers, churned users)

## Pixel Setup by Platform

### Meta Pixel
- `PageView` — fires on every page
- `Lead` — fires on lead magnet download or demo request
- `CompleteRegistration` — fires on account creation
- `Purchase` — fires on successful payment (with value and currency)

**Conversions API (CAPI):** Implement server-side events for purchases. Browser-blocking loses 15-30% of conversion events.

### Google Ads Tag
Import conversions from GA4 rather than implementing a separate tag.

### LinkedIn Insight Tag
Required for B2B. Powers demographic reporting and retargeting audiences.

## UTM Parameter Strategy

```
?utm_source=[source]&utm_medium=[medium]&utm_campaign=[campaign]&utm_content=[content]&utm_term=[term]
```

**Naming convention:** snake_case, lowercase, no spaces.

| Field | What it tracks | Examples |
|---|---|---|
| `utm_source` | Where traffic comes from | `google`, `linkedin`, `newsletter` |
| `utm_medium` | Marketing channel type | `cpc`, `email`, `organic_social` |
| `utm_campaign` | Specific campaign name | `q1_lead_gen`, `product_launch` |
| `utm_content` | Ad variant or link variant | `hero_cta`, `footer_link` |
| `utm_term` | Keyword (paid search) | `project_management_software` |

## Attribution Models

**Last-click:** All credit to the last touchpoint. Use at MVP stage.
**Data-driven (GA4 default):** Requires 400+ conversions/30 days. Use at Growth stage+.
**First-click:** Useful for understanding acquisition channels.

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "We'll add tracking after launch" | You lose launch-day data forever. |
| "We just need Google Analytics" | GA4 alone is fine at MVP. At Pilot you need conversion events. |
| "UTMs are too much overhead" | One UTM naming convention doc prevents months of broken attribution. |
| "Server-side tracking is too complex" | Browser-side tracking loses 15-30% of conversions to ad blockers at Candidate stage. |

## Verification

- [ ] GTM installed and dataLayer events confirmed firing (GTM Preview mode)
- [ ] GA4 receiving events (DebugView shows events in real time)
- [ ] Conversion events marked in GA4
- [ ] Meta Pixel verified in Events Manager
- [ ] UTM naming convention documented and shared with team
- [ ] Data retention set to 14 months in GA4
- [ ] Server-side CAPI implemented for purchase events (if running Meta ads)
