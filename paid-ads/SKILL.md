---
name: paid-ads
description: Use when the user wants help with paid advertising campaigns on Google Ads, Meta (Facebook/Instagram), LinkedIn, or other ad platforms. Also use when the user mentions 'PPC,' 'paid social,' 'SEM,' 'paid media,' 'Google Ads,' 'Facebook Ads,' 'ad budget,' 'campaign setup,' or 'ROAS.'
---

# Paid Advertising

Expert knowledge for planning, launching, and optimizing paid advertising campaigns across Google, Meta, LinkedIn, and other platforms.

## Core Framework

```
1. Goal Definition → 2. Audience → 3. Offer & Creative → 4. Campaign Structure → 5. Bid Strategy → 6. Measurement → 7. Optimization
```

Never start with campaign structure. Start with goal and audience.

## Goal Definition

| Goal | Primary Metric | Platform Fit |
|---|---|---|
| Lead generation (B2B) | Cost per lead (CPL), MQL rate | Google Search, LinkedIn |
| Free trial signups | Cost per activation, CAC | Google Search, Meta |
| Brand awareness | CPM, reach, frequency | Meta, YouTube, LinkedIn |
| Retargeting / nurture | Cost per engagement | Meta, Google Display, LinkedIn |
| E-commerce sales | ROAS, revenue | Meta Shopping, Google Shopping |

For SaaS: track CAC against LTV. If LTV:CAC > 3:1, the channel is viable.

## Google Ads

### Campaign Types

| Type | Use For |
|---|---|
| Search | Bottom-of-funnel, high-intent buyers |
| Display | Retargeting, awareness |
| Performance Max | Google-automated cross-channel (use with caution) |
| YouTube | Video awareness and consideration |

### Match Type Strategy

- **Exact [keyword]:** Highest precision, lowest volume
- **Phrase "keyword":** Balance of precision and reach
- **Broad keyword:** Use only with smart bidding and good negative keyword lists

**Start with phrase and exact match.** Add broad only after phrase match proves ROI and you have 500+ conversions.

### Bidding Strategy

| Strategy | Use When |
|---|---|
| Manual CPC | Low conversion volume, tight control |
| Target CPA | Have 50+ conversions/month, stable CPA goal |
| Target ROAS | E-commerce with revenue data, 100+ conversions/month |
| Maximize Conversions | Launching a new campaign, want volume |

Smart bidding requires data. Don't use Target CPA until 30-50 conversions in past 30 days.

## Meta (Facebook / Instagram) Ads

### Audience Strategy

**Cold (prospecting):** Lookalike audiences, broad targeting + creative testing.

**Warm (retargeting):** Website visitors (30/60/180 days), video viewers, customer list exclusion.

**Sweet spot audience size:** 500K–3M for most B2B markets.

### Budget Allocation

| Funnel Stage | Budget % |
|---|---|
| Cold (prospecting) | 60-70% |
| Warm (retargeting) | 20-30% |
| Existing customer | 10% |

## LinkedIn Ads

Expensive (CPCs of $8-15) but unique targeting: job title, seniority, company size, ABM.

**Use when:** ICP is enterprise decision-makers, niche job titles, account-based marketing.

**LinkedIn Lead Gen Forms** pre-populate from LinkedIn profile — 2-5× higher conversion vs landing page. Keep to 3-4 fields max.

## ROAS and CAC Calculation

```
CAC = Total Ad Spend / Number of New Customers Acquired
ROAS = Revenue from Ads / Ad Spend
Payback Period = CAC / (MRR × Gross Margin)
```

**SaaS benchmarks:** CAC payback < 12 months self-serve, < 18 months enterprise. LTV:CAC > 3:1.

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "More campaigns = better coverage" | More campaigns means less data per campaign. Smart bidding needs concentration. |
| "We should test every audience and creative at once" | Scattered tests produce uninterpretable results. |
| "Our click-through rate is high" | CTR is a vanity metric without conversion data. |
| "We'll set it and forget it" | Paid ads require weekly optimization. |

## Verification

- [ ] Campaign goal defined with primary metric and target
- [ ] Audience defined before campaign built
- [ ] Negative keyword list built for Google Search
- [ ] Budget allocated across funnel stages
- [ ] Conversion tracking verified before going live
- [ ] Weekly optimization cadence defined
- [ ] LTV:CAC target set for channel viability
