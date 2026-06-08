---
name: database-design
description: Use when the user wants to design a database schema, choose between SQL and NoSQL, decide on normalization, set up indexes, handle migrations, or make data modeling decisions. Also use when the user mentions 'schema design,' 'data model,' 'normalization,' 'when to denormalize,' 'indexing strategy,' 'database migration,' 'foreign keys,' 'multi-tenancy,' 'soft delete,' or 'how should I structure my data.'
---

# Database Design

Designs schemas that stay coherent under real query patterns — right normalization, right indexes, clear ownership, and migrations that don't break production.

## SQL vs NoSQL Decision

**Use SQL (PostgreSQL) by default.** The cases for NoSQL are specific:

| Use SQL when | Use NoSQL when |
|---|---|
| Data has clear relationships | Data has no fixed schema and schema changes constantly |
| You need joins across entities | You need horizontal write scaling beyond what Postgres can handle |
| You need ACID transactions | You're storing documents, logs, or time-series data |
| You're unsure — SQL handles unknowns better | You're building a specific use case: cache (Redis), search (Elasticsearch), graph (Neo4j) |

**Rule:** Start with PostgreSQL. Migrate to specialized storage when you hit a concrete limit, not a theoretical one.

## Normalization: The Right Level

**3rd Normal Form (3NF) is the default target.** Denormalize only with evidence.

**Normalize when:**
- Data is written frequently (normalization prevents update anomalies)
- Consistency is critical (one source of truth)
- Storage cost matters

**Denormalize when:**
- Read performance is measurably slow after indexing
- The query requires joining > 4 tables and can't be simplified
- The denormalized field is derived and read-heavy (e.g., `comment_count` on posts)

**Common denormalization patterns:**
- Cached aggregate: store `comment_count` on the parent, update on write
- Materialized view: pre-compute expensive joins, refresh on schedule
- Event log + projection: write events, read from projection table

## Schema Design Rules

**Every table needs:**
- `id` — UUID (preferred over serial int for distributed systems and public APIs)
- `created_at` — timestamp with timezone, default `now()`
- `updated_at` — timestamp with timezone, updated by trigger

**Naming conventions:**
- Tables: plural snake_case (`user_projects`, not `UserProject`)
- Foreign keys: `{referenced_table_singular}_id` (`user_id`, `project_id`)
- Booleans: prefix with `is_` or `has_` (`is_active`, `has_verified_email`)
- Soft delete: `deleted_at` timestamp (null = not deleted)

**Enum vs lookup table:**
- Enum: small, stable set that won't grow (status, role, type)
- Lookup table: set that users or admins will add to over time

## Indexing Strategy

**Create indexes for:**
- Every foreign key column (prevents full table scan on joins)
- Columns that appear in `WHERE`, `ORDER BY`, or `JOIN ON` clauses in frequent queries
- Columns used in uniqueness constraints

**Index types:**
| Type | Use for |
|---|---|
| B-tree (default) | Equality, range, sort — 95% of indexes |
| GIN | Full-text search, JSONB containment, arrays |
| GiST | Geometric data, IP ranges |
| Partial index | Index subset of rows: `WHERE deleted_at IS NULL` |

**Don't over-index:** Every index slows writes. Index the queries you have, not the queries you imagine.

**Composite index column order:** Most selective column first. `(user_id, created_at)` — not `(created_at, user_id)` — if user_id is the primary filter.

## Multi-Tenancy Patterns

| Pattern | How | When |
|---|---|---|
| **Row-level** | `tenant_id` column on every table + RLS policy | Default for SaaS — simple, scalable |
| **Schema-level** | One schema per tenant | Strong isolation required, < 1000 tenants |
| **Database-level** | One database per tenant | Compliance/data residency requirements |

**Row-level with RLS (recommended for most SaaS):**
- Add `org_id` to every tenant-scoped table
- Enable Row Level Security on each table
- Create policy: `USING (org_id = auth.jwt() ->> 'org_id')`
- Every query automatically scoped — no application-level filtering needed

## Soft Delete vs Hard Delete

**Soft delete by default for user-generated content:**
```sql
deleted_at TIMESTAMPTZ DEFAULT NULL
-- Query: WHERE deleted_at IS NULL
-- Partial index: CREATE INDEX ON items (id) WHERE deleted_at IS NULL
```

**Hard delete for:** PII under GDPR right-to-erasure, logs, temporary records, anything with strict storage constraints.

**Never mix:** Choose one approach per table. Mixing creates "is this actually deleted?" confusion.

## Migration Rules

1. **Always additive first:** Add columns as nullable before making them NOT NULL
2. **Never rename in one step:** Add new column → backfill → switch app code → drop old column
3. **Test rollback:** Every migration needs a `down` migration that's been run at least once
4. **Batch large updates:** Updating 1M rows in one transaction locks the table. Use batches of 10k.
5. **Index before constraint:** Adding a UNIQUE constraint on a large table = full table scan. Build the index first with `CREATE INDEX CONCURRENTLY`.

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "I'll use a single JSON column — more flexible" | JSON columns are flexible right up until you need to query or index into them. |
| "I'll add indexes later when it's slow" | Adding indexes on a 10M-row table in production blocks queries. Index early. |
| "Soft delete is more complex" | Hard deleting user data and then needing it back is more complex. |
| "UUID primary keys are slower" | Measurably slower only at extreme scale. The API and distribution benefits far outweigh this. |
| "I'll normalize later" | Schema changes in production require migrations. Design the right shape first. |

## Verification

- [ ] Every table has `id` (UUID), `created_at`, `updated_at`
- [ ] Every foreign key column has an index
- [ ] Multi-tenant tables have `org_id` column + RLS policy
- [ ] Soft delete strategy chosen and applied consistently per table
- [ ] Enums used for stable sets, lookup tables for admin-managed sets
- [ ] Composite index column order matches query patterns (selective first)
- [ ] Migrations are additive — no destructive single-step changes
- [ ] Every migration has a tested rollback
