---
name: ux-design
description: Use when the user wants to design a user interface, create a user flow, improve usability, design empty states or error states, or make UX decisions about navigation or interaction patterns. Also use when the user mentions 'user flow,' 'wireframe,' 'usability,' 'UX review,' 'how should this screen work,' 'information architecture,' 'navigation design,' 'onboarding flow,' or 'how do I design this."
---

# UX Design

Designs interfaces that users understand without instructions — clear flows, honest feedback, and zero unnecessary steps between intent and outcome.

## User Flow Before Screens

Never design screens before drawing the flow. A screen is a step in a flow — designing it in isolation produces screens that don't connect.

**User flow anatomy:**
```
[Trigger] → [Entry point] → [Step 1] → [Decision] → [Step 2A / Step 2B] → [Outcome]
                                              ↓
                                         [Error path] → [Recovery]
```

**Draw every flow with:**
- The trigger (what makes the user start?)
- The happy path (fewest steps to the outcome)
- The error path (what happens when something fails?)
- The exit (can they leave without completing? what happens to their data?)

**Rule:** If the happy path has more than 5 steps, it will lose users. Find the step to cut.

## Nielsen's 10 Usability Heuristics (applied)

| Heuristic | Practical test |
|---|---|
| Visibility of system status | Does the user always know what the system is doing? (loading, saving, done) |
| Match with real world | Do labels use words the user would use — not internal jargon? |
| User control and freedom | Can they undo the last action? Can they exit without penalty? |
| Consistency | Does the same action look and work the same everywhere? |
| Error prevention | Does the UI prevent the error before it happens? (confirm dialogs, disabled states) |
| Recognition over recall | Is everything the user needs visible — not hidden in memory? |
| Flexibility | Can power users shortcut? (keyboard shortcuts, bulk actions) |
| Aesthetic and minimalist | Is every element on screen earning its place? |
| Error recovery | Are error messages specific and actionable — not "Something went wrong"? |
| Help and documentation | If help is needed, is it contextual and at the point of confusion? |

## The Four States Every Screen Needs

Design all four before handing off to engineering:

**1. Loading state** — what do users see while data fetches?
- Use skeletons for content-heavy screens
- Use spinners for actions (form submit, file upload)
- Never show a blank screen

**2. Empty state** — what do users see when there's nothing yet?
- Explain why it's empty ("You haven't created any projects yet")
- Offer the next action ("Create your first project")
- Use illustration only if it adds warmth, not for decoration

**3. Error state** — what do users see when something fails?
- Name the problem specifically (not "Error 500")
- Tell them what to do ("Try again" or "Contact support")
- Offer a way out (back button, retry)

**4. Populated/full state** — what do users see when the screen has real content?
- Design with realistic data, not "Lorem ipsum" or "User 1"
- Test with long names, long text, many items

## Information Architecture

**Navigation depth rule:** Users should reach any screen in ≤ 3 clicks from the home screen.

**Primary navigation:** 3–7 items. More than 7 = you haven't prioritized.

**Label test:** Show navigation labels to 5 users. If 3+ can't predict what's behind a label — rename it.

**Group by user task, not by your org chart.** Users don't care about your team structure.

## Interaction Patterns

**Confirmation dialogs:** Only for irreversible destructive actions (delete, cancel subscription). Overuse causes dialog blindness — users click "Confirm" without reading.

**Inline validation:** Validate on blur (when user leaves field), not on submit. Show success as well as errors.

**Progressive disclosure:** Show only what's needed now. Reveal complexity as the user opts in.

**Affordances:** Clickable things look clickable. Static things look static. Never make the user guess.

## Accessibility Baseline (WCAG 2.1 AA)

- Color contrast ratio ≥ 4.5:1 for body text, ≥ 3:1 for large text
- Every interactive element reachable by keyboard (Tab order logical)
- Every image has descriptive alt text
- Error messages not communicated by color alone
- Focus indicator visible at all times

**Rule:** Accessibility is not a post-launch audit. Wire it into component design from day one.

## Copywriting in UX

UI copy is UX. Bad copy makes good design feel broken.

| Bad | Good |
|---|---|
| "Error occurred" | "Couldn't save — check your internet connection and try again" |
| "Are you sure?" | "Delete this project? This can't be undone." |
| "Submit" | "Save changes" |
| "Invalid input" | "Email must include @" |
| "Loading..." | "Loading your projects..." |

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "Users will figure it out" | Users leave. They don't figure it out — they leave. |
| "We'll add the empty state later" | Empty state is the first thing new users see. It's not an edge case. |
| "The design is clean — fewer elements is always better" | Minimalism serves usability. Removing help text to look clean is anti-UX. |
| "We'll make it accessible in v2" | Retrofitting accessibility costs 10× what building it in costs. |
| "Our users are tech-savvy, they don't need obvious UI" | Tech-savvy users have zero patience for bad UX. The bar is higher, not lower. |

## Verification

- [ ] User flow drawn before any screen is designed (happy path + error path)
- [ ] Happy path ≤ 5 steps from trigger to outcome
- [ ] All four states designed: loading, empty, error, populated
- [ ] Navigation labels tested with real users (or the 5-user label test applied)
- [ ] Confirmation dialogs used only for irreversible destructive actions
- [ ] Error messages specific and actionable — no generic "Something went wrong"
- [ ] Color contrast ≥ 4.5:1 for body text
- [ ] All interactive elements keyboard-accessible
