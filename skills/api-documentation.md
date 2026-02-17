---
name: api-documentation
description: "OpenAPI/Swagger, AsyncAPI, Documentation-as-Code, API Versioning"
---

# 📡 API Documentation

> Standards and templates for creating clear, comprehensive API documentation.

## OpenAPI 3.0 Template

```yaml
openapi: 3.0.3
info:
  title: My API
  description: Brief description of what this API does
  version: 1.0.0
  contact:
    email: dev@example.com

servers:
  - url: https://api.example.com/v1
    description: Production
  - url: http://localhost:3000/v1
    description: Local development

paths:
  /users:
    get:
      summary: List users
      description: Returns a paginated list of users
      operationId: listUsers
      tags: [Users]
      parameters:
        - name: page
          in: query
          schema:
            type: integer
            default: 1
        - name: limit
          in: query
          schema:
            type: integer
            default: 20
            maximum: 100
      responses:
        '200':
          description: Successful response
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/UserListResponse'
        '401':
          $ref: '#/components/responses/Unauthorized'

    post:
      summary: Create user
      operationId: createUser
      tags: [Users]
      security:
        - bearerAuth: []
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateUserRequest'
      responses:
        '201':
          description: User created
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/UserResponse'
        '400':
          $ref: '#/components/responses/ValidationError'
        '409':
          description: Email already exists

components:
  schemas:
    User:
      type: object
      properties:
        id:
          type: string
          format: uuid
        name:
          type: string
          minLength: 2
          maxLength: 100
        email:
          type: string
          format: email
        createdAt:
          type: string
          format: date-time
      required: [id, name, email, createdAt]

    CreateUserRequest:
      type: object
      properties:
        name:
          type: string
          minLength: 2
          maxLength: 100
        email:
          type: string
          format: email
      required: [name, email]

    UserResponse:
      type: object
      properties:
        success:
          type: boolean
        data:
          $ref: '#/components/schemas/User'

    UserListResponse:
      type: object
      properties:
        success:
          type: boolean
        data:
          type: array
          items:
            $ref: '#/components/schemas/User'
        meta:
          type: object
          properties:
            page:
              type: integer
            total:
              type: integer

    ErrorResponse:
      type: object
      properties:
        success:
          type: boolean
          example: false
        error:
          type: object
          properties:
            code:
              type: string
            message:
              type: string

  responses:
    Unauthorized:
      description: Missing or invalid authentication
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/ErrorResponse'
    ValidationError:
      description: Invalid request body
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/ErrorResponse'

  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT
```

---

## Endpoint Documentation Format

### Markdown Template

```markdown
### `POST /api/v1/users`

Create a new user account.

**Authentication**: Required (Bearer token)

**Rate Limit**: 10 requests/minute

#### Request Body

| Field | Type | Required | Validation |
|-------|------|----------|-----------|
| `name` | string | ✅ | 2-100 characters |
| `email` | string | ✅ | Valid email format |
| `role` | string | ❌ | Default: `"user"`. Enum: `user`, `admin` |

#### Example Request

\```bash
curl -X POST https://api.example.com/v1/users \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d '{"name": "Alice", "email": "alice@example.com"}'
\```

#### Success Response (201)

\```json
{
  "success": true,
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "name": "Alice",
    "email": "alice@example.com",
    "role": "user",
    "createdAt": "2024-01-15T10:30:00Z"
  }
}
\```

#### Error Responses

| Status | Code | Description |
|--------|------|-------------|
| 400 | `VALIDATION_ERROR` | Invalid name or email format |
| 401 | `UNAUTHORIZED` | Missing or expired token |
| 409 | `CONFLICT` | Email already registered |
```

---

## API Versioning Strategies

| Strategy | Example | Pros | Cons |
|----------|---------|------|------|
| **URL path** | `/api/v1/users` | Simple, visible | URL changes |
| **Query param** | `/api/users?version=1` | Flexible | Easy to forget |
| **Header** | `Accept: application/vnd.api.v1+json` | Clean URLs | Hidden |
| **Date-based** | `API-Version: 2024-01-15` | Clear timeline | Complex |

### Recommended: URL path versioning

```
/api/v1/users   ← Current stable
/api/v2/users   ← New version (breaking changes)

Deprecation policy:
- v1 supported for 12 months after v2 release
- Deprecation header: Sunset: Sat, 01 Jan 2025 00:00:00 GMT
```

---

## Documentation Tools

```bash
# Generate docs from OpenAPI spec
npx @redocly/cli build-docs openapi.yaml --output docs/api.html

# Validate OpenAPI spec
npx @redocly/cli lint openapi.yaml

# Generate TypeScript types from spec
npx openapi-typescript openapi.yaml -o src/types/api.ts

# Mock server from spec
npx prism mock openapi.yaml
```

---

## Documentation Checklist

- [ ] All endpoints documented with request/response examples
- [ ] Authentication described (how to get tokens)
- [ ] Error codes and messages documented
- [ ] Rate limits documented
- [ ] Versioning strategy documented
- [ ] OpenAPI spec validates without errors
- [ ] Examples are copy-paste ready (curl/fetch)
- [ ] Changelog tracks API changes
