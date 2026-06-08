---
name: accessibility
description: Use when the user wants to make their app accessible, fix accessibility issues, meet WCAG standards, or support screen readers and keyboard navigation. Also use when the user mentions 'a11y', 'WCAG', 'screen reader', 'keyboard navigation', 'color contrast', 'ARIA', or 'accessibility audit'.
---

# Accessibility (a11y)

Expert knowledge for building WCAG 2.1 AA-compliant web applications. Accessibility is not a feature — it is a quality dimension that benefits all users.

## WCAG 2.1 AA — The Target

**4 principles:** Perceivable, Operable, Understandable, Robust (POUR)

**Level AA compliance** is the legal standard in most jurisdictions (EU, Canada, US Section 508) and the minimum for Candidate/Production maturity.

**Key AA requirements:**
- Color contrast ≥ 4.5:1 for normal text, ≥ 3:1 for large text (≥ 18px regular or ≥ 14px bold)
- All functionality available via keyboard
- No keyboard traps
- Focus visible at all times
- Images have meaningful alt text (or `alt=""` for decorative)
- Form inputs have associated labels
- Error messages identify the field and describe the fix
- No content that flashes > 3 times per second

## Semantic HTML First

The single highest-leverage accessibility improvement is using correct HTML elements.

```html
<!-- Bad: div soup -->
<div class="button" onclick="submit()">Submit</div>
<div class="nav"><div class="link">Home</div></div>

<!-- Good: semantic HTML -->
<button type="submit">Submit</button>
<nav><a href="/">Home</a></nav>
```

**Rule:** If a native HTML element does what you need, use it. ARIA only when native semantics are insufficient.

**Semantic landmarks:**
```html
<header>  <!-- site header -->
<nav>     <!-- navigation -->
<main>    <!-- primary content, one per page -->
<aside>   <!-- supplementary content -->
<footer>  <!-- site footer -->
```

## Keyboard Navigation

**Focus management rules:**
- Interactive elements (buttons, links, inputs, selects) receive focus natively — don't add `tabindex` unless you know why
- `tabindex="0"` makes a non-interactive element focusable (use sparingly)
- `tabindex="-1"` makes an element programmatically focusable but removes it from tab order (use for focus management in modals/drawers)
- Never `tabindex > 0` — breaks natural tab order

**Modal / dialog focus trap:**
```javascript
// When modal opens: move focus to modal container
modalRef.current.focus();
// Trap Tab/Shift+Tab within modal while open
// When modal closes: return focus to trigger element
triggerRef.current.focus();
```

**Keyboard interactions by component:**
| Component | Expected keyboard behavior |
|---|---|
| Button | Enter or Space activates |
| Link | Enter navigates |
| Dropdown/Select | Arrow keys navigate options, Enter selects, Escape closes |
| Modal | Escape closes, focus trapped inside, focus returns to trigger on close |
| Accordion | Enter/Space toggles, Arrow keys navigate between headers |
| Tab panel | Arrow keys navigate tabs, Enter/Space activates |

## Screen Reader Support

**Test with real screen readers:**
- macOS/iOS: VoiceOver (built-in, free)
- Windows: NVDA (free), JAWS (paid)
- Android: TalkBack

**ARIA usage rules:**
1. No ARIA > bad ARIA. Wrong ARIA is worse than no ARIA.
2. Never override native semantics unnecessarily: `<button role="button">` is redundant.
3. Always pair `aria-labelledby` or `aria-label` with landmark roles and interactive widgets.
4. Use `aria-live` regions for dynamic content that updates without page reload.

**Common ARIA patterns:**
```html
<!-- Icon-only button -->
<button aria-label="Close dialog">
  <svg aria-hidden="true">...</svg>
</button>

<!-- Loading state -->
<div aria-live="polite" aria-atomic="true">
  <!-- Screen reader announces changes here -->
</div>

<!-- Error message -->
<input aria-describedby="email-error" aria-invalid="true" />
<p id="email-error" role="alert">Enter a valid email address</p>
```

## Color and Visual Design

**Contrast checker:** Use Colour Contrast Analyser (desktop) or browser DevTools accessibility panel.

**Never use color alone** to convey information:
```html
<!-- Bad: red text only -->
<span style="color: red">Error</span>

<!-- Good: icon + color + text -->
<span class="error">
  <svg aria-hidden="true"><!-- error icon --></svg>
  Error: email is required
</span>
```

**Focus indicator:** Default browser focus rings are often removed by CSS resets. Always provide a visible focus style:
```css
:focus-visible {
  outline: 2px solid #0066cc;
  outline-offset: 2px;
}
```

## Forms

```html
<!-- Every input needs an associated label -->
<label for="email">Email address</label>
<input id="email" type="email" autocomplete="email" required />

<!-- Group related inputs -->
<fieldset>
  <legend>Shipping address</legend>
  <!-- address fields -->
</fieldset>

<!-- Inline error -->
<input aria-describedby="email-error" aria-invalid="true" />
<span id="email-error">Enter a valid email address</span>
```

**Don't rely on placeholder text as a label** — placeholders disappear on focus and have low contrast.

## Motion and Animation

```css
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
```

Honor this media query for all decorative animations. Functional animations (loading spinners) may be kept but slowed.

## Audit Tools

| Tool | Type | Use for |
|---|---|---|
| axe DevTools (browser extension) | Automated | Catch ~30% of issues fast |
| Lighthouse Accessibility | Automated | CI integration, score tracking |
| VoiceOver / NVDA | Manual | Real screen reader experience |
| Colour Contrast Analyser | Manual | Precise contrast ratios |
| Keyboard-only navigation | Manual | Tab order, focus traps, shortcuts |

Automated tools catch roughly 30-40% of WCAG issues. Manual testing is required for full compliance.

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "Our users don't use screen readers" | 1 in 4 adults has a disability. Many disabilities are invisible. You can't know your users' needs. |
| "We'll add accessibility later" | Retrofitting is 3-10× more expensive than building accessibly from the start. |
| "ARIA fixes everything" | Incorrect ARIA breaks screen readers. Semantic HTML first, ARIA as last resort. |
| "Color contrast is a design decision" | 4.5:1 is a legal requirement in most jurisdictions, not a preference. |

## Verification

- [ ] axe DevTools or Lighthouse shows zero critical/serious accessibility violations
- [ ] All interactive elements reachable and operable via keyboard only
- [ ] All images have alt text (or `alt=""` for decorative)
- [ ] All form inputs have associated labels
- [ ] Color contrast ≥ 4.5:1 for body text, ≥ 3:1 for large text
- [ ] Focus indicator visible on all interactive elements
- [ ] `prefers-reduced-motion` honored for animations
- [ ] Modal/dialog focus trap implemented and tested
- [ ] Tested with VoiceOver (macOS) or NVDA (Windows) for key user flows
