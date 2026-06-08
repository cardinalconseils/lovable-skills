---
name: programmatic-seo
description: Use when the user wants to build programmatic SEO pages, template-driven content at scale, location pages, comparison pages, or data-driven landing pages. Also use when the user mentions 'programmatic SEO', 'pSEO', 'template pages', 'location pages', 'scalable SEO', 'data-driven pages', or 'SEO at scale'.
---

# Programmatic SEO

Expert knowledge for building template-driven pages at scale that rank for long-tail keywords — without thin content penalties.

## What Programmatic SEO Is

pSEO = a template + a data source = N pages, where N is large and each page targets a specific long-tail keyword cluster.

**The constraint:** Each page must provide enough unique value to earn a ranking. Google's helpful content system penalizes pages that exist only to capture query volume without serving user intent.

## Keyword Patterns That Work

| Pattern | Example | Data Source |
|---|---|---|
| [Tool] for [Use Case] | "Notion for project management" | Product categories |
| [Tool] vs [Tool] | "Webflow vs Framer" | Competitor pairs |
| Best [Category] in [Location] | "Best accountants in Montreal" | Business directory |
| [Category] for [Industry] | "CRM for real estate" | Industry list |
| How to [Task] in [Tool] | "How to export CSV in Airtable" | Feature × tool matrix |
| [Job Title] tools | "Tools for growth marketers" | Job title taxonomy |
| [Adjective] [Category] | "Free email marketing tools" | Attribute × category |

**Research method:** Use Google Autocomplete, People Also Ask, and keyword tools to validate search volume before building. A pattern with < 100 monthly searches per variant is rarely worth the infrastructure.

## Data Source Types

| Type | Example | Freshness need |
|---|---|---|
| Internal product data | Feature list, pricing, integrations | Real-time or daily sync |
| Public database | Crunchbase, Census, OpenStreetMap | Monthly refresh |
| Scraped / licensed | G2 reviews, Trustpilot ratings | Weekly |
| User-submitted | Directory listings, job board | Real-time |
| Editorial | Curated list with descriptions | Manual, quarterly |

**Quality signal:** The more proprietary and fresh your data, the stronger the moat. Anyone can build on public data. Proprietary data is defensible.

## The 60/40 Rule

**60% template:** Structure, navigation, calls-to-action, meta tags, schema markup.
**40% unique content:** At minimum, 40% of each page's content must be unique to that page.

Thin content threshold: pages under ~300 words with > 80% templated text are at risk of manual action or algorithmic demotion.

**Unique content strategies:**
- Dynamic statistics pulled from your data source
- User reviews or ratings specific to that variant
- Editorially written introductions for high-volume variants
- Auto-generated comparisons or summaries derived from structured data
- "Last updated" timestamps showing freshness

## Thin Content Avoidance

Signals that trigger thin content penalties:
- Identical or near-identical meta descriptions across pages
- Body text that only changes the keyword
- No original data, analysis, or editorial perspective
- Pages indexed faster than they can be reviewed (rapid mass publication)

**Safe publication cadence:** For new pSEO sites, publish 50–100 pages initially, monitor indexation and ranking for 30 days, then scale. Don't publish 10,000 pages on day one.

## Canonicalization

- Every page: self-canonical (`<link rel="canonical" href="[this page's URL]"/>`)
- Faceted/filtered variants: canonical points to the root template page
- Duplicate content (e.g., location A and location B with identical content): use noindex on the weaker page or consolidate

## Internal Linking Strategy

pSEO pages are typically thin on authority. Internal links from high-authority pages pass PageRank.

**Linking patterns:**
1. Hub-and-spoke: Create a category hub page that links to all variants
2. Comparison clusters: Each vs. page links to the two individual tool pages
3. Blog to pSEO: In-content links from blog posts to relevant pSEO pages
4. Breadcrumbs: Structural breadcrumbs on every pSEO page (also add BreadcrumbList schema)

**Rule:** No pSEO page should exist with zero internal links pointing to it from your site.

## Page Quality Scoring

Score each page variant before publishing:

```
Quality Score = (Unique Content % × 0.4)
              + (Data Freshness Score × 0.3)
              + (User Signals Proxy × 0.2)
              + (Internal Link Count × 0.1)
```

Pages scoring < 0.5 should be revised or held from indexing.

## Phased Indexation

| Phase | Action |
|---|---|
| 1. Seed (50–100 pages) | Publish, submit to Search Console, monitor for indexation |
| 2. Assess (30 days) | Check: % indexed, ranking positions, click-through rate |
| 3. Optimize | Improve lowest-quality pages before scaling |
| 4. Scale | Publish remaining variants in batches of 500–1,000 |
| 5. Maintain | Monthly freshness update on all pages (update dates, refresh data) |

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "More pages = more traffic" | Thin pages dilute crawl budget and trigger algorithmic penalties. Quality > quantity. |
| "We'll add unique content later" | Thin content that gets indexed as thin stays thin. Build the 40% unique content into the template from day one. |
| "All our location pages say the same thing with the city name swapped" | That's a thin content penalty waiting to happen. Each location needs genuinely different data. |
| "We published 10,000 pages in a week and nothing is ranking" | You overwhelmed Google's quality assessment. Phased indexation exists for this reason. |

## Verification

- [ ] Keyword pattern validated with search volume data before build
- [ ] Data source identified and refresh cadence documented
- [ ] 40% unique content minimum per page variant confirmed
- [ ] Self-canonical tag on every page
- [ ] No pSEO page exists with zero internal links
- [ ] BreadcrumbList schema implemented on all variants
- [ ] Phased indexation plan: seed cohort published, 30-day monitoring before scale
- [ ] Page quality score calculated and pages < 0.5 held from indexing
