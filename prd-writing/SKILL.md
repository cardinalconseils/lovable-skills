---
name: prd-writing
description: Use when the user wants to write a product requirements document, spec a feature, define acceptance criteria, or communicate what needs to be built to engineers and designers. Also use when the user mentions 'PRD,' 'product spec,' 'requirements,' 'acceptance criteria,' 'feature brief,' 'what to build,' or 'definition of done.'
---

# PRD Writing

A PRD is a decision document, not a feature wish list. It exists to align the team on what success looks like before anyone writes code.

## PRD Structure

### 1. Problem Statement (not the solution)
What is the customer problem? Who has it? How do we know it's real?
- Evidence: research quotes, support tickets, metric data
- Scope: who specifically has this problem (not everyone)

### 2. Goal and Success Metric
One primary metric that moves if this ships successfully.
- "Increase trial-to-paid conversion from 18% to 25%"
- Not: "improve user experience"

### 3. Non-Goals (explicit)
What this does NOT do. Saves more arguments than any other section.

### 4. User Stories / Job Stories
How the user's situation changes. Format:
"When [context], I want to [action], so I can [outcome]."

### 5. Acceptance Criteria
Verifiable conditions that define done. Each criterion must be testable true/false.

**Good AC:** "User can export a CSV of all campaigns filtered by date range. Export completes in < 3 seconds for up to 10,000 rows."
**Bad AC:** "Export works well and is fast."

### 6. Edge Cases and Out of Scope
State what happens in the top 3 edge cases. Unanswered edge cases become last-minute scope creep.

### 7. Open Questions
Documented unknowns. Each has an owner and a decision date.

## Writing Good Acceptance Criteria

Each AC should answer: how will we know this is working?

| Pattern | Example |
|---|---|
| Given/When/Then | Given a logged-in user, when they click Export, then a CSV downloads within 3s |
| Measurable threshold | Page loads in < 2s on 3G connection |
| State change | User status changes from trial to paid after checkout completes |
| Error handling | If payment fails, user sees error message X and is not charged |

## Definition of Done Checklist

- [ ] All acceptance criteria pass
- [ ] Edge cases handled (or explicitly deferred with ticket)
- [ ] Error states designed and implemented
- [ ] Mobile / responsive verified if applicable
- [ ] Analytics events firing for key actions
- [ ] Reviewed by design and engineering lead before ship

## PRD Anti-Patterns

| Anti-pattern | Why it breaks |
|---|---|
| Solution in the problem statement | Locks the team out of better solutions |
| "Users should be able to..." acceptance criteria | "Should" is not testable. Use "can" with a measurable condition. |
| No non-goals section | Everything becomes in scope when no one says no |
| Open questions without owners | They stay open until launch, then become fires |
| Success metric is an output ("ship the feature") | Outputs don't measure value. Use outcomes. |

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "We don't have time to write a PRD" | You'll spend the time anyway — either in planning or in rework. |
| "Everyone knows what we're building" | They know the feature name. They disagree on the details. Write it down. |
| "Acceptance criteria are too restrictive" | Vague criteria produce vague outcomes. Specific criteria produce shippable software. |

## Verification

- [ ] Problem statement includes evidence (not assumption)
- [ ] Success metric is measurable and outcome-based
- [ ] Non-goals section explicitly names at least 2 things this won't do
- [ ] All acceptance criteria are testable true/false
- [ ] Open questions have owners and decision dates
