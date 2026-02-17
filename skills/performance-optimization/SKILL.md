---
name: performance-optimization
description: Web performance optimization — bundle size, lazy loading, caching, Core Web Vitals, image optimization. Use when improving load times or Lighthouse scores.
allowed-tools: Read, Write, Edit, Glob, Grep, Bash
version: 1.0.0
source: Synthesized from web.dev, Lighthouse docs, cursor.directory patterns
---

# Web Performance Optimization

## Core Web Vitals (2024 Targets)

| Metric | Target | Measures |
|--------|--------|----------|
| **LCP** (Largest Contentful Paint) | < 2.5s | Loading speed |
| **INP** (Interaction to Next Paint) | < 200ms | Responsiveness |
| **CLS** (Cumulative Layout Shift) | < 0.1 | Visual stability |

---

## Bundle Size Optimization

### Analyze Bundle
```bash
# Vite
npx vite-bundle-visualizer

# Webpack
npx webpack-bundle-analyzer stats.json
```

### Reduction Strategies

1. **Tree shaking** — import only what you need
```javascript
// ❌ Bad: imports entire library
import _ from 'lodash';
_.debounce(fn, 300);

// ✅ Good: import specific function
import debounce from 'lodash/debounce';
debounce(fn, 300);
```

2. **Dynamic imports** — lazy load routes/features
```javascript
// Split by route
const Dashboard = () => import('./pages/dashboard.js');
const Settings = () => import('./pages/settings.js');
```

3. **External CDN** — for large libraries
```html
<!-- Load from CDN, not bundle -->
<script src="https://cdn.jsdelivr.net/npm/chart.js@4"></script>
```

4. **Remove unused dependencies**
```bash
npx depcheck   # Find unused deps
```

---

## Image Optimization

### Format Selection
| Format | Best For | Savings |
|--------|----------|---------|
| WebP | Photos, complex graphics | 25-35% vs JPEG |
| AVIF | Modern browsers | 50% vs JPEG |
| SVG | Icons, logos | Infinitely scalable |
| PNG | Transparency required | Lossless |

### Implementation
```html
<!-- Responsive images with modern formats -->
<picture>
  <source srcset="image.avif" type="image/avif">
  <source srcset="image.webp" type="image/webp">
  <img src="image.jpg" alt="Description"
       width="800" height="600"
       loading="lazy"
       decoding="async">
</picture>
```

### Rules
- Always set `width` and `height` (prevents CLS)
- Use `loading="lazy"` for below-fold images
- Use `decoding="async"` to not block rendering
- Compress images before deployment

---

## Caching Strategies

### HTTP Cache Headers
```
# Static assets (CSS, JS, images) — immutable hash in filename
Cache-Control: public, max-age=31536000, immutable

# HTML files — always revalidate
Cache-Control: no-cache

# API responses — short cache
Cache-Control: public, max-age=60, s-maxage=300
```

### Service Worker (for PWA)
```javascript
// Cache-first for static assets, network-first for API
self.addEventListener('fetch', (event) => {
  if (event.request.url.includes('/api/')) {
    event.respondWith(networkFirst(event.request));
  } else {
    event.respondWith(cacheFirst(event.request));
  }
});
```

---

## Loading Performance

### Critical Rendering Path
```html
<!-- 1. Preload critical resources -->
<link rel="preload" href="/fonts/inter.woff2" as="font" crossorigin>
<link rel="preload" href="/css/variables.css" as="style">

<!-- 2. Inline critical CSS -->
<style>
  /* Above-fold styles only */
  body { background: #0f0f23; color: #e2e8f0; }
</style>

<!-- 3. Defer non-critical CSS -->
<link rel="stylesheet" href="/css/components.css" media="print" onload="this.media='all'">

<!-- 4. Defer JavaScript -->
<script type="module" src="/js/app.js"></script>
```

### Font Loading
```css
/* Use font-display: swap to prevent FOIT */
@font-face {
  font-family: 'Inter';
  src: url('/fonts/inter.woff2') format('woff2');
  font-display: swap;
}
```

---

## Runtime Performance

### DOM Operations
```javascript
// ❌ Bad: multiple DOM updates cause reflows
items.forEach(item => {
  container.innerHTML += `<div>${item}</div>`;
});

// ✅ Good: batch DOM updates
const html = items.map(item => `<div>${item}</div>`).join('');
container.innerHTML = html;

// ✅ Better: DocumentFragment for complex elements
const fragment = document.createDocumentFragment();
items.forEach(item => {
  const el = document.createElement('div');
  el.textContent = item;
  fragment.appendChild(el);
});
container.appendChild(fragment);
```

### Debounce & Throttle
```javascript
// Debounce for search/input (wait until user stops)
function debounce(fn, delay = 300) {
  let timer;
  return (...args) => {
    clearTimeout(timer);
    timer = setTimeout(() => fn(...args), delay);
  };
}

// Throttle for scroll/resize (fire at most once per interval)
function throttle(fn, interval = 100) {
  let lastTime = 0;
  return (...args) => {
    const now = Date.now();
    if (now - lastTime >= interval) {
      lastTime = now;
      fn(...args);
    }
  };
}
```

---

## Performance Budget

| Resource | Budget |
|----------|--------|
| Total JS (compressed) | < 150KB |
| Total CSS (compressed) | < 50KB |
| Total page weight | < 500KB |
| First paint | < 1.5s |
| Time to Interactive | < 3.5s |
| Lighthouse Performance | ≥ 80 |
