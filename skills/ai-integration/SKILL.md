---
name: ai-integration
description: AI API integration patterns — Gemini, OpenAI, Claude. Prompt engineering, streaming responses, token management, cost optimization. Use when building AI-powered features.
allowed-tools: Read, Write, Edit, Glob, Grep
version: 1.0.0
source: Synthesized from Gemini API docs, OpenAI cookbook, prompt engineering best practices
---

# AI Integration Patterns

## API Comparison

| Feature | Gemini | OpenAI | Claude |
|---------|--------|--------|--------|
| Free tier | ✅ 60 req/min | ❌ Pay-per-use | ❌ Pay-per-use |
| Streaming | ✅ | ✅ | ✅ |
| Vision | ✅ (multimodal) | ✅ GPT-4V | ✅ |
| Best for | Cost-effective | Widest ecosystem | Long context |
| Auth | API Key | API Key | API Key |

---

## Gemini API (Google)

### Setup
```javascript
// services/gemini.js
const GEMINI_API = 'https://generativelanguage.googleapis.com/v1beta/models';

export async function generateContent(prompt, apiKey, options = {}) {
  const { model = 'gemini-2.0-flash', temperature = 0.7, maxTokens = 2048 } = options;
  
  const response = await fetch(
    `${GEMINI_API}/${model}:generateContent?key=${apiKey}`,
    {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        contents: [{ parts: [{ text: prompt }] }],
        generationConfig: {
          temperature,
          maxOutputTokens: maxTokens,
        },
      }),
    }
  );

  if (!response.ok) {
    const error = await response.json();
    throw new Error(error.error?.message || 'Gemini API error');
  }

  const data = await response.json();
  return data.candidates?.[0]?.content?.parts?.[0]?.text || '';
}
```

### Streaming Response
```javascript
export async function streamContent(prompt, apiKey, onChunk, options = {}) {
  const { model = 'gemini-2.0-flash' } = options;
  
  const response = await fetch(
    `${GEMINI_API}/${model}:streamGenerateContent?alt=sse&key=${apiKey}`,
    {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        contents: [{ parts: [{ text: prompt }] }],
      }),
    }
  );

  const reader = response.body.getReader();
  const decoder = new TextDecoder();
  let buffer = '';
  let fullText = '';

  while (true) {
    const { done, value } = await reader.read();
    if (done) break;

    buffer += decoder.decode(value, { stream: true });
    const lines = buffer.split('\n');
    buffer = lines.pop();

    for (const line of lines) {
      if (line.startsWith('data: ')) {
        try {
          const data = JSON.parse(line.slice(6));
          const text = data.candidates?.[0]?.content?.parts?.[0]?.text || '';
          fullText += text;
          onChunk(text, fullText);
        } catch (e) {
          // Skip malformed JSON
        }
      }
    }
  }
  return fullText;
}
```

---

## OpenAI API

### Setup
```javascript
// services/openai.js
const OPENAI_API = 'https://api.openai.com/v1/chat/completions';

export async function chat(messages, apiKey, options = {}) {
  const { model = 'gpt-4o-mini', temperature = 0.7 } = options;
  
  const response = await fetch(OPENAI_API, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Authorization': `Bearer ${apiKey}`,
    },
    body: JSON.stringify({
      model,
      messages,
      temperature,
    }),
  });

  const data = await response.json();
  return data.choices[0].message.content;
}
```

---

## Prompt Engineering Patterns

### System Prompt Template
```javascript
function buildSystemPrompt(task, context) {
  return `You are a ${task.role}.

## Task
${task.description}

## Context
${context}

## Rules
- ${task.rules.join('\n- ')}

## Output Format
${task.outputFormat}`;
}
```

### Structured Output (JSON)
```javascript
const prompt = `
Analyze the following text and return JSON:
{
  "sentiment": "positive|negative|neutral",
  "topics": ["topic1", "topic2"],
  "summary": "one sentence summary"
}

Text: ${inputText}

Return ONLY valid JSON, no markdown.`;
```

### Chain of Thought
```javascript
const prompt = `
Think step by step:
1. First, identify the main topic
2. Then, list the key points
3. Finally, write a summary

Input: ${text}
`;
```

---

## API Key Management

### Client-Side (Per-User)
```javascript
// Store in Firestore per user (not in .env)
async function saveApiKey(userId, provider, apiKey) {
  await updateDoc(doc(db, 'settings', userId), {
    [`${provider}ApiKey`]: apiKey,
    updatedAt: serverTimestamp(),
  });
}

// Retrieve when needed
async function getApiKey(userId, provider) {
  const settings = await getDoc(doc(db, 'settings', userId));
  return settings.data()?.[`${provider}ApiKey`];
}
```

### Server-Side (Shared)
```javascript
// Store in environment variables
const GEMINI_API_KEY = process.env.GEMINI_API_KEY;
```

---

## Cost Optimization

1. **Use smallest model** — gemini-2.0-flash or gpt-4o-mini for most tasks
2. **Limit output tokens** — set `maxOutputTokens` appropriately
3. **Cache responses** — same prompt = same response
4. **Batch requests** — combine multiple prompts when possible
5. **Rate limiting** — don't exceed free tier limits

### Token Estimation
```javascript
// Rough estimate: 1 token ≈ 4 characters (English)
function estimateTokens(text) {
  return Math.ceil(text.length / 4);
}
```

---

## Error Handling

| Error | Cause | Solution |
|-------|-------|----------|
| 400 | Invalid prompt | Check prompt format |
| 401 | Bad API key | Verify key, check expiry |
| 429 | Rate limited | Wait and retry with backoff |
| 500 | Server error | Retry once |
| Safety blocked | Content filtered | Modify prompt, lower risk |

```javascript
async function safeGenerate(prompt, apiKey) {
  try {
    return await generateContent(prompt, apiKey);
  } catch (error) {
    if (error.message.includes('SAFETY')) {
      return '⚠️ Content was filtered. Try rephrasing.';
    }
    if (error.message.includes('429')) {
      await sleep(2000);
      return await generateContent(prompt, apiKey);
    }
    throw error;
  }
}
```
