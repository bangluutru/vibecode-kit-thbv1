---
name: accessibility-patterns
description: "WCAG 2.2 Compliance, ARIA Patterns, Screen Reader Testing, Inclusive Design"
---

# ♿ Accessibility Patterns

> Patterns and checklists for building WCAG 2.2 AA compliant web applications.

## Semantic HTML First

### Use native elements instead of ARIA

```html
<!-- ✅ GOOD: Native semantics -->
<button onclick="submit()">Submit</button>
<nav aria-label="Main">...</nav>
<main>...</main>
<footer>...</footer>

<!-- ❌ BAD: ARIA on generic elements -->
<div role="button" tabindex="0" onclick="submit()">Submit</div>
<div role="navigation">...</div>
```

### Landmark Structure

```html
<body>
  <header>
    <nav aria-label="Main navigation">...</nav>
  </header>
  
  <a href="#main-content" class="skip-link">Skip to main content</a>
  
  <main id="main-content">
    <article>
      <h1>Page Title</h1>
      <section aria-labelledby="section-heading">
        <h2 id="section-heading">Section Title</h2>
      </section>
    </article>
  </main>
  
  <aside aria-label="Related content">...</aside>
  
  <footer>...</footer>
</body>
```

---

## ARIA Patterns Library

### Modal Dialog

```html
<div role="dialog" aria-modal="true" aria-labelledby="dialog-title">
  <h2 id="dialog-title">Confirm Delete</h2>
  <p>Are you sure you want to delete this item?</p>
  <button autofocus>Cancel</button>
  <button>Delete</button>
</div>

<script>
// Trap focus inside modal
// Return focus to trigger element on close
// Close on Escape key
</script>
```

### Tabs

```html
<div role="tablist" aria-label="Settings">
  <button role="tab" aria-selected="true" aria-controls="panel-1" id="tab-1">
    General
  </button>
  <button role="tab" aria-selected="false" aria-controls="panel-2" id="tab-2">
    Security
  </button>
</div>

<div role="tabpanel" id="panel-1" aria-labelledby="tab-1">
  General settings content
</div>
<div role="tabpanel" id="panel-2" aria-labelledby="tab-2" hidden>
  Security settings content
</div>
```

### Toast/Alert

```html
<!-- Polite: announced when user is idle -->
<div role="status" aria-live="polite">
  Settings saved successfully
</div>

<!-- Assertive: announced immediately -->
<div role="alert" aria-live="assertive">
  Error: Please fix the form errors below
</div>
```

### Accordion

```html
<div>
  <h3>
    <button aria-expanded="true" aria-controls="section-1-content">
      Section 1 Title
    </button>
  </h3>
  <div id="section-1-content">
    Section 1 content here
  </div>
</div>
```

---

## Form Accessibility

### Labels and Errors

```html
<div class="form-group">
  <label for="email">Email address <span aria-hidden="true">*</span></label>
  <input 
    type="email" 
    id="email" 
    required
    aria-required="true"
    aria-invalid="true"
    aria-describedby="email-help email-error"
  />
  <p id="email-help" class="help-text">We'll never share your email.</p>
  <p id="email-error" class="error" role="alert">
    Please enter a valid email address (e.g., user@domain.com)
  </p>
</div>
```

### Form Validation

```javascript
function validateForm(form) {
  const errors = [];
  
  // Set focus on first error
  const firstError = form.querySelector('[aria-invalid="true"]');
  if (firstError) firstError.focus();
  
  // Announce error count to screen readers
  const liveRegion = document.getElementById('form-status');
  liveRegion.textContent = `${errors.length} errors found. Please fix them.`;
}
```

---

## CSS for Accessibility

```css
/* Skip link (visible on focus) */
.skip-link {
  position: absolute;
  left: -10000px;
  top: auto;
  width: 1px;
  height: 1px;
  overflow: hidden;
}
.skip-link:focus {
  position: static;
  width: auto;
  height: auto;
  padding: 8px 16px;
  background: #000;
  color: #fff;
  z-index: 9999;
}

/* Focus visible */
:focus-visible {
  outline: 3px solid #4A90D9;
  outline-offset: 2px;
}

/* Reduce motion */
@media (prefers-reduced-motion: reduce) {
  * {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}

/* High contrast mode support */
@media (forced-colors: active) {
  .button {
    border: 2px solid ButtonText;
  }
}

/* Screen reader only text */
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border-width: 0;
}
```

---

## Testing Commands

```bash
# Automated
npx @axe-core/cli https://your-site.com
npx pa11y https://your-site.com --standard WCAG2AA
npx lighthouse https://your-site.com --only-categories=accessibility

# Manual checklist
# 1. Tab through entire page — focus visible?
# 2. Screen reader — content announced correctly?
# 3. 200% zoom — no horizontal scrolling?
# 4. Color contrast — ratios meet WCAG AA?
# 5. Keyboard only — all actions possible?
```
