---
name: testing-discipline
description: "Test-driven development discipline and testing strategy for production applications. Use when: writing tests, fixing bugs, implementing features, running test suites, choosing what to test, or when the agent skips testing. Enforces RED-GREEN-REFACTOR cycle and the Prove-It Pattern for bug fixes."
---

# Testing Discipline

## TDD Cycle: RED-GREEN-REFACTOR

1. **RED**: Write a test that describes the desired behavior. Run it. It MUST fail.
2. **GREEN**: Write the minimum code to make the test pass. No more.
3. **REFACTOR**: Clean up the code while keeping all tests green.

Small cycles (5–15 minutes) keep you focused.

## The Prove-It Pattern (Bug Fixes)

1. Bug reported or observed
2. Write a test that **reproduces the bug** — this test MUST FAIL
3. Fix the bug — the test now PASSES
4. The test stays in the suite **forever** — it prevents regression

Never fix a bug without a test.

## Test Pyramid

| Level | Quantity | Speed | Scope |
|-------|----------|-------|-------|
| Unit | Many (70%) | Fast (ms) | Single function/class |
| Integration | Some (20%) | Medium (s) | Multiple components |
| E2E | Few (10%) | Slow (s-min) | Full user flow |

## What to Test

- Business logic and domain rules
- Edge cases: null, undefined, empty string, empty array, boundary values
- Error paths: invalid input, network failures, permission denied
- Security boundaries: auth checks, input validation, access control

## What NOT to Test

- Framework internals (React rendering, Express routing)
- Third-party library behavior
- Trivial code with no logic (getters, simple pass-through)
- Private implementation details (test behavior, not internals)

## Framework Patterns

| Layer | Recommended Tools |
|-------|------------------|
| Unit tests | Jest, Vitest, pytest, Go testing |
| Component tests | Testing Library (React/Vue/Svelte) |
| API integration | Supertest, httpx |
| E2E tests | Playwright, Cypress |

## Test Naming

```
describe("UserService", () => {
  describe("createUser", () => {
    it("creates a user with valid email and password", ...)
    it("throws ValidationError when email is empty", ...)
    it("throws ConflictError when email already exists", ...)
  })
})
```

Pattern: `it("[expected behavior] when [condition]")`

## Mocking Discipline

- Mock at **boundaries**: external APIs, databases, file system, time
- Do NOT mock internal functions — that tests implementation, not behavior
- If you need more than 3 mocks in a unit test, the unit is too coupled

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "This is too simple to test" | Simple code with tests stays simple. Without tests it becomes complex. |
| "I'll add tests later" | Later means never. The test is the proof. Write it now. |
| "The manual test worked" | Manual tests don't prevent regressions. Automated tests do. |
| "100% coverage is the goal" | Coverage measures lines executed, not correctness. Test behavior. |

## Verification

- [ ] Every new feature has tests written BEFORE implementation (RED first)
- [ ] Every bug fix has a regression test that fails without the fix
- [ ] Test suite passes with zero skipped tests
- [ ] Unit tests run in under 30 seconds
- [ ] No mocks on internal functions — only at boundaries
- [ ] Edge cases covered: null, empty, boundary values, error paths
- [ ] CI runs full test suite on every PR
