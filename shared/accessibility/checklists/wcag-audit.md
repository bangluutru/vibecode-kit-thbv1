# WCAG 2.2 AA Audit Checklist

## 1. Perceivable

### 1.1 Text Alternatives
- [ ] All `<img>` have meaningful `alt` (or `alt=""` for decorative)
- [ ] All `<svg>` have `<title>` or `aria-label`
- [ ] All `<video>` have captions (`<track kind="captions">`)
- [ ] All `<audio>` have transcripts
- [ ] Icon-only buttons have `aria-label`

### 1.2 Time-based Media
- [ ] Video has captions
- [ ] Video has audio description (for visual-only content)
- [ ] Live streams have real-time captions

### 1.3 Adaptable
- [ ] Semantic HTML used (`<nav>`, `<main>`, `<article>`, `<aside>`, `<footer>`)
- [ ] Heading hierarchy correct (`h1` → `h2` → `h3`, no skipping)
- [ ] Reading order matches visual order in DOM
- [ ] `<table>` has `<th>` with `scope` attribute
- [ ] `<fieldset>` + `<legend>` for related form groups

### 1.4 Distinguishable
- [ ] Text contrast ≥ 4.5:1 (normal), ≥ 3:1 (large/bold)
- [ ] UI component contrast ≥ 3:1
- [ ] Text resizable to 200% without loss
- [ ] No images of text (use real text)
- [ ] User can override colors (no `!important` on text colors)

---

## 2. Operable

### 2.1 Keyboard Accessible
- [ ] All interactive elements reachable via Tab
- [ ] Tab order follows visual reading order
- [ ] No keyboard traps
- [ ] Custom keyboard shortcuts can be disabled
- [ ] Focus indicator visible (`:focus-visible`)

### 2.2 Enough Time
- [ ] No time limits (or user can extend/disable)
- [ ] Auto-scrolling/auto-updating can be paused
- [ ] Timeout warnings given 20s before expiry

### 2.3 Seizures & Physical Reactions
- [ ] No content flashes > 3 times/second
- [ ] `prefers-reduced-motion` respected

### 2.4 Navigable
- [ ] Skip-to-content link present
- [ ] Page `<title>` is descriptive and unique
- [ ] Focus order is logical
- [ ] Link text is descriptive (no "click here")
- [ ] Multiple ways to find pages (nav, search, sitemap)
- [ ] Headings describe content sections

### 2.5 Input Modalities
- [ ] Touch targets ≥ 44×44 CSS pixels
- [ ] Drag actions have alternative (button, keyboard)
- [ ] No motion-activated functions (or can be disabled)

---

## 3. Understandable

### 3.1 Readable
- [ ] Page language declared (`<html lang="vi">`)
- [ ] Language changes marked (`<span lang="en">`)
- [ ] Abbreviations expanded on first use

### 3.2 Predictable
- [ ] Consistent navigation across pages
- [ ] Consistent identification of UI elements
- [ ] No context change on focus or input (without warning)

### 3.3 Input Assistance
- [ ] Error messages describe the problem
- [ ] Error messages suggest how to fix
- [ ] Labels present for all form inputs
- [ ] Required fields marked (`aria-required="true"`)
- [ ] Autocomplete attributes on personal data fields
- [ ] Submissions can be reviewed/confirmed/reversed

---

## 4. Robust

### 4.1 Compatible
- [ ] HTML validates (no duplicate IDs, proper nesting)
- [ ] ARIA roles, states, properties used correctly
- [ ] Status messages announced to assistive tech (`aria-live`)
- [ ] Custom components have proper name, role, value

---

## Scoring

| Category | Items Passed | Total | Score |
|----------|-------------|-------|-------|
| Perceivable | | /14 | % |
| Operable | | /16 | % |
| Understandable | | /11 | % |
| Robust | | /4 | % |
| **Overall** | | **/45** | **%** |

**Compliance**: ≥ 95% = ✅ AA Compliant | 80-94% = ⚠️ Partial | < 80% = ❌ Non-compliant
