---
name: system-design
description: Use when the user wants to design a system before building it, create a design document, define component boundaries, map data flows, or establish API contracts. Also use when the user mentions 'design.md,' 'system design,' 'architecture document,' 'component diagram,' 'data flow,' 'API contract,' 'service boundaries,' 'entity model,' 'dependency map,' or 'design before coding.'
---

# System Design

Produces a DESIGN.md artifact that anchors the build — components, data flows, API contracts, state boundaries, and dependency graph — before any code is written.

## Why DESIGN.md Before Code

Building without a design document is the single biggest source of rework in software projects. The design phase forces three things:

1. **Contradiction detection** — requirements that conflict surface in diagrams, not in code
2. **Scope lock** — what is in scope and out of scope is explicit and agreed
3. **Interface-first thinking** — APIs and contracts are designed for consumers, not for implementation convenience

A DESIGN.md takes 30–90 minutes to write. The rework it prevents takes days.

## DESIGN.md Structure

Every DESIGN.md contains these sections in order:

### 1. System Summary (3–5 sentences)
What the system does, who uses it, and what it does NOT do. The "does not do" line is mandatory — it prevents scope creep from day one.

```
System: [Name]
Purpose: [One sentence — what problem it solves]
Primary users: [Who interacts with it directly]
Out of scope: [What this system explicitly does not handle]
```

### 2. Component Map

List every major component (service, module, subsystem) and its single responsibility.

| Component | Responsibility | Technology |
|---|---|---|
| [Name] | [One sentence — what it owns] | [Stack] |

**Rules:**
- Each component has ONE responsibility — if the sentence has "and," split it
- No component should know about the internal implementation of another
- A component that is both a data store and a business logic layer is two components

### 3. Data Flow Diagram

Trace the path of the most important data through the system.

```
[Source] → [Component A] → [Component B] → [Storage]
                ↓
         [Side effect / event]
```

**Draw one flow per major use case.** Don't try to capture everything in one diagram — it becomes unreadable.

**Mark each arrow with:**
- Protocol: HTTP, gRPC, WebSocket, queue message, function call
- Data shape: JSON payload name or key fields
- Sync or async

### 4. Entity Model

Define the core data entities and their relationships.

```
User
  id: uuid (PK)
  email: string (unique)
  created_at: timestamp

Project
  id: uuid (PK)
  owner_id: uuid (FK → User)
  name: string
  status: enum [draft, active, archived]
```

**Rules:**
- Define primary keys and foreign keys explicitly
- Define enum values — not "a status field"
- Mark nullable fields (nullable fields are design decisions, not accidents)
- Don't define every field — define the fields that drive behavior or relationships

### 5. API Contract

Define the interface between components before implementing either side.

```
POST /api/projects
Request:  { name: string, template_id?: string }
Response: { id: string, name: string, created_at: string }
Errors:   400 (invalid name), 401 (not authenticated), 409 (name taken)

GET /api/projects/:id
Response: { id, name, status, owner: { id, email } }
Errors:   401, 403 (not owner), 404
```

**Rules:**
- Define error cases — not just the happy path
- Name the request and response shapes — not just "JSON object"
- If the API is consumed by a frontend, design it from the frontend's perspective
- Define pagination for any list endpoint (cursor or offset — decide now)

### 6. State Boundaries

For any component that holds state, define what it owns and what it does not.

| Component | Owns | Does NOT own |
|---|---|---|
| Auth service | Session tokens, user credentials | User profile data |
| Project service | Project records, membership | Billing status |

**Why this matters:** State ownership conflicts (two services writing to the same table, or neither owning a piece of data) are the most expensive architectural bugs to fix post-launch.

### 7. Dependency Graph

List external dependencies and the risk associated with each.

| Dependency | Used for | Risk if unavailable | Mitigation |
|---|---|---|---|
| Supabase | Auth + DB | Total outage | — |
| OpenAI API | LLM calls | Feature degraded | Fallback to cached response |
| Stripe | Billing | Can't process payments | Queue for retry |
| Resend | Email | No transactional email | Queue for retry |

**Rule:** Any dependency with "Total outage" in the risk column needs either a fallback or an explicit product decision that the risk is accepted.

### 8. Open Questions

List every decision that was deferred with a due date.

| Question | Why deferred | Decide by |
|---|---|---|
| Multi-tenant or single-tenant DB? | Need load estimate first | Before Phase 2 |
| Real-time updates via WebSocket or polling? | Depends on user research | Before Phase 1 |

**Rule:** Open questions without a "decide by" date are decisions that will never be made. Date them or make them now.

## Design Review Checklist

Before handing DESIGN.md to a builder:

- [ ] Every component has exactly one responsibility
- [ ] No circular dependencies between components
- [ ] Every API endpoint has error cases defined
- [ ] Every entity has a primary key
- [ ] Every foreign key relationship is named and directional
- [ ] State ownership is unambiguous — no two components own the same data
- [ ] Every external dependency has a risk + mitigation entry
- [ ] Open questions have "decide by" dates
- [ ] "Out of scope" section is present and specific

## Common Design Anti-Patterns

| Anti-pattern | Problem | Fix |
|---|---|---|
| God component | One component does everything | Split by responsibility — one noun, one component |
| Shared mutable state | Two services write to the same table | Assign one owner; other reads via API |
| Implicit contracts | "The frontend knows what format to expect" | Write the contract in DESIGN.md before coding either side |
| Optimistic entity model | Fields added later "when we need them" | Define the behavior-driving fields now; add derived fields later |
| Undated open questions | "We'll decide later" | Every open question gets a decide-by date or gets decided now |
| Skipping error cases in API | Only happy path defined | Every endpoint needs at least 401 + the domain error |
| Dependency without mitigation | "Total outage" risk accepted silently | Document the acceptance explicitly, or add a fallback |

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "We'll design as we build" | You'll design as you rework. The cost of design post-code is 3–5× the cost pre-code. |
| "The design will change anyway" | The data model and API contracts almost never change as much as you think. Lock them early. |
| "DESIGN.md is documentation, not engineering" | It's a contract. The frontend team, backend team, and product team all sign it. |
| "Our system is too simple to need this" | Simple systems that skip design become complex systems nobody understands. |
| "We can add the open questions later" | Open questions without dates are decisions that never get made. Date them now. |

## Verification

- [ ] DESIGN.md exists at project root or in `.docs/`
- [ ] System summary includes an explicit "out of scope" line
- [ ] Component map: every component has one responsibility (no "and")
- [ ] Data flow: at least one flow drawn per major use case
- [ ] Entity model: PKs, FKs, enums, and nullable fields defined
- [ ] API contract: every endpoint has error cases
- [ ] State boundaries: ownership unambiguous for every stateful component
- [ ] Dependency graph: risk + mitigation for every external dependency
- [ ] Open questions: every entry has a "decide by" date
