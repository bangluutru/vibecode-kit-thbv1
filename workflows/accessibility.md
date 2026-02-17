---
description: Run accessibility audit (WCAG 2.2 AA compliance check)
---

# /accessibility — Accessibility Audit Workflow

## When to use
- Before every production deployment
- After major UI changes
- When building forms, modals, or navigation components
- During quarterly accessibility reviews

---

## Step 1: Automated Scan

Run automated accessibility testing tools:

```bash
# Lighthouse accessibility audit
npx lighthouse <url> --only-categories=accessibility --output=json

# axe-core comprehensive scan
npx @axe-core/cli <url> --exit

# pa11y WCAG 2.2 AA check
npx pa11y <url> --standard WCAG2AA --reporter json
```

Record: Score, number of violations, violation types.

---

## Step 2: Manual Keyboard Testing

Test the entire page flow using keyboard only:

1. [ ] Tab through all interactive elements — focus visible?
2. [ ] Enter/Space activates buttons and links?
3. [ ] Escape closes modals/dropdowns?
4. [ ] Skip-to-content link present and working?
5. [ ] Tab order follows visual order (no jumps)?
6. [ ] No keyboard traps (can always Tab out)?

---

## Step 3: Screen Reader Testing

Test with system screen reader:
- **macOS**: VoiceOver (Cmd + F5)
- **Windows**: NVDA (free download)

Check:
1. [ ] All images have meaningful alt text
2. [ ] Form inputs have labels announced
3. [ ] Headings announced in correct hierarchy
4. [ ] Live regions announce dynamic content
5. [ ] ARIA landmarks identified (main, nav, footer)
6. [ ] Error messages announced when they appear

---

## Step 4: Visual Checks

1. [ ] Color contrast ≥ 4.5:1 (normal text), ≥ 3:1 (large text)
2. [ ] Content readable at 200% zoom (no horizontal scrolling)
3. [ ] Focus indicators visible (outline or ring)
4. [ ] Information not conveyed by color alone
5. [ ] Touch targets ≥ 44x44 CSS pixels (mobile)
6. [ ] `prefers-reduced-motion` respected

---

## Step 5: Fix and Verify

For each violation found:
1. Classify severity: Critical → High → Medium → Low
2. Fix critical issues first
3. Re-run automated scan after fixes
4. Re-test manually for fixed items

---

## Step 6: Report

Generate accessibility audit report:

```markdown
# Accessibility Audit Report
Date: [date]
URL: [url]

## Scores
- Lighthouse Accessibility: [score]/100
- axe violations: [count]
- WCAG 2.2 AA compliance: [PASS/FAIL]

## Critical Issues
1. [Issue] — [Location] — [Fix applied]

## Remaining Issues
1. [Issue] — [Severity] — [Planned fix date]

## Recommendations
- [Future improvements]
```

---

## Quality Gates

| Metric | Required | Ideal |
|--------|----------|-------|
| Lighthouse a11y score | ≥ 80 | ≥ 95 |
| axe violations (critical) | 0 | 0 |
| Keyboard navigable | ✅ | ✅ |
| Screen reader compatible | ✅ | ✅ |
| Color contrast WCAG AA | ✅ | ✅ |
