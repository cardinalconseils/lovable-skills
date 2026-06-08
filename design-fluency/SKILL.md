---
name: design-fluency
description: Use when the user wants to audit UI quality, eliminate visual slop, improve visual design, run the impeccable linter, or apply design verbs to improve a screen. Also use when the user mentions 'impeccable,' 'visual slop,' 'AI-generated design,' 'design quality,' 'design looks generic,' 'design looks like AI,' 'fix the design,' 'design review,' 'typography choices,' or 'how do I make this look less like an AI built it.'
---

# Design Fluency

Design vocabulary, visual-slop detection, and fix patterns for AI-generated UIs — so the product looks intentional, not default.

> Attribution: Visual slop signals adapted from [impeccable](https://github.com/pbakaus/impeccable) by Peter Bakaus, Apache-2.0.

## Why This Exists

AI tools converge on the same visual defaults: Inter or Plus Jakarta Sans, purple-to-violet gradients, nested cards with thick left borders, icon tiles stacked above headings. The result is a recognizable aesthetic that signals "AI built this" before a user reads a word.

Design fluency is the ability to recognize these patterns and fix them with precision. The fix is never "make it prettier" — it's a specific verb applied to a specific signal.

## The Linter

No install needed. Run against any file path or directory:

```bash
npx impeccable detect src/
npx impeccable detect src/components/Card.tsx
npx impeccable detect dist/index.html
```

Each finding has: `id` (signal name), `file`, `line`, `message`. Map `id` to the signal tables below for the fix verb.

**Rule:** Run the linter before declaring any UI screen complete. Zero slop-category findings is the bar.

## Design Verbs

Every fix maps to a verb. Use the verb to name the change:

| Verb | When to use | Effect |
|---|---|---|
| `bolder` | Design feels safe, bland, or invisible | Increase visual weight — bigger type, stronger color, decisive hierarchy |
| `quieter` | Design is noisy, cluttered, overwhelming | Reduce visual noise — mute colors, tighten spacing, cut decoration |
| `distill` | Too many cards, labels, sections, or elements | Strip until nothing left to cut — one idea per section |
| `polish` | Structurally sound but unrefined | Micro-details — spacing rhythm, alignment, hover states, kerning |
| `clarify` | Hard to scan or understand | Stronger size contrast, better labels, fix line length, fix hierarchy |
| `animate` | Needs life or feedback cues | Purposeful motion — enter/exit transitions, micro-interactions |
| `harden` | Works for happy path only | Edge cases — error states, empty states, overflow, dark mode, i18n |

## Visual Slop Signals: AI Category

These are the AI-generated UI tells. Eliminate all of them.

| Signal | What it looks like | Fix verb |
|---|---|---|
| `side-tab` | Thick colored left border on a card — the #1 AI UI tell | `distill` — remove; use full border or background tint |
| `border-accent-on-rounded` | Thick accent border on a rounded card — border clashes with radius | `distill` — remove the border or the radius |
| `gradient-text` | `background-clip: text` with a gradient | `distill` — solid color text only |
| `nested-cards` | Cards inside cards | `distill` — flatten; use spacing, type hierarchy, dividers |
| `overused-font` | Inter, Roboto, Geist, Fraunces, Plus Jakarta Sans, Space Grotesk | `bolder` — choose a face with genuine personality |
| `single-font` | One font for everything, no typographic contrast | `clarify` — pair a display face with a body face |
| `flat-type-hierarchy` | Font sizes too close together, no visual hierarchy | `clarify` — fewer sizes, at least 1.25× ratio between steps |
| `ai-color-palette` | Purple/violet gradients, cyan-on-dark | `bolder` — use a distinctive intentional palette |
| `monotonous-spacing` | Same spacing value used everywhere, no rhythm | `polish` — tight groups for related items, generous gaps between sections |
| `everything-centered` | Every text element center-aligned | `clarify` — left-align body; center only hero and CTA |
| `icon-tile-stack` | Rounded-square icon above heading — universal AI feature card | `distill` — side-by-side icon+heading or inline icon |
| `italic-serif-display` | Oversized italic serif (Fraunces, Recoleta) as hero headline | `bolder` — set roman, or use a non-serif display face |
| `hero-eyebrow-chip` | Tiny uppercase tracked label or pill chip above hero headline | `distill` — drop the eyebrow; integrate into headline |
| `repeated-section-kickers` | Tiny uppercase label above every section heading | `distill` — replace with stronger structure or imagery |
| `bounce-easing` | Bounce or elastic CSS easing on any animation | `animate` — use ease-out-quart, ease-out-quint, or ease-out-expo |
| `dark-glow` | Dark background with colored box-shadow glows | `quieter` — subtle lighting or remove dark theme |

## Visual Slop Signals: Quality Category

These are accessibility and craft issues. Fix critical severity before launch.

| Signal | What it looks like | Fix verb |
|---|---|---|
| `low-contrast` | Text failing WCAG AA (< 4.5:1 body, < 3:1 large) | `harden` — increase contrast |
| `pure-black-white` | Pure `#000000` or `#ffffff` backgrounds | `polish` — tint toward brand hue: `oklch(12% 0.01 250)` |
| `gray-on-color` | Gray text on a colored background | `clarify` — use darker shade of background color |
| `tight-leading` | Line height below 1.3× font size | `clarify` — use 1.5–1.7 for body text |
| `line-length` | Body text wider than ~80 characters | `clarify` — add `max-width: 65ch` to text containers |
| `cramped-padding` | Text too close to container edge | `polish` — minimum 8px, ideally 12–16px padding |
| `body-text-viewport-edge` | Body text flush against viewport edge | `harden` — wrap in container with 16–32px horizontal padding |
| `tiny-text` | Body text below 12px | `harden` — minimum 14px body, 16px preferred |
| `all-caps-body` | Long passages in uppercase | `clarify` — uppercase for short labels only |
| `wide-tracking` | Letter spacing above 0.05em on body text | `polish` — reserve wide tracking for uppercase labels |
| `justified-text` | Justified text without hyphenation | `clarify` — use `text-align: left` for body |
| `skipped-heading` | Heading levels skip (h1 then h3) | `harden` — fix document outline for screen reader navigation |
| `layout-transition` | Animating `width`, `height`, `padding`, `margin` | `animate` — use `transform`/`opacity` instead |

## How to Run a Design Review

1. Run `npx impeccable detect src/` — collect all findings
2. Group findings by verb (`distill`, `clarify`, `bolder`, etc.)
3. Address all slop-category findings — no exceptions
4. Address quality-category findings at critical severity
5. For each change, name the verb: "Applied `distill` to remove side-tab borders"
6. Re-run linter — confirm finding count is zero for slop category

## Typography Quick Reference

**Font pairing pattern:** One display face for headings, one text face for body. Never the same face at different sizes only.

**Avoid (AI defaults):** Inter, Roboto, Geist, Plus Jakarta Sans, Space Grotesk, Fraunces, Recoleta

**Size scale rule:** Use a modular scale (1.25, 1.333, or 1.5 ratio). Never pick sizes by feel. Example at 1.25:
- xs: 12px, sm: 14px, base: 16px, lg: 20px, xl: 25px, 2xl: 31px, 3xl: 39px

**Line height:** 1.5–1.7 for body, 1.1–1.3 for headings.

## Color Quick Reference

**Use OKLCH for color definitions** — perceptually uniform, predictable lightness:
```css
/* Brand primary */
--color-primary: oklch(55% 0.20 250);
/* Lighter tint — same hue, adjust L and C */
--color-primary-subtle: oklch(92% 0.05 250);
```

**Avoid:** `purple`, `violet`, `#7c3aed`, `#8b5cf6`, `#06b6d4` (cyan-on-dark) — the AI default palette.

**Contrast check:** `oklch(L% C H)` — if L < 40% on light background or L > 70% on dark background, check contrast ratio.

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "Inter is fine — everyone uses it" | That's the problem. Every AI UI uses it. It signals default, not intent. |
| "Gradient text looks premium" | It reads as AI-generated to any design-literate user. Solid color is more premium. |
| "Nested cards add structure" | Nested cards add noise. Spacing and type hierarchy add structure. |
| "The linter is too strict" | Each rule maps to a documented AI-slop tell or accessibility standard. Challenge with evidence. |
| "We'll fix design slop after MVP" | Slop trains users to expect low quality. First impressions are product. |
| "This design is intentional" | Intentional means you can explain the tradeoff. If you can't explain it, it's accidental. |
| "Users don't notice these things" | Users can't name what they notice. They notice the feeling. 'Feels cheap' = slop. |

## Verification

- [ ] `npx impeccable detect` run — output parsed
- [ ] All slop-category findings addressed (or explicitly accepted with rationale)
- [ ] Quality-category findings at critical severity addressed
- [ ] Every design change named with a verb (`distill`, `clarify`, etc.)
- [ ] Font pair defined: display face + body face (not Inter-only)
- [ ] Color palette uses intentional brand hues (not AI purple/violet defaults)
- [ ] OKLCH used for color definitions
- [ ] Body text contrast ≥ 4.5:1 (WCAG AA)
- [ ] Body line height ≥ 1.5
- [ ] No layout-transition animations (use transform/opacity only)
