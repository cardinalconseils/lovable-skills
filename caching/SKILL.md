---
name: caching
description: "Caching strategy selection and implementation for production applications. Use when: choosing between write-through and write-behind cache, adding Redis or Memcached, reducing write or read latency, designing cache invalidation, implementing read-through or cache-aside patterns, or evaluating data loss vs latency trade-offs."
---

# Caching

## When to Use

- Write requests are slow due to database write latency
- Read requests repeatedly fetch the same expensive data
- Database cannot handle the required read/write throughput
- Expensive computations (ML inference, aggregation queries) are called frequently

## When NOT to Use

- Prototype stage where complexity outweighs the benefit
- Data that must never be stale (financial balances, inventory counts)
- Before profiling confirms the cache will actually hit

## Strategies

### Write-Through Cache
Server writes to both cache and database simultaneously, waits for both before responding.
- **Consistency**: Strong — cache and DB always in sync
- **Data loss risk**: None
- **Best for**: Financial transactions, inventory, any data where loss is unacceptable

### Write-Behind Cache
Server writes only to cache and responds immediately; cache flushes to DB asynchronously.
- **Latency**: Lowest
- **Data loss risk**: Real — if cache fails before flush, writes are lost
- **Best for**: High-volume writes where occasional loss is acceptable (analytics events, view counts)

### Read-Through Cache
Application always reads from cache; on miss, cache fetches from DB and populates itself.
- **Best for**: Read-heavy workloads with infrequent updates (product catalogs, configuration)

### Cache-Aside (Lazy Loading)
Application checks cache first; on miss, reads from DB and writes to cache manually.
- **Best for**: When the application needs fine-grained control over what is cached

## Strategy Comparison

| Strategy | Write Latency | Data Loss Risk | Consistency | Complexity |
|---|---|---|---|---|
| Write-Through | High | None | Strong | Low |
| Write-Behind | Lowest | Real (cache failure) | Eventual | Medium |
| Read-Through | N/A | N/A | TTL-based | Low |
| Cache-Aside | N/A | N/A | App-controlled | Medium |

## When to Choose Each

| Requirement | Recommended Strategy |
|---|---|
| Financial data, no data loss tolerance | Write-Through |
| High-volume writes, loss acceptable | Write-Behind |
| Read-heavy, infrequent changes | Read-Through or Cache-Aside |
| Need explicit control over cache population | Cache-Aside |

## Implementation Patterns

**TTL discipline** — Every cache entry must have a TTL. No TTL means stale data accumulates indefinitely.

**Cache stampede prevention** — When a popular key expires, many requests hit the DB simultaneously. Mitigate with probabilistic early expiration, mutex locks on miss, or background refresh.

**Hot key avoidance** — Shard hot keys with a suffix (`user:123:1`, `user:123:2`) and fan out reads.

**Graceful degradation** — If cache is unavailable, fall through to the database rather than returning an error.

**Write-behind flush discipline** — Flush to DB on schedule AND on cache eviction. Never rely solely on TTL.

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "A cache will fix our slow queries" | Caching hides slow queries. Fix the query first; cache if throughput still requires it. |
| "Write-behind is fine, we rarely lose data" | "Rarely" becomes "definitely" during the outage you didn't predict. |
| "We'll add cache invalidation later" | Stale data bugs are the hardest to debug. Design invalidation before deploying the cache. |
| "We don't need TTLs for this data" | Without TTLs, caches become stale indefinitely. Every key needs a TTL or explicit invalidation trigger. |

## Verification

- [ ] Caching strategy selected based on data loss tolerance
- [ ] Every cache key has a TTL
- [ ] Write-behind: flush-on-eviction configured
- [ ] Cache miss fallback path tested (what happens when Redis is down?)
- [ ] Cache hit rate monitored
- [ ] Stale data scenario tested
