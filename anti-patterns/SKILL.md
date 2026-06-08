---
name: anti-patterns
description: Use when the user wants to review code or AI output for common mistakes, bad patterns, or quality issues. Also use when the user mentions 'bad code', 'code smell', 'antipattern', 'cleanup', 'technical debt', 'over-engineering', or 'silent failure'.
---

# Anti-Patterns

A catalog of recurring mistakes in AI-assisted development — code patterns, process patterns, output patterns, and documentation patterns that create hidden debt.

## Code Anti-Patterns

### Wrapper Bloat
Creating a class, function, or module whose only purpose is to call something else.

```typescript
// Anti-pattern: wrapper adds zero value
class UserService {
  async getUser(id: string) {
    return await userRepository.findById(id);
  }
}

// Better: call the repository directly
const user = await userRepository.findById(id);
```

**When a wrapper IS justified:** It adds error handling, caching, transformation, or cross-cutting concerns. If it does none of these, delete it.

### Defensive Noise
Adding null checks, try/catch blocks, and fallbacks for conditions that cannot happen given the surrounding code's guarantees.

```typescript
// Anti-pattern: checked after non-null assertion three lines earlier
const user = await getUser(id); // throws if not found
if (!user) { return; } // this line is never reached
```

Defensive code without understanding is noise. It hides real bugs by making impossible states appear handled.

### Silent Failure
Catching errors and discarding them without logging, re-throwing, or surfacing to the user.

```typescript
// Anti-pattern
try {
  await sendEmail(user);
} catch (e) {
  // ignore
}

// Better: at minimum, log with context
try {
  await sendEmail(user);
} catch (e) {
  logger.error('Email send failed', { userId: user.id, error: e });
  throw e; // or handle + surface to user
}
```

### Premature DRY
Extracting a shared abstraction because two things look similar, before it's clear they belong together.

Three similar lines is better than a premature abstraction. Wait until you have 3+ genuine callsites before extracting. Similarity in form ≠ similarity in concept.

### Feature-Flag Creep
Feature flags that never ship and never get cleaned up. After 30+ days, a feature flag for a shipped feature becomes permanent complexity.

**Rule:** Every feature flag gets a removal ticket at merge time. Flags live in code for ≤ 30 days post-launch.

## Process Anti-Patterns

### Undocumented Completion
Declarating work done without showing evidence. "It works" is not evidence. Show the test output, the build log, or the screenshot.

### Phase Bypass
Skipping a lifecycle phase because the task "feels simple." Simple tasks still need acceptance criteria. The cost of discovery is 10 minutes. The cost of building the wrong thing is 10 hours.

### Silent Decision
Making an architectural or approach choice without surfacing the alternatives and trade-offs. Future engineers will re-litigate silent decisions.

### Shadow Implementation
Building something outside the agreed plan because it "seemed easier" or "was already there." Shadow implementations diverge from the design and create maintenance confusion.

### Band-Aid Fix
Fixing a symptom without identifying the root cause. Symptoms reappear. Root causes, when fixed, stay fixed.

## Output Anti-Patterns (AI-specific)

### Narrated Execution
Describing what you are about to do instead of doing it. "I'll now create the file..." followed by creating the file adds no value. Show the result, not the plan to produce it.

### Speculative Done
"This should work" / "It looks correct" / "Seems fine." These phrases signal that the output was not verified. Run it. Show the output.

### Dead Comment
Comments that explain WHAT the code does (which the code already says) instead of WHY:

```typescript
// Anti-pattern: comment restates the code
// Get user by ID
const user = await getUser(id);

// Justified comment: explains a non-obvious constraint
// Must run before session.init() or the auth token is stale
const user = await getUser(id);
```

### Invisible Evidence
Claiming a feature is complete without showing the verification output. Every "done" claim needs a test pass output, a screenshot path, or an explicit "Prototype — happy path manually verified: [description]."

## Documentation Anti-Patterns

### Unverifiable Criterion
Acceptance criteria written as feelings, not facts:

- Bad: "Users feel the checkout is fast"
- Good: "Checkout flow completes in < 3s on Slow 4G (Lighthouse throttled)"

### Missing Exclusions
A scope document without explicit out-of-scope boundaries. Without exclusions, scope expands to fill available time.

### Tautological Done
"Done when the feature is implemented." This criterion is always true at the moment it's evaluated.

### Aspirational Summary
SUMMARY.md that describes what was intended to be built rather than what was actually built. Summaries must reflect reality, not plans.

## Detection Checklist

**Before declaring done, scan for:**
- [ ] Any try/catch that swallows errors silently
- [ ] Any function whose entire body is a call to another function (wrapper bloat)
- [ ] Any null check on a value that cannot be null at that point
- [ ] Any feature flag older than 30 days in code marked as shipped
- [ ] Any comment that explains WHAT instead of WHY
- [ ] Any "should work" / "looks correct" claim in output
- [ ] Any acceptance criterion written as a feeling

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "The wrapper makes future changes easier" | Future changes that never come are not a justification. Remove it when it adds no current value. |
| "Silent failure is fine here — it's not critical" | Every silent failure is a future mystery. Log at minimum. |
| "These two things are similar, let's abstract" | Wait for the third instance. Two similar things might diverge. |
| "I'll clean up the feature flags later" | Add the removal ticket now. Later never comes. |
| "I said it works — that's evidence" | Show the output. Claims are not evidence. |
