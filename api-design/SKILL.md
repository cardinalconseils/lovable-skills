---
name: api-design
description: Use when the user wants to design an API, define endpoint contracts, choose between REST and GraphQL, handle authentication, version an API, or define error responses. Also use when the user mentions 'API design,' 'REST conventions,' 'endpoint naming,' 'API versioning,' 'authentication patterns,' 'JWT vs session,' 'API error handling,' 'pagination,' 'rate limiting,' or 'API contract.'
---

# API Design

Designs APIs that frontend teams can consume without reading source code — predictable conventions, explicit contracts, and error messages that tell callers what to do next.

## REST vs GraphQL Decision

**Use REST by default.** GraphQL for specific cases:

| Use REST when | Use GraphQL when |
|---|---|
| Standard CRUD operations | Multiple frontends with very different data needs |
| Simple resource relationships | Frontend needs to compose data from many resources in one request |
| Public API (simpler to document) | Data is highly graph-shaped with deep nesting |
| Team is new to API design | You have a dedicated API team with GraphQL expertise |

**Rule:** GraphQL's flexibility comes with complexity in caching, rate limiting, and tooling. Default to REST unless you have a concrete problem it solves.

## REST Conventions

**URL structure:**
```
GET    /resources              — list
POST   /resources              — create
GET    /resources/:id          — get one
PUT    /resources/:id          — replace (full update)
PATCH  /resources/:id          — partial update
DELETE /resources/:id          — delete
GET    /resources/:id/children — nested resource list
```

**Naming rules:**
- Plural nouns for collections (`/projects`, not `/project`)
- Kebab-case for multi-word resources (`/team-members`, not `/teamMembers`)
- No verbs in URLs — the HTTP method is the verb (`DELETE /sessions`, not `POST /logout`)
- Nest only one level deep — `GET /projects/:id/members` is fine; `/projects/:id/members/:id/roles` is too deep

## Request and Response Shape

**Consistent response envelope:**
```json
// Success
{ "data": { ... } }

// List
{ "data": [...], "meta": { "total": 100, "page": 1, "per_page": 20 } }

// Error
{ "error": { "code": "RESOURCE_NOT_FOUND", "message": "Project not found", "details": {} } }
```

**Rules:**
- Always wrap in `data` — never return a naked object or array at root
- Always return the same shape for the same endpoint — no conditional shapes
- Include `meta` on every list endpoint — clients need total count for pagination

## Error Design

Errors are part of the API contract. Design them as carefully as success responses.

| HTTP Status | Use for |
|---|---|
| 200 | Success |
| 201 | Created (POST that creates a resource) |
| 204 | No content (DELETE, or PATCH with no body response) |
| 400 | Bad request — client sent invalid data |
| 401 | Unauthenticated — no valid credentials |
| 403 | Unauthorized — authenticated but not allowed |
| 404 | Not found |
| 409 | Conflict — duplicate, version mismatch |
| 422 | Validation error — semantically invalid request |
| 429 | Rate limited |
| 500 | Server error — never expose internals |

**Error response must include:**
- Machine-readable code: `"VALIDATION_ERROR"`, `"RESOURCE_NOT_FOUND"` (for programmatic handling)
- Human-readable message: what went wrong
- Details: field-level errors for validation failures

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Request validation failed",
    "details": {
      "email": ["must be a valid email address"],
      "name": ["is required"]
    }
  }
}
```

## Authentication Patterns

**JWT (stateless) — use for:**
- APIs consumed by SPAs or mobile apps
- Distributed systems where session sharing is complex
- Short-lived tokens (15–60 min) with refresh token rotation

**Session (stateful) — use for:**
- Server-rendered apps with a single backend
- When immediate revocation is required (e.g., security-sensitive apps)
- Simpler infrastructure (no token refresh logic)

**JWT rules:**
- Access token TTL: 15–60 minutes
- Refresh token TTL: 7–30 days, stored in httpOnly cookie
- Rotate refresh token on every use (refresh token rotation)
- Never store sensitive data in JWT payload — it's base64, not encrypted
- Verify signature on every request — never trust the payload without verification

**Authorization header convention:**
```
Authorization: Bearer <token>
```

## API Versioning

**URL versioning is the pragmatic default:**
```
/v1/projects
/v2/projects
```

**Rules:**
- Never break an existing version — add a new version
- v1 is the first public version — internal APIs can skip versioning
- Deprecation notice: add `Deprecation` header 6 months before sunset
- Sunset date: add `Sunset` header with the removal date

## Pagination

**Cursor pagination for production, offset for simple cases:**

```json
// Offset (simple, degrades at scale)
?page=2&per_page=20
{ "data": [...], "meta": { "total": 100, "page": 2, "per_page": 20 } }

// Cursor (stable, scales)
?after=cursor_xyz&limit=20
{ "data": [...], "meta": { "next_cursor": "cursor_abc", "has_more": true } }
```

**Use cursor when:** list is large (> 10k rows), items are frequently inserted/deleted, or sorted by non-unique field.

## Rate Limiting

Every public API needs rate limiting. Standard headers:
```
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 95
X-RateLimit-Reset: 1640000000
Retry-After: 60  (on 429 response)
```

**Limits by tier (starting points):**
- Unauthenticated: 20 req/min
- Free tier: 60 req/min
- Paid tier: 600 req/min

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "We'll design the API as we build" | The frontend team is blocked until the contract is defined. Design first. |
| "Errors just return 500 for now" | Clients can't retry intelligently without status codes. Design errors on day one. |
| "We don't need versioning yet" | Versioning is cheap to add before you have clients. Retrofitting it after = breaking all clients. |
| "Verbs in URLs are more readable" | REST uses HTTP verbs. `POST /logout` is noise; `DELETE /sessions` is clear. |
| "JWT is always better than sessions" | JWT is stateless which means you can't revoke it. For admin tools or security-sensitive apps, sessions win. |

## Verification

- [ ] URL structure follows REST conventions (plural nouns, no verbs, max one nesting level)
- [ ] All responses wrapped in `{ "data": ... }` envelope
- [ ] List endpoints include `meta` with total count and pagination info
- [ ] Error responses include machine-readable code + human message + field details
- [ ] HTTP status codes used correctly (401 vs 403, 400 vs 422)
- [ ] Authentication pattern chosen (JWT or session) with TTL and rotation defined
- [ ] API versioned from day one (`/v1/`)
- [ ] Rate limiting headers present on all endpoints
- [ ] Pagination strategy chosen: cursor (large datasets) or offset (simple)
