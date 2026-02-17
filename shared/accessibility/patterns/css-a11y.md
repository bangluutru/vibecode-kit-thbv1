# Accessibility CSS Utilities

> Drop-in CSS classes and utilities for accessibility compliance.

## Screen Reader Only

```css
/* Visually hidden but accessible to screen readers */
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

/* Become visible when focused (skip links) */
.sr-only-focusable:focus,
.sr-only-focusable:active {
  position: static;
  width: auto;
  height: auto;
  padding: 0.5rem 1rem;
  margin: 0;
  overflow: visible;
  clip: auto;
  white-space: normal;
}
```

## Skip Link

```css
.skip-link {
  position: absolute;
  left: -10000px;
  top: auto;
  width: 1px;
  height: 1px;
  overflow: hidden;
  z-index: 9999;
}

.skip-link:focus {
  position: fixed;
  top: 0;
  left: 0;
  width: auto;
  height: auto;
  padding: 12px 24px;
  background: #1a1a1a;
  color: #ffffff;
  font-size: 1rem;
  font-weight: 600;
  text-decoration: none;
  z-index: 99999;
}
```

## Focus Indicators

```css
/* Keyboard-only focus (not on mouse click) */
:focus-visible {
  outline: 3px solid var(--focus-color, #4A90D9);
  outline-offset: 2px;
  border-radius: 2px;
}

/* Remove default focus ring on mouse click */
:focus:not(:focus-visible) {
  outline: none;
}

/* High contrast focus for dark backgrounds */
.dark :focus-visible {
  outline-color: #FFD700;
}
```

## Reduced Motion

```css
/* Respect user's motion preferences */
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}

/* Safe animation: only animate if user is OK with motion */
@media (prefers-reduced-motion: no-preference) {
  .animate-fade-in {
    animation: fadeIn 0.3s ease-in;
  }
  
  .animate-slide-up {
    animation: slideUp 0.3s ease-out;
  }
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

@keyframes slideUp {
  from { transform: translateY(10px); opacity: 0; }
  to { transform: translateY(0); opacity: 1; }
}
```

## High Contrast Mode

```css
/* Support Windows High Contrast Mode */
@media (forced-colors: active) {
  /* Ensure custom buttons have visible borders */
  .btn {
    border: 2px solid ButtonText;
  }
  
  /* Ensure custom focus is visible */
  :focus-visible {
    outline: 2px solid Highlight;
  }
  
  /* Ensure icons are visible */
  .icon {
    forced-color-adjust: auto;
  }
}
```

## Color Contrast Utilities

```css
/* Accessible color pairs (all ≥ 4.5:1 ratio) */
.text-on-light {
  color: #1a1a1a;       /* On white: 16.8:1 */
  background: #ffffff;
}

.text-on-dark {
  color: #f5f5f5;       /* On dark: 15.3:1 */
  background: #1a1a1a;
}

.text-muted {
  color: #555555;       /* On white: 7.0:1 ✅ */
}

.text-danger {
  color: #c62828;       /* On white: 6.3:1 ✅ */
}

.text-success {
  color: #2e7d32;       /* On white: 4.6:1 ✅ */
}

/* ❌ FAIL — Do NOT use these alone */
/* #999 on white = 2.8:1 ratio */
/* #ccc on white = 1.6:1 ratio */
```

## Touch Target Sizing

```css
/* Minimum 44x44 CSS pixels for touch targets */
button,
a,
input[type="checkbox"],
input[type="radio"],
select {
  min-height: 44px;
  min-width: 44px;
}

/* For inline links, add padding instead */
a {
  padding: 4px 2px;
}

/* Icon buttons need explicit sizing */
.icon-btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  min-width: 44px;
  min-height: 44px;
  padding: 8px;
}
```

## Print Accessibility

```css
@media print {
  /* Show URLs after links */
  a[href]::after {
    content: " (" attr(href) ")";
    font-size: 0.8em;
    color: #666;
  }
  
  /* Don't print navigation and decorative elements */
  nav, .skip-link, .toast, [aria-hidden="true"] {
    display: none !important;
  }
}
```
