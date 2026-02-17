# Endpoint Documentation Template

> Copy this template for each API endpoint documentation.

---

## `[METHOD] /api/v1/[resource]`

[One-line description of what this endpoint does.]

**Authentication**: Required / Public  
**Rate Limit**: [X] requests/minute  
**Permission**: [any / user / admin]

### Request

#### Headers

| Header | Required | Description |
|--------|----------|-------------|
| `Authorization` | ✅ | `Bearer <token>` |
| `Content-Type` | ✅ | `application/json` |

#### Path Parameters

| Param | Type | Description |
|-------|------|-------------|
| `id` | string (UUID) | Resource identifier |

#### Query Parameters

| Param | Type | Default | Description |
|-------|------|---------|-------------|
| `page` | integer | 1 | Page number |
| `limit` | integer | 20 | Items per page (max: 100) |

#### Request Body

| Field | Type | Required | Validation |
|-------|------|----------|-----------|
| `name` | string | ✅ | 2-100 characters |
| `email` | string | ✅ | Valid email format |

### Response

#### Success (200/201)

```json
{
  "success": true,
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "name": "Alice",
    "email": "alice@example.com",
    "createdAt": "2024-01-15T10:30:00Z"
  }
}
```

#### Errors

| Status | Code | When |
|--------|------|------|
| 400 | `VALIDATION_ERROR` | Invalid input data |
| 401 | `UNAUTHORIZED` | Missing/expired token |
| 403 | `FORBIDDEN` | No permission |
| 404 | `NOT_FOUND` | Resource doesn't exist |
| 409 | `CONFLICT` | Duplicate (e.g., email) |
| 429 | `RATE_LIMITED` | Too many requests |

### Example

#### cURL

```bash
curl -X POST https://api.example.com/v1/users \
  -H "Authorization: Bearer eyJhbGciOi..." \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Alice",
    "email": "alice@example.com"
  }'
```

#### JavaScript (fetch)

```javascript
const response = await fetch('https://api.example.com/v1/users', {
  method: 'POST',
  headers: {
    'Authorization': `Bearer ${token}`,
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    name: 'Alice',
    email: 'alice@example.com',
  }),
});

const data = await response.json();
```

### Notes

- [Any special behavior or caveats]
- [Related endpoints]
