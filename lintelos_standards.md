# Lintelos Website Code & Design Standards

**Version:** 1.0.0
**Last updated:** 2026-06-27

These are the official design and code standards for the Lintelos website. They define the
palette, typography, components, and patterns used across every page. Reference this
document when building or editing any page so the site stays consistent.

## Table of Contents
1. [Brand Identity & Colors](#brand-identity--colors)
2. [Typography](#typography)
3. [Layout & Spacing](#layout--spacing)
4. [Components](#components)
5. [Motion](#motion)
6. [Accessibility](#accessibility)
7. [File Organization](#file-organization)

---

## Brand Identity & Colors

Use CSS custom properties for all colors. These are the approved brand tokens.

```css
:root {
    /* Brand */
    --ivory:      #f5f3ee;
    --copper:     #d98238;
    --pale-olive: #cadba9;
    --avocado:    #a1ba56;
    --army:       #717a2c;

    /* Surfaces & neutrals */
    --paper:      #f5f3ee;  /* default page background */
    --paper-deep: #ece8df;  /* alternating section background */
    --ink:        #1c1d17;  /* headings / near-black, warm */
    --ink-soft:   #45473a;  /* body text */
    --muted:      #6f7163;  /* secondary / metadata text */
    --line:       #ddd8cb;  /* hairline borders */
    --sun:        #e2873a;  /* warm accent (brand sun) */
    --forest:     #3a3f22;  /* dark section background */
    --white:      #ffffff;
}
```

### Usage
- **Backgrounds:** alternate `--paper` and `--paper-deep` between sections. Dark sections
  (CTA, philosophy) use a `--forest` → `--ink` gradient.
- **Text:** `--ink` for headings, `--ink-soft` for body, `--muted` for metadata.
- **Accents:** `--army` for primary actions and eyebrow labels; `--copper`/`--sun` for the
  small accent rule, hover states, and the soft radial "sun" behind heroes.
- **Borders:** `--line` for all hairline borders; cards highlight to `--avocado` on hover.
- Never hardcode hex values; always reference tokens.

---

## Typography

```css
--font-display: 'Funnel Display', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
--font-body:    'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
--font-mono:    'JetBrains Mono', ui-monospace, 'SF Mono', Menlo, monospace;
```

Load once per document:

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Funnel+Display:wght@400;500;600;700;800&family=Inter:wght@400;500;600;700&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
```

- **`--font-display`** — all headings (h1, section titles, card titles). Matches the
  LINTELOS wordmark. Use weight 600, tight tracking (`letter-spacing: -0.015em to -0.02em`).
- **`--font-body`** — body copy, buttons, nav, form controls. Inter 400/500/600.
- **`--font-mono`** — eyebrow labels, numeric indices, footer headers, metadata. Always
  uppercase with `letter-spacing: 0.1em–0.2em`. This is the primary "technology in the
  architecture space" signal; use it deliberately, not decoratively.

### Scale (fluid)
```css
.hero h1       { font-size: clamp(2.7rem, 6.5vw, 5rem); }
.section-title { font-size: clamp(2rem, 4vw, 2.85rem); }
.card-title    { font-size: 1.4rem; }
body, p        { font-size: 1rem; line-height: 1.65; }
.eyebrow       { font-size: 0.78rem; }
```

---

## Layout & Spacing

```css
.container      { max-width: 1180px; margin: 0 auto; padding: 0 2rem; }
.section-padding{ padding: 7rem 0; }              /* 4–5rem on mobile */
```

- Card/feature grids use explicit `repeat(n, 1fr)` columns that collapse to 1 column at
  `968px` / `768px` (avoid `auto-fit` so column counts stay intentional).
- Generous whitespace is part of the aesthetic; do not crowd sections.

---

## Components

### Eyebrow label
Monospace kicker above every section title.
```css
.eyebrow {
    font-family: var(--font-mono); font-size: 0.78rem; font-weight: 500;
    letter-spacing: 0.2em; text-transform: uppercase; color: var(--army);
    display: inline-flex; align-items: center; gap: 0.6rem;
}
.eyebrow::before { content: ''; width: 26px; height: 1.5px; background: var(--copper); }
```

### Buttons
- **Primary:** `background: var(--army)`, white text; hover → `--forest`, lift 2px, shadow.
- **Ghost:** transparent, `2px solid var(--ink)`; hover fills with `--ink`.
- Include a `<span class="btn-arrow">&rarr;</span>` that nudges right on hover.

### Cards (benefit / mission / service / product)
- `background: var(--white)`, `1px solid var(--line)`, `border-radius: 14px`.
- Monospace index (`01`, `02`, …) in `--copper` above a hairline divider — no emoji.
- Hover: `translateY(-4px)`, `border-color: var(--avocado)`, soft shadow.

### Callout / Feature
- `.callout` — white panel with a 4px `--copper` left rule for emphasis blocks.
- `.feature` — two-column white panel (text + check-style `.feature-list`).

### Process timeline
- Numbered circles outlined in `--army` (mono numerals) connected by a `--line` rail.

### Dark sections
- `linear-gradient(135deg, var(--forest), var(--ink))` with a low-opacity `--sun` radial
  in a corner. Eyebrow turns `--avocado`.

### Iconography
- No emoji. Use typographic indices, hairline detailing, and monospace labels. Functional
  third-party glyphs (e.g. the YouTube play icon on tool demos) are the only exception.

---

## Motion

```css
.reveal { opacity: 0; transform: translateY(24px); transition: opacity .6s ease, transform .6s ease; }
.reveal.is-visible { opacity: 1; transform: none; }
```
- Reveal on scroll via `IntersectionObserver`; stagger with small `transition-delay`.
- Transitions are 0.25s (interactive) / 0.6s (reveal). Respect `prefers-reduced-motion`.

---

## Accessibility
- Maintain heading hierarchy; use semantic landmarks (`header`, `nav`, `main`, `footer`).
- All interactive elements get a visible `:focus-visible` outline (`--army` / `--ink`).
- Color contrast meets WCAG AA (`--ink-soft` on `--paper`, white on `--army`/`--forest`).
- Honor `prefers-reduced-motion` and `prefers-contrast`.

---

## File Organization
- Each page is a self-contained `.html` file with an embedded `<style>` block carrying the
  full token + component system (no shared external CSS, matching the deploy).
- `header.html` and `footer.html` are shared, injected components — keep their logo image
  URLs, nav links, and JS intact when editing.
- The Archi-Flow page is a content fragment (no `<head>`/`<body>`); its tokens and styles
  are scoped under `.lintelos-revit-addins-container` so they don't leak when embedded.
- Preserve integrations on edit: Calendly (`openCalendly`), the Formspree endpoint, the
  Stripe checkout/billing links, phone `tel:+17274704161`, and `mailto:support@lintelos.com`.

This document is the reference for every new page, component, or feature to keep brand and
code consistency across the Lintelos website.
