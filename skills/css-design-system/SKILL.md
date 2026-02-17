---
name: css-design-system
description: Pure CSS design system architecture. Design tokens, CSS Variables, theming, responsive design, no Tailwind needed. Use when building custom design systems with vanilla CSS.
allowed-tools: Read, Write, Edit, Glob, Grep
version: 1.0.0
source: Synthesized from Smashing Magazine, Design Systems Collective, cursor.directory patterns
---

# CSS Design System Architecture

## Core Philosophy

1. **Tokens first** — define ALL values as CSS Variables
2. **Semantic naming** — name by purpose, not value (`--color-primary`, not `--blue-500`)
3. **3-tier architecture** — Primitive → Semantic → Component tokens
4. **Dark mode by default** — premium, modern feel
5. **Mobile-first** — start small, scale up

---

## File Architecture
```
css/
├── variables.css    ← Design tokens (THE source of truth)
├── base.css         ← Reset, typography, animations, utilities
├── layout.css       ← App shell structure
└── components.css   ← Buttons, cards, forms, modals, toasts
```

Flow: `variables.css` → `base.css` → `layout.css` → `components.css`

---

## Design Tokens (variables.css)

### 3-Tier Token System
```css
:root {
  /* === TIER 1: Primitive Tokens (raw values) === */
  --raw-blue-500: #667eea;
  --raw-blue-600: #5a6fd6;
  --raw-gray-800: #1a1a2e;
  --raw-gray-900: #0f0f23;
  --raw-white: #ffffff;
  --raw-green: #48bb78;
  --raw-red: #fc5c65;
  --raw-amber: #f6ad55;
  
  /* === TIER 2: Semantic Tokens (purpose-based) === */
  --color-bg: var(--raw-gray-900);
  --color-surface: var(--raw-gray-800);
  --color-primary: var(--raw-blue-500);
  --color-primary-hover: var(--raw-blue-600);
  --color-text: #e2e8f0;
  --color-text-muted: #94a3b8;
  --color-border: rgba(255, 255, 255, 0.1);
  --color-success: var(--raw-green);
  --color-error: var(--raw-red);
  --color-warning: var(--raw-amber);
  
  /* Spacing Scale (8px base) */
  --space-xs: 0.25rem;   /* 4px */
  --space-sm: 0.5rem;    /* 8px */
  --space-md: 1rem;      /* 16px */
  --space-lg: 1.5rem;    /* 24px */
  --space-xl: 2rem;      /* 32px */
  --space-2xl: 3rem;     /* 48px */
  
  /* Typography */
  --font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
  --font-mono: 'JetBrains Mono', monospace;
  --font-size-xs: 0.75rem;
  --font-size-sm: 0.875rem;
  --font-size-md: 1rem;
  --font-size-lg: 1.125rem;
  --font-size-xl: 1.5rem;
  --font-size-2xl: 2rem;
  --font-weight-normal: 400;
  --font-weight-medium: 500;
  --font-weight-bold: 700;
  --line-height: 1.6;
  
  /* Borders & Shadows */
  --radius-sm: 6px;
  --radius-md: 8px;
  --radius-lg: 12px;
  --radius-full: 9999px;
  --shadow-sm: 0 1px 2px rgba(0, 0, 0, 0.2);
  --shadow-md: 0 4px 6px rgba(0, 0, 0, 0.3);
  --shadow-lg: 0 10px 25px rgba(0, 0, 0, 0.4);
  
  /* Layout */
  --sidebar-width: 260px;
  --header-height: 60px;
  --max-content: 1200px;
  
  /* Transitions */
  --transition-fast: 0.15s ease;
  --transition: 0.2s ease;
  --transition-slow: 0.3s ease;
  
  /* === TIER 3: Component Tokens (optional) === */
  --btn-padding: var(--space-sm) var(--space-lg);
  --card-padding: var(--space-lg);
  --input-height: 44px;
}
```

### Light Mode Override
```css
[data-theme="light"] {
  --color-bg: #f8fafc;
  --color-surface: #ffffff;
  --color-text: #1e293b;
  --color-text-muted: #64748b;
  --color-border: rgba(0, 0, 0, 0.1);
}
```

---

## Base Styles (base.css)

```css
/* Reset */
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

body {
  font-family: var(--font-family);
  font-size: var(--font-size-md);
  line-height: var(--line-height);
  color: var(--color-text);
  background: var(--color-bg);
  -webkit-font-smoothing: antialiased;
}

/* Utilities */
.hidden { display: none !important; }
.text-muted { color: var(--color-text-muted); }
.text-center { text-align: center; }
.mt-md { margin-top: var(--space-md); }
.mb-md { margin-bottom: var(--space-md); }

/* Animations */
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(8px); }
  to { opacity: 1; transform: translateY(0); }
}

.animate-in { animation: fadeIn var(--transition-slow) ease; }
```

---

## Responsive Breakpoints

```css
/* Mobile-first approach */
/* Default: mobile (< 768px) */

@media (min-width: 768px) {
  /* Tablet */
}

@media (min-width: 1024px) {
  /* Desktop */
}

@media (min-width: 1440px) {
  /* Large desktop */
}
```

---

## Rules

- ✅ Every color → CSS Variable
- ✅ Every spacing → use scale (xs/sm/md/lg/xl)
- ✅ Every font-size → use scale
- ✅ Every border-radius → use token
- ✅ Every transition → use token
- ❌ Never use `px` for text (use `rem`)
- ❌ Never hardcode colors (use variables)
- ❌ Never use `!important` (except utilities)
