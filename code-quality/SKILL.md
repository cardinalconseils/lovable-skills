---
name: code-quality
description: Use when the user wants to improve code quality, simplify code, refactor for clarity, apply naming conventions, or enforce engineering discipline. Also use when the user mentions 'clean code', 'refactor', 'simplify', 'naming', 'code review', 'readability', or 'maintainability'.
---

# Code Quality

Principles and patterns for writing code that is readable, maintainable, and safe to change — without introducing unnecessary abstraction.

## Core Principle

**Simplicity beats cleverness.** Code is read 10× more than it is written. Optimize for the reader, not the writer.

The test: would a junior engineer understand this in 30 seconds? If not, simplify.

## Naming

**Functions and methods:** verb + noun phrases that describe what they do
- `getUserById` not `user` or `fetchData`
- `calculateTotalPrice` not `calc` or `process`
- `sendPasswordResetEmail` not `handleEmail`

**Booleans:** `is`, `has`, `can`, `should` prefix
- `isActive`, `hasPermission`, `canEdit`, `shouldRedirect`
- Never: `active`, `permission`, `edit`, `redirect` (ambiguous as boolean)

**Variables:** noun phrases describing the value, not the type
- `activeUsers` not `userArray` or `arr`
- `orderTotal` not `total` or `num`
- Single-letter variables only in tight loops (`i`, `j`) and short lambdas (`x => x * 2`)

**Constants:** SCREAMING_SNAKE_CASE for true constants, camelCase for module-level config
- `MAX_RETRY_COUNT = 3`
- `defaultPageSize = 20`

## Function Size and Responsibility

**30-line budget:** A function longer than 30 lines is doing too much. Split at natural boundaries.

**Single responsibility:** A function should do one thing and do it completely. If you need "and" to describe what it does, split it.

**Pure functions first:** Functions with no side effects are easier to test, easier to reason about, and easier to reuse.

```typescript
// Hard to test: side effects mixed with logic
async function processOrder(orderId: string) {
  const order = await db.findOrder(orderId);
  order.total = order.items.reduce((sum, item) => sum + item.price, 0);
  await db.saveOrder(order);
  await sendConfirmationEmail(order);
}

// Easier to test: pure calculation extracted
function calculateOrderTotal(items: OrderItem[]): number {
  return items.reduce((sum, item) => sum + item.price, 0);
}
// Side-effectful orchestration kept separate and minimal
async function processOrder(orderId: string) {
  const order = await db.findOrder(orderId);
  const total = calculateOrderTotal(order.items);
  await db.saveOrder({ ...order, total });
  await sendConfirmationEmail(order);
}
```

## Error Handling Strategy

**Pick one pattern per layer and use it consistently:**

| Pattern | Use when |
|---|---|
| Throw / catch | Functions that should not proceed on failure; caller handles the exception |
| Return `null` / `undefined` | Optional values that callers are expected to check |
| Return `Result<T, E>` type | When the caller must handle both success and failure paths |
| Early return (`guard clause`) | Validate preconditions at the top of a function |

**Never mix patterns in the same codebase layer** without a documented reason.

```typescript
// Guard clause pattern — preferred for validation
function processPayment(amount: number, card: Card) {
  if (amount <= 0) throw new Error('Amount must be positive');
  if (!card.isValid()) throw new Error('Invalid card');
  // happy path below
}
```

## Testability

Code is hard to test because of three things: hidden dependencies, side effects, and global state.

**Inject dependencies** instead of instantiating them:
```typescript
// Hard to test: tight coupling
class OrderService {
  private db = new Database(); // cannot be replaced in tests
}

// Testable: injected dependency
class OrderService {
  constructor(private db: Database) {}
}
```

**One assertion concept per test:** A test that checks 5 unrelated things tells you something broke but not what.

**Test behavior, not implementation:** Tests should break when behavior changes, not when you rename an internal function.

## Refactoring Discipline

When improving existing code:

1. **Preserve behavior exactly.** Refactoring changes structure, not behavior. If behavior changes, it's not a refactor — it's a feature.
2. **Follow project conventions.** Match the existing style, even if you'd do it differently. Consistency beats personal preference.
3. **One change at a time.** Rename in one commit, restructure in another. Mixed changes make review impossible.
4. **Leave adjacent code alone.** Do not "clean up" code you're not changing. Unrequested changes introduce unrequested risk.

## Idiom Conformance

Before writing new code, read 3 similar existing files to understand:
- Naming conventions used in this codebase
- Error handling approach chosen
- Import organization
- Comment style
- Test structure

Write code that looks like it already belongs. A reviewer should not be able to tell which PR added which function.

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "This abstraction will be useful later" | Write for now. Abstractions for hypothetical futures create complexity for real presents. |
| "It's only 60 lines — doesn't need splitting" | Length is a symptom. Split when there are distinct responsibilities, regardless of length. |
| "The name is obvious from context" | Context disappears. Names must be self-contained. |
| "I'll add tests after the feature works" | Tests written after are tests that confirm what you wrote, not tests that define what you need. |
| "Error handling can come later" | Silent failures in production are discovered by users, not engineers. Handle errors now. |

## Verification

- [ ] All functions under 30 lines or explicitly justified if longer
- [ ] All boolean variables use `is`/`has`/`can`/`should` prefix
- [ ] No function does two things describable with "and"
- [ ] Error handling pattern is consistent within each layer
- [ ] New code matches existing project idioms (naming, structure, imports)
- [ ] Pure logic extracted from side-effectful orchestration where possible
- [ ] Tests verify behavior, not implementation details
