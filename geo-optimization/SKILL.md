---
name: geo-optimization
description: Use when the user wants their brand, product, or content to be cited by AI systems like ChatGPT, Perplexity, Claude, Gemini, or other LLM-powered tools. Also use when the user mentions 'GEO,' 'generative engine optimization,' 'AI citations,' 'getting mentioned by AI,' 'LLM visibility,' 'Perplexity ranking,' 'ChatGPT mentions,' 'AI search,' or 'being cited in AI answers.'
---

# Generative Engine Optimization (GEO)

GEO is the practice of making your brand, product, and content visible to large language models so they cite you in generated responses. Where AEO targets Google's answer boxes, GEO targets the outputs of ChatGPT, Perplexity, Claude, Gemini, and other AI systems that synthesize answers from training data and live retrieval.

## How LLMs Decide What to Cite

LLMs have two knowledge sources:

1. **Training data** — What was in the corpus when the model was trained. High-authority, widely-linked, frequently-cited content gets baked in.
2. **Retrieval (RAG)** — What the model fetches at query time via web search (Perplexity, Bing Copilot, ChatGPT with browsing). Freshness and relevance matter here.

**GEO strategy must address both:** baked-in authority (training data) and real-time retrievability (RAG).

## The Five GEO Signals

1. **Citation density** — Are you cited by authoritative sources covering your topic? Backlinks from trusted publications are training data signals.
2. **Entity consistency** — Is your brand name, product name, and key claims stated consistently across the web? LLMs build entity representations from consistent co-occurrence.
3. **Topical authority** — Do you cover your topic comprehensively and deeply? LLMs favor sources that are authoritative on a topic, not just mentioning it once.
4. **Quotability** — Do you publish specific, citable claims? Data, frameworks, definitions, and named methodologies are more citable than general prose.
5. **Freshness** — For RAG-based systems, recent publication and updated content ranks higher.

## llms.txt

A new emerging standard: `llms.txt` at the root of your domain tells LLMs what your site is about and what to index.

```
# Your Company Name

> One-line description of what you do and who you serve.

## Key Pages
- [About](https://yoursite.com/about): Company overview, mission, leadership
- [Product](https://yoursite.com/product): What the product does and for whom
- [Docs](https://yoursite.com/docs): Full documentation
- [Blog](https://yoursite.com/blog): Thought leadership and case studies

## Key Claims
- [Claim 1]: [Evidence URL]
- [Statistic]: [Source URL]
```

Not yet universally adopted, but Perplexity and some crawlers already use it.

## Content Strategies for GEO

### Original Data and Research
Publish original statistics, surveys, or studies. LLMs cite data sources heavily. "X% of [segment] do Y" with your company as the source = recurring citation.

### Named Frameworks
Create and name your own frameworks. "The [Your Brand] Method" or "[Your Concept] Framework." Named frameworks become citable entities.

### Definition Pages
Own the definition of key terms in your category. "What is [category term]?" pages with clear, citable definitions get referenced when LLMs explain the concept.

### Comparison and Category Pages
LLMs synthesize comparisons constantly. Be present in "best [category]" and "[product] vs [product]" content.

## Entity Establishment

LLMs build mental models of entities (brands, people, concepts). Establish your entity across:

- **Wikipedia:** If eligible, a Wikipedia page is the highest-weight entity signal
- **Wikidata:** Structured entity data consumed by many AI systems
- **Google Knowledge Panel:** Claim and verify your Knowledge Panel
- **Industry databases:** Crunchbase, G2, Capterra, Product Hunt
- **Authoritative mentions:** Press coverage in recognized publications
- **Consistent NAP:** Same name, description, and key claims everywhere

## Prompt-Matched Content Strategy

Research the exact prompts people use to ask about your category in AI tools:

1. Ask ChatGPT/Perplexity: "What are the best [your category] tools?"
2. Note which sources are cited
3. Analyze what those sources have that you don't
4. Create content that answers the exact prompts where you're absent

## Perplexity-Specific Optimization

Perplexity uses live web retrieval. Visibility tactics:
- Fresh content (published or updated in last 30-90 days)
- Clear, direct answers to specific questions (same as AEO)
- Technical accuracy — Perplexity sources are shown to users who will verify
- H2/H3 structure so the retrieval layer can extract specific sections

## Measuring GEO Performance

| Metric | How to measure |
|---|---|
| AI citation rate | Manual prompt testing in ChatGPT, Perplexity, Claude weekly |
| Brand mention in AI answers | Tools: Profound, Brandwatch AI mentions, Share of Voice |
| Topical authority | Google Search Console: impressions for target topic queries |
| Citation sources | Ahrefs/SEMrush: referring domain authority for your key topic pages |

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "LLMs train on the whole web, I can't influence that" | You can influence citation density, entity consistency, and topical authority — all of which affect training data representation. |
| "GEO is too new to invest in" | AI search is growing 40%+ YoY. Early movers in a new channel get disproportionate returns. |
| "Our SEO content already covers this" | SEO content is optimized for keyword matching. GEO content is optimized for citation and entity establishment. Often different. |
| "We don't know if this works yet" | Prompt-test your brand name in ChatGPT and Perplexity today. You'll immediately know your current GEO baseline. |

## Verification

- [ ] Brand prompt-tested in ChatGPT, Perplexity, and Google AI Overviews — baseline documented
- [ ] llms.txt file created and published at domain root
- [ ] Entity established on Wikidata, Crunchbase, G2/Capterra (minimum)
- [ ] At least one original data/research asset published and promoted
- [ ] Named framework or methodology created and consistently referenced
- [ ] Definition page exists for primary category term
- [ ] Content calendar includes freshness updates for top GEO target pages
