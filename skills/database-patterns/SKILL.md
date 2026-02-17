---
name: database-patterns
description: Database schema design, indexing, queries for Firestore, PostgreSQL, MongoDB. Data modeling, relationships, migrations. Use when designing data architecture.
allowed-tools: Read, Write, Edit, Glob, Grep
version: 1.0.0
source: Synthesized from mcpmarket.com Database Schema Designer, cursor.directory patterns
---

# Database Patterns

## Decision Matrix

| Criteria | Firestore | PostgreSQL | MongoDB |
|----------|-----------|------------|---------|
| Best for | Mobile/web apps, real-time | Complex queries, transactions | Flexible schemas, high write |
| Scaling | Auto (serverless) | Vertical + read replicas | Horizontal (sharding) |
| Schema | Flexible (NoSQL) | Strict (SQL) | Flexible (NoSQL) |
| Real-time | Built-in listeners | Requires polling/WebSocket | Change streams |
| Cost model | Per read/write | Per instance | Per instance or Atlas |
| Free tier | Firebase free tier | Supabase/Neon free | Atlas free (512MB) |

---

## Firestore Patterns

### Schema Rules
1. **Flat collections** — avoid deep nesting (max 2 subcollection levels)
2. **Denormalize** — duplicate data to avoid extra reads
3. **Every document** has: `createdAt`, `updatedAt`, `ownerId`
4. **Document ID** — auto-generated or meaningful (e.g., userId)

### Common Schema
```javascript
// Collection: users/{userId}
{
  displayName: "John",
  email: "john@example.com",
  photoURL: "https://...",
  createdAt: Timestamp,
  plan: "free"  // denormalized from subscriptions
}

// Collection: contents/{contentId}
{
  title: "My Post",
  body: "...",
  status: "draft",  // draft | published | archived
  tags: ["health", "nutrition"],
  ownerId: "userId123",  // for security rules
  createdBy: "userId123",
  createdAt: Timestamp,
  updatedAt: Timestamp
}

// Collection: settings/{userId}
{
  geminiApiKey: "encrypted_or_direct",
  facebookPageId: "page123",
  wordpressSiteUrl: "https://blog.example.com",
  theme: "dark"
}
```

### Query Patterns
```javascript
// Compound query (needs composite index)
query(collection(db, 'contents'),
  where('ownerId', '==', userId),
  where('status', '==', 'published'),
  orderBy('createdAt', 'desc'),
  limit(20)
);

// Pagination with cursors
query(collection(db, 'contents'),
  where('ownerId', '==', userId),
  orderBy('createdAt', 'desc'),
  startAfter(lastDoc),
  limit(20)
);
```

---

## PostgreSQL Patterns

### Schema Rules
1. **Normalize to 3NF** — then denormalize for performance
2. **UUID for IDs** — `gen_random_uuid()`
3. **Timestamps** — always `created_at` and `updated_at`
4. **Soft delete** — `deleted_at` instead of actual deletion
5. **Index** — on foreign keys, frequently queried columns

### Common Schema
```sql
-- Users
CREATE TABLE users (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  email VARCHAR(255) UNIQUE NOT NULL,
  display_name VARCHAR(255),
  avatar_url TEXT,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- Contents
CREATE TABLE contents (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  title VARCHAR(500) NOT NULL,
  body TEXT,
  status VARCHAR(20) DEFAULT 'draft' CHECK (status IN ('draft', 'published', 'archived')),
  tags TEXT[] DEFAULT '{}',
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW(),
  deleted_at TIMESTAMPTZ
);

-- Indexes
CREATE INDEX idx_contents_user ON contents(user_id);
CREATE INDEX idx_contents_status ON contents(status);
CREATE INDEX idx_contents_created ON contents(created_at DESC);

-- Auto-update updated_at
CREATE OR REPLACE FUNCTION update_updated_at()
RETURNS TRIGGER AS $$
BEGIN
  NEW.updated_at = NOW();
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER contents_updated_at
  BEFORE UPDATE ON contents
  FOR EACH ROW EXECUTE FUNCTION update_updated_at();
```

### Row Level Security (Supabase)
```sql
ALTER TABLE contents ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Users see own contents" ON contents
  FOR SELECT USING (auth.uid() = user_id);

CREATE POLICY "Users create own contents" ON contents
  FOR INSERT WITH CHECK (auth.uid() = user_id);

CREATE POLICY "Users update own contents" ON contents
  FOR UPDATE USING (auth.uid() = user_id);
```

---

## MongoDB Patterns

### Schema Rules
1. **Embed** when data is always accessed together (1:1, 1:few)
2. **Reference** when data is accessed independently (1:many, many:many)
3. **Document size** — max 16MB per document
4. **Index** — compound indexes for frequent queries

### Embed vs Reference
| Pattern | Embed | Reference |
|---------|-------|-----------|
| 1:1 | ✅ Always embed | Unnecessary |
| 1:Few (< 100) | ✅ Embed | Optional |
| 1:Many (100+) | ❌ Reference | ✅ Reference |
| Many:Many | ❌ Never | ✅ Reference both sides |

---

## Anti-Patterns

- ❌ No indexes on queried columns
- ❌ Storing derived data that can be computed
- ❌ Too many subcollections in Firestore (read costs)
- ❌ No pagination on list queries
- ❌ Storing sensitive data without encryption
- ❌ No `ON DELETE CASCADE` on foreign keys
