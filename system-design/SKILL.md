---
name: system-design
description: Use when the user wants to design a system before building it, create a design document, define component boundaries, map data flows, or establish API contracts. Also use when the user mentions 'design.md,' 'system design,' 'architecture document,' 'component diagram,' 'data flow,' 'API contract,' 'service boundaries,' 'entity model,' 'dependency map,' 'design before coding,' 'impeccable,' or 'design quality.'
---

# System Design

Produces a DESIGN.md artifact that anchors the build — components, data flows, API contracts, state boundaries, dependency graph, and UI design quality standard — before any code is written.

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

### 9. UI Design Quality Standard

*Required when the system includes a user interface. Skip only for pure API or backend systems.*

AI-generated UIs share a recognizable set of visual patterns — called **visual slop** — that signal default choices rather than intentional design. Design quality is not a post-launch concern: first impressions are product.

**Run the linter before marking any UI complete:**
```bash
npx impeccable detect src/
```

Each finding maps to a **design verb** that names the fix:

| Verb | When to apply | Effect |
|---|---|---|
| `bolder` | Design feels safe, bland, or invisible | Stronger contrast, bigger type, more decisive color |
| `quieter` | Design is noisy or overwhelming | Mute colors, tighten spacing, remove decoration |
| `distill` | Too many cards, labels, or sections | Strip until nothing left to cut — one idea per section |
| `polish` | Structurally sound but unrefined | Spacing rhythm, alignment, hover states, micro-details |
| `clarify` | Hard to scan or understand | Stronger size contrast, better labels, fix line length |
| `animate` | Needs life or feedback cues | Purposeful motion — enter/exit, micro-interactions |
| `harden` | Works for happy path only | Error states, empty states, overflow, dark mode, i18n |

**Top visual slop signals to eliminate before shipping:**

| Signal | What it looks like | Fix |
|---|---|---|
| `side-tab` | Thick colored left border on a card | `distill` — remove; use background tint instead |
| `gradient-text` | `background-clip: text` gradient | `distill` — solid color only |
| `nested-cards` | Cards inside cards | `distill` — flatten; use spacing and type hierarchy |
| `overused-font` | Inter, Geist, Plus Jakarta Sans, Space Grotesk | `bolder` — choose a face with genuine personality |
| `ai-color-palette` | Purple/violet gradients, cyan-on-dark | `bolder` — use a distinctive intentional palette |
| `monotonous-spacing` | Same spacing value everywhere | `polish` — tight groups for related items, generous gaps between sections |
| `everything-centered` | Every element center-aligned | `clarify` — left-align body; center only hero and CTA |
| `icon-tile-stack` | Rounded-square icon above heading | `distill` — side-by-side icon+heading |
| `flat-type-hierarchy` | Font sizes too close together | `clarify` — at least 1.25× ratio between type steps |
| `low-contrast` | Text failing WCAG AA (< 4.5:1 body) | `harden` — increase contrast |
| `bounce-easing` | Bounce or elastic CSS easing | `animate` — use ease-out-quart/expo instead |

**Define the design quality bar in DESIGN.md:**
```
UI Quality:
  Linter: npx impeccable detect — zero slop-category findings at launch
  Contrast: WCAG AA minimum (4.5:1 body, 3:1 large text)
  Typography: custom font pair defined (not Inter-only)
  Color: intentional brand palette (not AI default purple/violet)
  States: loading, empty, error designed for every data-fetching screen
```

> Attribution: Impeccable visual-slop signals adapted from [impeccable](https://github.com/pbakaus/impeccable) by Peter Bakaus, Apache-2.0.

---

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
- [ ] UI quality bar defined (if system has a UI)

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
| Design quality deferred | "We'll fix the UI after launch" | Slop trains users to expect low quality. First impressions are product. |

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "We'll design as we build" | You'll design as you rework. The cost of design post-code is 3–5× the cost pre-code. |
| "The design will change anyway" | The data model and API contracts almost never change as much as you think. Lock them early. |
| "DESIGN.md is documentation, not engineering" | It's a contract. The frontend team, backend team, and product team all sign it. |
| "Our system is too simple to need this" | Simple systems that skip design become complex systems nobody understands. |
| "We can add the open questions later" | Open questions without dates are decisions that never get made. Date them now. |
| "The linter is too strict" | Each impeccable rule maps to a documented AI-slop tell or accessibility standard. Challenge with evidence, not vibes. |
| "Inter is fine — everyone uses it" | That is the problem. Every AI-generated UI uses it. It signals default choice, not design intent. |

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
- [ ] `npx impeccable detect` run on UI — all slop-category findings addressed
- [ ] UI quality bar documented in DESIGN.md (if system has a UI)
