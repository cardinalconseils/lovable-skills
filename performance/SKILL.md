---
name: performance
description: Use when the user wants to improve app speed, fix slow load times, reduce bundle size, optimize database queries, or hit Core Web Vitals targets. Also use when the user mentions 'slow page', 'LCP', 'bundle size', 'N+1 query', 'performance audit', 'Lighthouse score', or 'image optimization'.
---

# Performance Optimization

Expert knowledge for diagnosing and fixing performance bottlenecks in web applications — from Core Web Vitals to database query patterns.

## Core Web Vitals Targets

| Metric | Good | Needs Work | Poor |
|---|---|---|---|
| LCP (Largest Contentful Paint) | < 2.5s | 2.5–4s | > 4s |
| INP (Interaction to Next Paint) | < 200ms | 200–500ms | > 500ms |
| CLS (Cumulative Layout Shift) | < 0.1 | 0.1–0.25 | > 0.25 |
| TTFB (Time to First Byte) | < 800ms | 800ms–1.8s | > 1.8s |

**Measure first.** Never optimize without data. Use Lighthouse (lab), PageSpeed Insights (field), or Web Vitals extension before writing a line of code.

## Bundle Analysis

**Tools:** `@next/bundle-analyzer`, `vite-bundle-visualizer`, `source-map-explorer`

**Common culprits:**
- Importing an entire library when one function is needed (`import _ from 'lodash'` → `import debounce from 'lodash/debounce'`)
- Duplicate dependencies (two versions of the same package)
- Unminified third-party scripts loaded synchronously
- Large polyfills for browsers you don't support

**Remedies:**
- Tree-shaking: use named imports, ensure `sideEffects: false` in package.json
- Code splitting: dynamic `import()` for routes and heavy components
- `next/dynamic` or `React.lazy` + `Suspense` for below-fold components
- Move analytics/chat scripts to `strategy="lazyOnload"`

## Image Optimization

**Format priority:** AVIF > WebP > JPEG (never PNG for photos)

**Required attributes:**
- Always set `width` and `height` (prevents CLS)
- Use `loading="lazy"` for below-fold images
- Use `fetchpriority="high"` on the LCP image (hero/product shot)
- Use `srcset` + `sizes` for responsive images

**Next.js:** Use `<Image>` component — it handles format, sizing, and lazy loading automatically.

**Self-hosted:** Use Sharp for server-side conversion. Target: < 200KB for hero images, < 50KB for thumbnails.

## N+1 Query Detection

N+1 pattern: one query to fetch a list, then one query per item to fetch related data.

```sql
-- N+1 (bad)
SELECT * FROM posts;          -- 1 query
SELECT * FROM users WHERE id = ?  -- N queries, one per post

-- Fixed with JOIN
SELECT posts.*, users.name
FROM posts
JOIN users ON posts.user_id = users.id;
```

**ORM signals:** Prisma `findMany` inside a loop, Sequelize `include` missing, Django ORM missing `select_related`/`prefetch_related`.

**Fix:** Always JOIN or batch with `WHERE id IN (...)`. Enable query logging in development to catch N+1 before production.

## Database Query Optimization

**Index checklist:**
- Every foreign key column
- Every column used in `WHERE`, `ORDER BY`, or `JOIN ON`
- Composite index when two columns are always queried together

**Slow query checklist:**
- `EXPLAIN ANALYZE` on any query > 50ms
- Avoid `SELECT *` — name only the columns you use
- Avoid functions in WHERE clauses (`WHERE LOWER(email) = ?` prevents index use → store emails lowercased)
- Avoid `OFFSET` pagination on large tables — use cursor-based pagination instead

**Connection pooling:** Use PgBouncer (Postgres) or a connection pool library. Never open a new DB connection per request in serverless environments.

## Frontend Runtime Performance

**React-specific:**
- `useMemo` / `useCallback` only when profiler shows re-render cost > 16ms
- Virtualize long lists: `react-window` or `@tanstack/virtual`
- Avoid state in parent when only one child needs it (lift down, not up)
- Use React DevTools Profiler before adding memoization

**General:**
- Debounce search inputs (300ms), throttle scroll handlers
- Web Workers for CPU-heavy tasks (parsing, encryption, sorting)
- `requestAnimationFrame` for animations, never `setTimeout` for visual updates

## Caching Strategy Summary

| Layer | Tool | TTL guidance |
|---|---|---|
| Browser cache | `Cache-Control` headers | Static assets: 1 year + content hash |
| CDN | Vercel Edge, Cloudflare | HTML: stale-while-revalidate 60s |
| Server-side | Redis / Upstash | Per use case — see caching skill |
| DB query cache | Postgres shared_buffers | Managed by DB engine |

## Lighthouse Audit Workflow

1. Run Lighthouse in Chrome DevTools (incognito, throttled to Slow 4G)
2. Fix **Opportunities** in order of estimated savings (largest first)
3. Fix **Diagnostics** in order: eliminate render-blocking resources → reduce unused JS → fix image issues
4. Re-run after each change — don't batch fixes and re-measure once
5. Target: Performance > 90 for Candidate/Production stages

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "We'll optimize after launch" | Performance debt compounds. Users don't return after a slow first visit. |
| "Our users have fast connections" | Mobile on 4G is the median user. Always throttle in testing. |
| "Adding more indexes can't hurt" | Over-indexing slows writes and wastes storage. Index selectively. |
| "useMemo everywhere prevents re-renders" | Memoization has cost. Profile first, optimize where measured. |

## Verification

- [ ] Lighthouse performance score ≥ 90 (throttled, incognito)
- [ ] LCP < 2.5s, INP < 200ms, CLS < 0.1 confirmed in PageSpeed Insights
- [ ] No N+1 queries in development query log
- [ ] Bundle size analyzed — no full-library imports where tree-shaking is possible
- [ ] All images have explicit width/height, use modern format, LCP image has fetchpriority="high"
- [ ] Connection pooling configured for database
