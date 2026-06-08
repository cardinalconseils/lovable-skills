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

Work through in order — technical issues block all subsequent gains.

## Layer 1: Crawlability

**robots.txt:** Check `/robots.txt` for blocked URLs that should be indexed. `Disallow: /` blocks the entire site — a catastrophic error.

**XML Sitemap:** Must exist at `/sitemap.xml`, include all important URLs, no noindex URLs, submitted to GSC.

**Core Web Vitals:**

| Metric | Good | Needs Work | Poor |
|---|---|---|---|
| LCP | < 2.5s | 2.5-4s | > 4s |
| INP | < 200ms | 200-500ms | > 500ms |
| CLS | < 0.1 | 0.1-0.25 | > 0.25 |

**Common CWV fixes:** Compress images to WebP, add `fetchpriority="high"` to hero image, defer non-critical JS, add `font-display: swap`.

## Layer 2: Indexation

**GSC Coverage report:**
- "Crawled, currently not indexed" → thin content, duplicate content, or low quality signal
- "Discovered, currently not indexed" → low PageRank signal, crawl budget issues
- "Duplicate, submitted URL not selected as canonical" → Google chose a different canonical

**Duplicate content:** URL parameter duplication, HTTP/HTTPS duplication, www/non-www duplication. Pick one; redirect the other.

## Layer 3: On-Page SEO

**Title tags:** Unique on every page, contains primary keyword near the front, 50-60 characters.

**H1 tags:** One H1 per page, contains primary keyword, different from the title tag.

**Internal linking:** All important pages have internal links, no orphaned pages, descriptive anchor texts.

## Layer 4: Content (E-E-A-T)

- **Experience:** Author bylines with relevant experience, first-hand accounts, original data
- **Expertise:** Author credentials, depth and accuracy, citations of reputable sources
- **Authoritativeness:** Backlinks from authoritative sites, brand mentions, press coverage
- **Trustworthiness:** HTTPS, clear privacy policy, accurate up-to-date content

**Keyword cannibalization:** Multiple pages targeting the same keyword confuses Google. Consolidate into one page or clearly differentiate intent.

## Layer 5: Authority

**Key metrics:** Domain Rating (Ahrefs), total referring domains, anchor text distribution, lost backlinks.

**Toxic link signals:** Link farms, sites with DR < 10 and no organic traffic, sudden spikes from unrelated foreign sites.

## Audit Report Structure

**Critical (1 week):** Crawl blocks, site-wide noindex, wrong canonical, 5xx errors.

**High (1 month):** CWV fails, duplicate titles, missing/duplicate H1s, key pages not indexed.

**Medium (quarter):** Missing meta descriptions, thin content, missing alt text, broken internal links.

**Low (when possible):** Suboptimal title lengths, missing schema markup, image size optimization.

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "We rank for our brand — SEO is fine" | Brand rankings are easy. Non-branded organic traffic is the revenue driver. |
| "We did an SEO audit 2 years ago" | Algorithm updates and site changes require annual audits at minimum. |
| "Our developer handles technical SEO" | Developers fix technical issues; they don't diagnose SEO strategy. |
| "CWV requires a site rebuild" | Most CWV improvements are targeted changes, not full rebuilds. |

## Verification

- [ ] robots.txt checked for unintended blocks
- [ ] XML sitemap submitted to GSC
- [ ] GSC Coverage report reviewed
- [ ] Core Web Vitals checked via PageSpeed Insights
- [ ] Canonical tags verified on all key pages
- [ ] Title tags: unique, 50-60 chars, keyword-containing
- [ ] H1 tags: one per page, keyword-containing
- [ ] Orphaned pages identified
- [ ] Backlink profile checked for toxic links
