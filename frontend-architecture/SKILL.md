---
name: frontend-architecture
description: Use when the user wants to structure a frontend application, choose a state management approach, organize components, design a file structure, or make React/TypeScript architectural decisions. Also use when the user mentions 'component design,' 'folder structure,' 'state management,' 'when to use context vs zustand,' 'React patterns,' 'file organization,' 'how to structure my app,' or 'frontend architecture.'
---

# Frontend Architecture

Designs React/TypeScript frontends that stay maintainable past the first sprint — clear component boundaries, predictable state, and a file structure that new developers understand in 10 minutes.

## Component Design Principles

**One responsibility per component.** If the component name contains "and" — split it.

**Three component types:**

| Type | Responsibility | Example |
|---|---|---|
| **Page** | Owns routing, fetches data, composes layout | `ProjectPage.tsx` |
| **Feature** | Owns one user-facing capability, local state | `ProjectForm.tsx` |
| **UI** | Purely presentational, no data fetching, no side effects | `Button.tsx`, `Card.tsx` |

**Rules:**
- Pages fetch, features interact, UI components display — never reverse this
- UI components accept only props — no hooks that call APIs
- Feature components own their loading and error states

## File Structure

```
src/
  pages/          — one file per route
  features/       — one folder per domain (auth/, projects/, billing/)
    components/   — feature-specific components
    hooks/        — feature-specific hooks
    types.ts      — feature-specific types
  components/     — shared UI components
  hooks/          — shared hooks
  lib/            — utilities, API clients, constants
  types/          — global types and interfaces
```

**Rule:** If a component is used in only one feature, it lives in that feature's folder. Promote to `src/components/` only when it's used in two or more features.

## State Management Decision Tree

```
Is state only needed in one component?
  YES → useState

Is state shared between sibling components?
  YES → lift to parent, pass as props

Is state needed across many components in one feature?
  YES → useContext + useReducer (feature-scoped)

Is state needed globally (auth, theme, cart)?
  YES → Zustand or Jotai (prefer Zustand for complex state)

Is state server data (fetched from an API)?
  YES → TanStack Query (React Query) — never duplicate in useState
```

**Never:** Store server data in useState. React Query owns server state. useState owns UI state. These are different things.

## Data Fetching Patterns

**Always use a data-fetching library** (TanStack Query, SWR) for API calls. Raw useEffect + fetch leads to:
- Race conditions on fast navigation
- Duplicate requests
- No caching
- Manual loading/error state management

**Query key convention:**
```typescript
// Resource + ID + filter
['projects']                        // all projects
['projects', projectId]             // one project
['projects', projectId, 'members']  // project members
```

**Mutations:** Always invalidate the relevant query key on success — never manually update the cache.

## TypeScript Conventions

**Define types at the boundary, not the leaf.** Types for API responses live in `lib/api/types.ts`. Types for component props are inline or in the component file.

**Avoid `any`.** If you need `any`, the type boundary is in the wrong place.

**Prefer interfaces for object shapes, types for unions:**
```typescript
interface Project { id: string; name: string; status: ProjectStatus }
type ProjectStatus = 'draft' | 'active' | 'archived'
```

## Performance Patterns

| Problem | Fix |
|---|---|
| Component re-renders too often | `React.memo` on pure UI components only — profile first |
| Expensive calculation on every render | `useMemo` — only when profiler confirms the cost |
| Callback recreated on every render | `useCallback` — only when passed to memoized child |
| Large bundle size | Route-based code splitting with `React.lazy` + `Suspense` |
| Slow list with 100+ items | Virtualization (`react-virtual`, `tanstack-virtual`) |

**Rule:** Don't optimize before profiling. `useMemo` and `useCallback` are not free — they add complexity and can slow renders if overused.

## Error and Loading States

Every data-fetching component needs three states designed explicitly:
- **Loading:** skeleton or spinner — never blank
- **Error:** actionable message — never raw error object
- **Empty:** explain why and offer a next action — never blank list

These are product decisions, not edge cases. Design them in Figma before coding.

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "I'll organize files later" | File structure decisions compound. A bad structure at 10 components is a nightmare at 100. |
| "useContext is enough for global state" | Context re-renders all consumers on every change. Zustand is 200 bytes and solves this. |
| "I'll just use useEffect for data fetching" | Race conditions, no caching, manual loading state. React Query exists for this. |
| "TypeScript is slowing me down" | You're writing the types that TypeScript would catch at runtime. Pay now or pay in prod. |
| "I'll add error states later" | Users hit error states on day one. They're not edge cases. |

## Verification

- [ ] File structure follows pages / features / components / hooks / lib
- [ ] Every component has a single responsibility (no "and" in the name)
- [ ] Server state managed by React Query — not duplicated in useState
- [ ] State management choice matches complexity (useState → context → Zustand)
- [ ] Every data-fetching component has loading, error, and empty states
- [ ] No raw `any` types in non-migration code
- [ ] Route-based code splitting implemented for pages
