---
name: vanilla-js-vite
description: Vanilla JavaScript + Vite development patterns. Modular SPA architecture, hash routing, DOM manipulation, CSS design system, no framework needed. Use when building lightweight apps without React/Vue/Angular.
allowed-tools: Read, Write, Edit, Glob, Grep, Bash
version: 1.0.0
source: Synthesized from cursor.directory, Vite docs, awesome-cursorrules patterns
---

# Vanilla JS + Vite Patterns

## When to Use
- Lightweight apps (tools, dashboards, internal apps)
- When React/Vue overhead is unnecessary
- When bundle size matters (< 100KB)
- Learning projects or prototypes that need to be fast

## Project Structure
```
project/
├── index.html           ← Entry point (Vite serves this)
├── vite.config.js       ← Build config
├── package.json
├── .env.example
├── .gitignore
├── css/
│   ├── variables.css    ← Design tokens
│   ├── base.css         ← Reset, typography, utilities
│   ├── layout.css       ← App shell (sidebar, header, main)
│   └── components.css   ← Buttons, cards, forms, modals
└── js/
    ├── app.js           ← Entry point: init, wiring
    ├── config.js        ← Constants, routes, endpoints
    ├── router.js        ← Hash-based SPA routing
    ├── auth.js          ← Authentication
    ├── state.js         ← Data layer / CRUD
    ├── services/        ← 1 file per external API
    ├── pages/           ← 1 file per route
    ├── components/      ← Reusable UI elements
    └── utils/           ← Helpers (dom, format, storage)
```

## Key Principles

1. **1 file = 1 responsibility** — max 200-400 lines
2. **ES modules** — `import`/`export` everywhere
3. **No build-time frameworks** — vanilla DOM manipulation
4. **CSS Variables** — all design tokens in `:root`
5. **Hash routing** — `#/page` for SPA navigation

---

## Patterns

### Hash Router
```javascript
// router.js
const routes = {};

export function registerRoute(path, handler) {
  routes[path] = handler;
}

export function navigate(path) {
  window.location.hash = `#${path}`;
}

export function initRouter() {
  const handleRoute = () => {
    const path = window.location.hash.slice(1) || '/';
    const handler = routes[path];
    if (handler) handler();
  };
  
  window.addEventListener('hashchange', handleRoute);
  handleRoute(); // initial route
}
```

### Page Module Pattern
```javascript
// pages/dashboard.js
export function renderDashboard() {
  const main = document.getElementById('main-content');
  main.innerHTML = `
    <div class="page-header">
      <h1>Dashboard</h1>
    </div>
    <div class="dashboard-grid">
      ${renderStats()}
      ${renderRecentItems()}
    </div>
  `;
  
  // Attach event listeners AFTER rendering
  attachDashboardEvents();
}

function renderStats() {
  return `<div class="card">...</div>`;
}

function attachDashboardEvents() {
  document.getElementById('create-btn')?.addEventListener('click', () => {
    navigate('/create');
  });
}
```

### Component Pattern
```javascript
// components/toast.js
export function showToast(message, type = 'info', duration = 3000) {
  const container = document.getElementById('toast-container');
  const toast = document.createElement('div');
  toast.className = `toast toast-${type}`;
  toast.textContent = message;
  container.appendChild(toast);
  
  setTimeout(() => {
    toast.classList.add('toast-exit');
    setTimeout(() => toast.remove(), 300);
  }, duration);
}
```

### State Management (No Store Needed)
```javascript
// For simple apps: module-level state + event dispatching
let currentUser = null;
let items = [];

export function setUser(user) {
  currentUser = user;
  window.dispatchEvent(new CustomEvent('user-changed', { detail: user }));
}

export function getUser() {
  return currentUser;
}
```

---

## Vite Configuration
```javascript
// vite.config.js
import { defineConfig } from 'vite';

export default defineConfig({
  server: {
    port: 3000,
    open: true,
  },
  build: {
    outDir: 'dist',
    minify: 'esbuild',
  },
});
```

## CSS Design System Architecture
```css
/* variables.css — Design Tokens */
:root {
  /* Colors */
  --color-bg: #0f0f23;
  --color-surface: #1a1a2e;
  --color-primary: #667eea;
  --color-text: #e2e8f0;
  
  /* Spacing */
  --space-xs: 0.25rem;
  --space-sm: 0.5rem;
  --space-md: 1rem;
  --space-lg: 1.5rem;
  --space-xl: 2rem;
  
  /* Typography */
  --font-family: 'Inter', sans-serif;
  --font-size-sm: 0.875rem;
  --font-size-md: 1rem;
  --font-size-lg: 1.25rem;
  
  /* Borders & Shadows */
  --radius: 8px;
  --shadow: 0 4px 6px rgba(0, 0, 0, 0.3);
  --transition: all 0.2s ease;
}
```

## Anti-Patterns to Avoid

- ❌ `document.write()` — use `innerHTML` or `createElement`
- ❌ Global variables — use ES module scope
- ❌ Inline styles — use CSS classes
- ❌ `onclick="..."` attributes — use `addEventListener`
- ❌ jQuery — native DOM API is sufficient in 2024+
- ❌ Over-abstraction — keep it simple, this isn't React
