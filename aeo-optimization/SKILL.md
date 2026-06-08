---
name: aeo-optimization
description: Use when the user wants to optimize their content to appear in AI-generated answers, voice search results, featured snippets, or knowledge panels. Also use when the user mentions 'AEO,' 'answer engine optimization,' 'featured snippets,' 'voice search,' 'Google SGE,' 'AI Overviews,' 'zero-click search,' 'position zero,' or 'getting into AI answers.'
---

# Answer Engine Optimization (AEO)

AEO is the practice of structuring content so that answer engines — Google AI Overviews, Bing Copilot, voice assistants, and AI chatbots — select it as the answer to a user's question. The goal is position zero: the answer box, not the blue link.

## What Answer Engines Look For

Answer engines extract content that is:

1. **Directly answerable** — The question is answered in the first sentence or paragraph, not buried
2. **Concise** — Featured snippets favor 40-60 word answers; voice search favors 29-word sentences
3. **Structured** — Lists, tables, and headers signal extractable information
4. **Authoritative** — High-trust domains, clear authorship, cited sources
5. **Semantically complete** — The full context of the question is addressed, not just the keyword

## Content Formats That Win AEO

| Format | Query type | Structure |
|---|---|---|
| **Definition paragraph** | "What is X?" | One sentence definition + 2-3 sentences of context |
| **Step list** | "How do I X?" | Numbered steps, each under 15 words |
| **Comparison table** | "X vs Y" | 3-5 row table with clear column headers |
| **FAQ block** | "What / Why / How / When" | H3 question + 40-60 word answer per item |
| **Statistic + source** | "How many / What percentage" | Data point + source citation in same sentence |

## AEO Page Structure

For any page targeting an answer query:

1. **H1 = the question** (or close variant): "What Is [Topic]?"
2. **First paragraph = the answer** (40-60 words, complete without reading further)
3. **Body = the depth** (supporting context, examples, nuance)
4. **FAQ section** (5-10 related questions with direct answers)
5. **FAQ schema markup** (JSON-LD — required for rich results)

## Schema Markup for AEO

**FAQ schema** — most impactful for answer box inclusion:
```json
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [{
    "@type": "Question",
    "name": "What is [topic]?",
    "acceptedAnswer": {
      "@type": "Answer",
      "text": "[40-60 word direct answer]"
    }
  }]
}
```

**Also implement:** `HowTo`, `Article`, `Organization`, `BreadcrumbList`

## Voice Search Optimization

Voice queries are longer, conversational, and question-based:
- Text: "best CRM software"
- Voice: "what's the best CRM software for a small sales team?"

**Voice answer requirements:**
- Answer fits in 1-2 spoken sentences (< 30 words)
- Starts with a direct response (not "great question")
- Uses natural spoken language, not keyword-stuffed prose
- Page has a high domain authority (voice heavily favors established domains)

## Google AI Overviews (SGE)

AI Overviews synthesize multiple sources. To be included:

- Cover the topic comprehensively — single narrow articles are less likely to be cited
- Include original data, research, or expertise (E-E-A-T signals)
- Structure content with clear subtopics (H2s/H3s that match related queries)
- Publish updates regularly — freshness matters more in AI Overviews than standard results
- Get mentioned on other authoritative pages covering the same topic

## E-E-A-T Signals (Experience, Expertise, Authoritativeness, Trustworthiness)

Google uses E-E-A-T to assess whether a source is trustworthy for AI answers:

- **Experience:** Author bio with firsthand experience. Specific examples from practice.
- **Expertise:** Credentials, professional background, linked profiles
- **Authoritativeness:** Backlinks from recognized publications, citations, mentions
- **Trustworthiness:** HTTPS, clear editorial policy, sourced claims, transparent corrections

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "We already rank #1, AEO doesn't matter" | #1 blue link gets fewer clicks than the answer box above it. AEO captures zero-click searches. |
| "Schema markup is too technical" | Plugins (Yoast, RankMath) generate FAQ schema without coding. Implement this week. |
| "Voice search is a small channel" | Smart speakers, mobile voice, and in-app assistants collectively represent 20%+ of search queries. |
| "Our content is already well-written" | Well-written for humans ≠ structured for extraction. Both matter. |

## Verification

- [ ] Target queries identified as question-format ("what is," "how to," "best way to")
- [ ] First paragraph answers the target question in 40-60 words
- [ ] FAQ section with 5+ questions and direct answers
- [ ] FAQ schema markup implemented and validated (Google Rich Results Test)
- [ ] E-E-A-T signals present: author bio, credentials, cited sources
- [ ] Voice answer candidate: one 20-30 word standalone answer per target query
