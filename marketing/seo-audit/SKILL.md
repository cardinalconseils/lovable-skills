---
name: seo-audit
description: Use when the user wants to audit, review, or diagnose SEO issues on their site. Also use when the user mentions 'SEO audit,' 'technical SEO,' 'why am I not ranking,' 'SEO issues,' 'on-page SEO,' 'site audit,' or 'how do I improve my Google rankings.'
---

# SEO Audit

Expert knowledge for diagnosing technical SEO issues, on-page problems, and content gaps that prevent sites from ranking.

## Audit Framework

```
1. Crawlability → 2. Indexation → 3. On-Page → 4. Content → 5. Authority
```

Don't optimize content on pages that aren't being crawled correctly.

## Layer 1: Crawlability

**robots.txt** — Check for blocked URLs that should be indexed (`/blog/`, `/pricing/`). `Disallow: /` blocks the entire site — catastrophic.

**XML Sitemap** — Must exist at `/sitemap.xml`, all important URLs included, no noindex URLs, submitted to Google Search Console.

**Core Web Vitals:**

| Metric | Good | Needs Work | Poor |
|---|---|---|---|
| LCP (Largest Contentful Paint) | < 2.5s | 2.5-4s | > 4s |
| INP (Interaction to Next Paint) | < 200ms | 200-500ms | > 500ms |
| CLS (Cumulative Layout Shift) | < 0.1 | 0.1-0.25 | > 0.25 |

**Common CWV fixes:**
- LCP: Compress images to WebP; add `fetchpriority="high"` to hero image
- CLS: Add width/height attributes to images
- INP: Defer non-critical JavaScript

## Layer 2: Indexation

**GSC Coverage report:**
- "Crawled, currently not indexed" → thin content, duplicate content, or low quality signal
- "Discovered, currently not indexed" → low PageRank signal, crawl budget issues
- "Duplicate, submitted URL not selected as canonical" → Google chose a different canonical

**Canonical Tags:** Present on every page, pointing to the correct URL, consistent form (www vs non-www, trailing slash).

## Layer 3: On-Page SEO

**Title Tags:** Unique on every page, primary keyword near front, 50-60 characters.

**Meta Descriptions:** Unique, 150-160 characters, includes CTA and primary keyword.

**H1 Tags:** One H1 per page, contains primary keyword, different from title tag.

**Internal Linking:** All important pages have internal links. No orphaned pages. Descriptive anchor texts.

## Layer 4: Content Audit

**E-E-A-T Signals:**
- Experience: Author bylines with relevant experience, first-hand accounts, original data
- Expertise: Author credentials, depth and accuracy
- Authoritativeness: Backlinks from authoritative sites
- Trustworthiness: HTTPS, clear privacy policy, accurate content

**Keyword Cannibalization:** Multiple pages targeting same keyword confuses Google. Consolidate or clearly differentiate intent.

## Layer 5: Authority

**Key metrics:** Domain Rating (Ahrefs DR), total referring domains, anchor text distribution, lost backlinks.

**Toxic link signals:** Links from link farms, sites with DR < 10 and no organic traffic, sudden spike from unrelated foreign-language sites.

## Audit Report Structure

**Critical (fix within 1 week):** Crawl blocks, whole-site noindex, wrong canonical, 5xx errors.

**High (fix within 1 month):** CWV fails, duplicate title tags, missing or duplicate H1s, key pages not indexed.

**Medium (fix within quarter):** Missing meta descriptions, thin content, missing alt text, broken internal links.

**Low (optimize when possible):** Suboptimal title lengths, missing schema markup, image size optimization.

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "We rank for our brand — SEO is fine" | Brand rankings are easy. Non-branded organic traffic is the revenue driver. |
| "We did an SEO audit 2 years ago" | Algorithm updates, site changes, and competitive shifts require annual audits at minimum. |
| "We can't fix Core Web Vitals — it requires a rebuild" | Most CWV improvements are targeted changes (image optimization, JS deferral), not full rebuilds. |

## Verification

- [ ] robots.txt checked for unintended blocks
- [ ] XML sitemap submitted to GSC
- [ ] GSC Coverage report reviewed
- [ ] Core Web Vitals checked via PageSpeed Insights
- [ ] Canonical tags verified on all key pages
- [ ] Title tags verified: unique, 50-60 chars, keyword-containing
- [ ] H1 tags verified: one per page
- [ ] Orphaned pages identified
- [ ] Audit findings organized into Critical/High/Medium/Low
