---
name: cloudflare-workers
description: Cloudflare Workers, Pages, KV, D1, R2 patterns. Edge computing, Wrangler CLI, serverless deployment. Use when deploying to Cloudflare or building edge functions.
allowed-tools: Read, Write, Edit, Glob, Grep, Bash
version: 1.0.0
source: Synthesized from playbooks.com cloudflare-workers-dev-experience, Cloudflare docs
---

# Cloudflare Workers & Pages Patterns

## Core Concepts

- **Workers**: Serverless functions running at the edge (250+ locations)
- **Pages**: Static site hosting with Git integration (unlimited free requests)
- **KV**: Key-value storage (eventually consistent)
- **D1**: SQLite at the edge
- **R2**: S3-compatible object storage (no egress fees)

---

## Cloudflare Pages (Static Sites)

### Best For
- Vite + Vanilla JS apps
- React/Next.js static exports
- Any SPA or static site

### Deploy via Git
1. Push to GitHub
2. Cloudflare Dashboard → Workers & Pages → Create → Pages
3. Connect to repo
4. Configure:
   - Build command: `npm run build`
   - Output directory: `dist`
5. Add environment variables (from `.env`)

### Configuration
```toml
# wrangler.toml (optional for Pages)
name = "my-app"
compatibility_date = "2024-01-01"

[site]
bucket = "./dist"
```

### Environment Variables
- Set in Cloudflare Dashboard → Settings → Environment Variables
- Available at build time via `import.meta.env.VITE_*` (Vite)
- Never commit `.env` to git

---

## Cloudflare Workers (Edge Functions)

### Basic Worker
```javascript
// src/index.js
export default {
  async fetch(request, env) {
    const url = new URL(request.url);
    
    if (url.pathname === '/api/hello') {
      return new Response(JSON.stringify({ message: 'Hello from edge!' }), {
        headers: { 'Content-Type': 'application/json' },
      });
    }
    
    return new Response('Not Found', { status: 404 });
  },
};
```

### Worker with KV
```javascript
export default {
  async fetch(request, env) {
    // Read from KV
    const value = await env.MY_KV.get('key');
    
    // Write to KV
    await env.MY_KV.put('key', 'value', { expirationTtl: 3600 });
    
    return new Response(value);
  },
};
```

### Worker with D1 (SQLite)
```javascript
export default {
  async fetch(request, env) {
    const results = await env.DB.prepare(
      'SELECT * FROM users WHERE id = ?'
    ).bind(1).first();
    
    return Response.json(results);
  },
};
```

### wrangler.toml
```toml
name = "my-worker"
main = "src/index.js"
compatibility_date = "2024-01-01"

[[kv_namespaces]]
binding = "MY_KV"
id = "abc123"

[[d1_databases]]
binding = "DB"
database_name = "my-db"
database_id = "def456"
```

---

## Development Workflow

### Setup
```bash
npm create cloudflare@latest my-project
cd my-project
```

### Local Dev
```bash
npx wrangler dev          # Start local dev server
npx wrangler dev --local  # Fully local (Miniflare)
```

### Deploy
```bash
npx wrangler deploy       # Deploy to production
npx wrangler pages deploy dist  # Deploy Pages
```

---

## Best Practices

1. **Keep `compatibility_date` current** — access latest features
2. **Use `wrangler dev --local`** — test with Miniflare before deploying
3. **Environment variables** — use Cloudflare Dashboard, not `.env` in production
4. **Cache static content** — use Cache API for frequently accessed data
5. **Bundle size** — keep Workers small (< 1MB after compression)
6. **Error handling** — always return proper HTTP responses
7. **CORS** — configure if Workers serve a different domain

### CORS Headers
```javascript
const corsHeaders = {
  'Access-Control-Allow-Origin': '*',
  'Access-Control-Allow-Methods': 'GET, POST, OPTIONS',
  'Access-Control-Allow-Headers': 'Content-Type, Authorization',
};

// Handle preflight
if (request.method === 'OPTIONS') {
  return new Response(null, { headers: corsHeaders });
}
```

---

## Free Tier Limits

| Service | Free Tier |
|---------|-----------|
| Pages | Unlimited requests, 500 builds/month |
| Workers | 100K requests/day |
| KV | 100K reads/day, 1K writes/day |
| D1 | 5M rows read/day, 100K writes/day |
| R2 | 10GB storage, 10M reads/month |
