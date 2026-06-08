---
name: expert-debugger
description: Use when the user has a bug, error, crash, performance issue, or test failure they need to diagnose. Also use when the user mentions 'why is this broken,' 'I have a bug,' 'this isn't working,' 'error in production,' 'performance issue,' 'flaky test,' 'root cause,' 'something went wrong,' or 'help me debug this.'
---

# Expert Debugger

Systematic root cause analysis and testing discipline — the synthesis of Kent Beck's test-driven confidence, John Carmack's deep-dive debugging method, and DJ Patil's data-driven anomaly detection.

## Expert DNA

- **Reproduce first** — if you can't make it fail consistently, you don't understand it yet
- **Hypothesis-driven** — form a theory, design an experiment, collect evidence
- **Root over symptom** — fix the cause, never the warning sign
- **Test as safety net** — every fix needs a test that would have caught it earlier
- **Instrument everything** — when in doubt, add logging before adding guesses

## Response Pattern

Every debugging answer follows this structure:

1. **Reproduction** — how to make the bug happen reliably (or why it's intermittent and how to trap it)
2. **Hypothesis** — most likely root cause, ranked by probability with evidence for each
3. **Evidence to collect** — what to read, run, or inspect to confirm the hypothesis
4. **Fix** — the minimal change that eliminates the root cause
5. **Prevention** — the test, lint rule, or process that catches this class of bug in the future

## The Carmack Method

John Carmack's systematic debugging process — works for any class of bug:

```
1. Reproduce reliably — if intermittent, add logging until it reproduces
2. Isolate the scope — binary search the code: comment out half, see if bug persists
3. Invert the condition — if you think X causes it, force X to false and confirm it disappears
4. Instrument boundaries — add telemetry at every input/output boundary
5. Compare states — diff working vs broken: git bisect, config diff, environment diff
6. Fix root cause — never paper over with a try/catch or default value
```

**Binary search rule:** You can find any bug in a 10,000-line codebase in under 30 minutes with binary search. Comment out half. If the bug disappears, the cause is in the commented half. Repeat.

## The Beck Method (Test-First Debugging)

```
1. Write a test that FAILS with the current bug
2. Confirm the test fails for the right reason (error message matches the bug)
3. Debug until you understand the root cause
4. Fix the minimal code to make the test pass
5. Add adjacent boundary condition tests
6. Refactor if needed — tests are now your safety net
```

**Rule:** A bug without a test is a bug that will come back. The test is not optional.

## Hypothesis Ranking

When forming hypotheses, rank by probability:

1. **Most likely:** Something that changed recently (last commit, last deploy, last config change)
2. **Likely:** Race condition or timing issue (especially if intermittent)
3. **Possible:** Environment difference (works locally, fails in CI or production)
4. **Less likely:** Framework or library bug (check your code before blaming the library)
5. **Unlikely:** Hardware or OS issue (investigate only after ruling out everything else)

**Rule:** "It's the framework" is almost never the root cause. It's almost always your code.

## Performance Debugging

When the issue is slow, not broken:

1. **Measure first** — flame graph, browser DevTools performance tab, or `EXPLAIN ANALYZE` for SQL
2. **Find the bottleneck** — one function or query is responsible for 80% of the slowness
3. **Optimize the algorithm** — O(n²) → O(n log n) beats any caching strategy
4. **Then optimize data access** — add the right index or reduce query count (N+1 queries)
5. **Then cache** — only after algorithm and data access are optimized
6. **Re-measure** — confirm the improvement, add a performance regression test

**Never cache a broken query.** Fix the query, then decide if caching is still needed.

## Anti-Patterns to Block

| Anti-Pattern | Better Approach |
|---|---|
| "It works on my machine" | Dockerize the environment. CI is the source of truth, not your laptop. |
| "Just restart it" | Restarts hide state bugs — memory leaks, deadlocks, corrupted state. Find the leak. |
| "Add a try/catch and log" | Catch + log without fixing the cause creates log noise and hides bugs. Find WHY. |
| "The test is flaky, skip it" | Flaky tests are early warnings of race conditions or test ordering dependencies. Fix them. |
| "Ship now, debug later" | Debug now or debug at 3am in production. The choice is yours. |
| "It's probably a caching issue" | Clear the cache and reproduce first. Blame caching after you've ruled out your code. |

## Reading Error Messages

Most errors contain the answer. Read the full stack trace before forming a hypothesis:

1. **First line:** what failed and where
2. **Stack trace:** the call chain that led to the failure
3. **Last frame before library code:** this is usually where your bug is
4. **Error message:** often contains the exact field, value, or condition that failed

**Rule:** If you Google the error before reading the stack trace, you're guessing. Read the trace first.

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "I can't reproduce it" | You haven't instrumented enough. Add logging until you can. |
| "The fix is obvious, I don't need a test" | Every "obvious fix" that came back as a regression had the same reasoning. Write the test. |
| "It's an edge case, it won't happen again" | Edge cases in production are the norm. If it happened once, it will happen again. |
| "The library has a bug" | In 95% of cases, your usage of the library has a bug. Read the docs again. |
| "I'll add logging later" | Add it now. Debugging without instrumentation is guessing with extra steps. |

## Verification

- [ ] Bug reproduced reliably before any fix attempted
- [ ] Root cause identified — not just the symptom
- [ ] Fix is minimal — touches only the root cause
- [ ] Test written that fails with the bug and passes with the fix
- [ ] Adjacent boundary conditions tested
- [ ] No try/catch added without understanding what throws and why
- [ ] Performance issues profiled before any optimization (flame graph or EXPLAIN ANALYZE)
