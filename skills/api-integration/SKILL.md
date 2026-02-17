---
name: api-integration
description: REST API, GraphQL, WebSocket integration patterns. Error handling, rate limiting, retry logic, streaming responses. Use when connecting to external APIs like Gemini, OpenAI, Facebook Graph, WordPress REST.
allowed-tools: Read, Write, Edit, Glob, Grep
version: 1.0.0
source: Synthesized from community best practices, cursor.directory patterns
---

# API Integration Patterns

## Core Principles

1. **Service module per API** — 1 file = 1 external service
2. **Never hardcode API keys** — use .env or per-user storage
3. **Always handle errors** — network, auth, rate limit, server errors
4. **Retry with backoff** — for transient failures
5. **Show loading states** — never leave the user wondering

---

## REST API Pattern

### Service Module Template
```javascript
// services/api-name.js
const BASE_URL = 'https://api.example.com/v1';

async function request(endpoint, options = {}) {
  const { method = 'GET', body, headers = {}, apiKey } = options;
  
  const response = await fetch(`${BASE_URL}${endpoint}`, {
    method,
    headers: {
      'Content-Type': 'application/json',
      ...(apiKey && { 'Authorization': `Bearer ${apiKey}` }),
      ...headers,
    },
    ...(body && { body: JSON.stringify(body) }),
  });
  
  if (!response.ok) {
    const error = await response.json().catch(() => ({}));
    throw new ApiError(response.status, error.message || response.statusText);
  }
  
  return response.json();
}

class ApiError extends Error {
  constructor(status, message) {
    super(message);
    this.status = status;
    this.name = 'ApiError';
  }
}
```

### Error Handling Matrix

| Status | Meaning | Action |
|--------|---------|--------|
| 400 | Bad Request | Show validation error to user |
| 401 | Unauthorized | Re-authenticate |
| 403 | Forbidden | Show permission error |
| 404 | Not Found | Show "not found" message |
| 429 | Rate Limited | Retry with exponential backoff |
| 500 | Server Error | Retry once, then show error |

### Retry with Exponential Backoff
```javascript
async function fetchWithRetry(fn, maxRetries = 3) {
  for (let i = 0; i < maxRetries; i++) {
    try {
      return await fn();
    } catch (error) {
      if (i === maxRetries - 1) throw error;
      if (error.status === 429 || error.status >= 500) {
        await sleep(Math.pow(2, i) * 1000); // 1s, 2s, 4s
        continue;
      }
      throw error; // Don't retry client errors
    }
  }
}

function sleep(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}
```

---

## Streaming API Pattern (SSE / Gemini / OpenAI)

```javascript
async function streamResponse(prompt, apiKey, onChunk) {
  const response = await fetch(API_URL, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Authorization': `Bearer ${apiKey}`,
    },
    body: JSON.stringify({ prompt, stream: true }),
  });

  const reader = response.body.getReader();
  const decoder = new TextDecoder();
  let buffer = '';

  while (true) {
    const { done, value } = await reader.read();
    if (done) break;
    
    buffer += decoder.decode(value, { stream: true });
    const lines = buffer.split('\n');
    buffer = lines.pop(); // Keep incomplete line in buffer
    
    for (const line of lines) {
      if (line.startsWith('data: ')) {
        const data = JSON.parse(line.slice(6));
        onChunk(data);
      }
    }
  }
}
```

---

## Facebook Graph API Pattern

```javascript
// services/facebook.js
const FB_API = 'https://graph.facebook.com/v18.0';

export async function publishPost(pageId, accessToken, { message, link, imageUrl }) {
  const endpoint = imageUrl ? `${FB_API}/${pageId}/photos` : `${FB_API}/${pageId}/feed`;
  const body = imageUrl
    ? { url: imageUrl, caption: message, access_token: accessToken }
    : { message, link, access_token: accessToken };
  
  const response = await fetch(endpoint, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(body),
  });
  
  return response.json();
}
```

---

## WordPress REST API Pattern

```javascript
// services/wordpress.js
export async function publishPost(siteUrl, username, appPassword, { title, content, status = 'draft' }) {
  const credentials = btoa(`${username}:${appPassword}`);
  
  const response = await fetch(`${siteUrl}/wp-json/wp/v2/posts`, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Authorization': `Basic ${credentials}`,
    },
    body: JSON.stringify({ title, content, status }),
  });
  
  return response.json();
}
```

---

## Anti-Patterns

- ❌ Storing API keys in source code
- ❌ No error handling on fetch calls
- ❌ No loading indicators during API calls
- ❌ Catching errors silently without user feedback
- ❌ No timeout on fetch requests
- ❌ Polling when WebSocket/SSE is appropriate
