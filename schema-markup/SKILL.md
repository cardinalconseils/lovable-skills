---
name: schema-markup
description: Use when the user wants to add structured data, schema markup, or JSON-LD to their pages for rich results in Google search. Also use when the user mentions 'structured data', 'JSON-LD', 'schema.org', 'rich results', 'rich snippets', 'FAQ schema', 'Product schema', or 'HowTo schema'.
---

# Schema Markup

Expert knowledge for implementing structured data that enables rich results in Google Search and improves search appearance.

## JSON-LD Always

Use JSON-LD. Not Microdata. Not RDFa.

JSON-LD is Google's preferred format, keeps markup out of HTML content, and is easier to maintain. Place the `<script type="application/ld+json">` block in the `<head>` or anywhere in the `<body>` — Google reads both.

## Schema Types by Page Type

### Organization (site-wide, homepage)
```json
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "name": "Your Company",
  "url": "https://yoursite.com",
  "logo": "https://yoursite.com/logo.png",
  "contactPoint": {
    "@type": "ContactPoint",
    "contactType": "customer support",
    "email": "support@yoursite.com"
  },
  "sameAs": [
    "https://twitter.com/yourhandle",
    "https://linkedin.com/company/yourcompany"
  ]
}
```

### SoftwareApplication (SaaS product page)
```json
{
  "@context": "https://schema.org",
  "@type": "SoftwareApplication",
  "name": "Your Product",
  "applicationCategory": "BusinessApplication",
  "operatingSystem": "Web",
  "offers": {
    "@type": "Offer",
    "price": "29",
    "priceCurrency": "USD",
    "priceSpecification": {
      "@type": "UnitPriceSpecification",
      "billingIncrement": 1,
      "unitCode": "MON"
    }
  },
  "aggregateRating": {
    "@type": "AggregateRating",
    "ratingValue": "4.7",
    "reviewCount": "243"
  }
}
```

### FAQPage (FAQ sections, support pages)
```json
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "How do I cancel my subscription?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "You can cancel from Settings > Billing > Cancel Plan. Your access continues until the end of the billing period."
      }
    }
  ]
}
```

### HowTo (tutorial pages, step-by-step guides)
```json
{
  "@context": "https://schema.org",
  "@type": "HowTo",
  "name": "How to set up your account",
  "step": [
    {
      "@type": "HowToStep",
      "name": "Create your account",
      "text": "Go to yoursite.com/signup and enter your email address."
    },
    {
      "@type": "HowToStep",
      "name": "Verify your email",
      "text": "Click the verification link sent to your inbox."
    }
  ]
}
```

### BlogPosting (blog articles)
```json
{
  "@context": "https://schema.org",
  "@type": "BlogPosting",
  "headline": "Article Title",
  "datePublished": "2025-01-15",
  "dateModified": "2025-03-01",
  "author": {
    "@type": "Person",
    "name": "Author Name"
  },
  "publisher": {
    "@type": "Organization",
    "name": "Your Company",
    "logo": {
      "@type": "ImageObject",
      "url": "https://yoursite.com/logo.png"
    }
  },
  "image": "https://yoursite.com/images/article-hero.jpg",
  "description": "Article meta description."
}
```

### BreadcrumbList (all inner pages)
```json
{
  "@context": "https://schema.org",
  "@type": "BreadcrumbList",
  "itemListElement": [
    {
      "@type": "ListItem",
      "position": 1,
      "name": "Home",
      "item": "https://yoursite.com"
    },
    {
      "@type": "ListItem",
      "position": 2,
      "name": "Blog",
      "item": "https://yoursite.com/blog"
    },
    {
      "@type": "ListItem",
      "position": 3,
      "name": "Article Title"
    }
  ]
}
```

## Rich Result Eligibility

| Schema Type | Rich Result Possible | SERP Feature |
|---|---|---|
| FAQPage | Yes | Expandable FAQ accordion |
| HowTo | Yes | Step list with images |
| Product | Yes | Price, availability, ratings |
| Recipe | Yes | Cook time, ratings, calories |
| BlogPosting | No | Article date in snippet only |
| SoftwareApplication | Yes | Rating stars, price |
| BreadcrumbList | Yes | Breadcrumbs in URL line |
| Organization | Partial | Knowledge panel (brand queries) |

## Validation

**Required before deploying:**
1. [Google Rich Results Test](https://search.google.com/test/rich-results) — validates eligibility for rich results
2. [Schema.org Validator](https://validator.schema.org/) — validates schema structure

**After deploying:**
- Submit URL to Google Search Console > URL Inspection > Request Indexing
- Monitor Search Console > Enhancements for schema errors (appears within 1–2 weeks)

## Common Mistakes

| Mistake | Fix |
|---|---|
| Schema on page doesn't match visible content | Every schema field must reflect what the user sees. Google cross-validates. |
| Using Microdata instead of JSON-LD | Rewrite as JSON-LD in `<script>` tag |
| FAQPage schema on pages where FAQ isn't visible | FAQ must be visible in the HTML content of the page |
| Incorrect `@type` nesting | Validate with Rich Results Test before deploying |
| Stale `dateModified` | Update `dateModified` every time content changes — use your CMS to automate |

## Implementation in Next.js

```tsx
// components/SchemaMarkup.tsx
export function SchemaMarkup({ schema }: { schema: object }) {
  return (
    <script
      type="application/ld+json"
      dangerouslySetInnerHTML={{ __html: JSON.stringify(schema) }}
    />
  );
}

// In your page
<SchemaMarkup schema={{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  // ...
}} />
```

## Verification

- [ ] JSON-LD used (not Microdata or RDFa)
- [ ] Organization schema on homepage
- [ ] FAQPage schema on FAQ sections (only if FAQ is visible in HTML)
- [ ] BlogPosting schema on all blog articles
- [ ] BreadcrumbList schema on all inner pages
- [ ] Product or SoftwareApplication schema on pricing/product pages
- [ ] All schema validated with Google Rich Results Test
- [ ] No schema fields present that aren't reflected in visible page content
- [ ] Search Console monitored for schema enhancement errors post-deploy
