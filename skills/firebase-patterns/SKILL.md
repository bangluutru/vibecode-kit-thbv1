---
name: firebase-patterns
description: Firebase Auth, Firestore, Security Rules, Analytics patterns. Use when building apps with Firebase BaaS, designing Firestore schemas, or implementing authentication flows.
allowed-tools: Read, Write, Edit, Glob, Grep, Bash
version: 1.0.0
source: Synthesized from cursor.directory, Firebase docs, ContentPilot project experience
---

# Firebase Patterns

## Core Principles

1. **Security-first**: Never deploy with test-mode rules
2. **Client-side only**: Firebase SDK runs in browser — no server needed for MVP
3. **Per-user data isolation**: Every document must have an ownerId field
4. **Environment variables**: All config via `VITE_` prefix (Vite) or `NEXT_PUBLIC_` (Next.js)

---

## Firebase SDK Setup

### Module Structure
```
js/
├── firebase.js    ← Init app, export auth/db instances
├── auth.js        ← Login, logout, onAuthStateChanged
├── state.js       ← Firestore CRUD (add, get, update, delete)
└── config.js      ← Collection names, routes, constants
```

### firebase.js Pattern
```javascript
import { initializeApp } from 'firebase/app';
import { getAuth } from 'firebase/auth';
import { getFirestore } from 'firebase/firestore';
import { getAnalytics } from 'firebase/analytics';

const firebaseConfig = {
  apiKey: import.meta.env.VITE_FIREBASE_API_KEY,
  authDomain: import.meta.env.VITE_FIREBASE_AUTH_DOMAIN,
  projectId: import.meta.env.VITE_FIREBASE_PROJECT_ID,
  storageBucket: import.meta.env.VITE_FIREBASE_STORAGE_BUCKET,
  messagingSenderId: import.meta.env.VITE_FIREBASE_MESSAGING_SENDER_ID,
  appId: import.meta.env.VITE_FIREBASE_APP_ID,
  measurementId: import.meta.env.VITE_FIREBASE_MEASUREMENT_ID,
};

const app = initializeApp(firebaseConfig);
export const auth = getAuth(app);
export const db = getFirestore(app);
export const analytics = typeof window !== 'undefined' ? getAnalytics(app) : null;
```

### auth.js Pattern
```javascript
import { GoogleAuthProvider, signInWithPopup, signOut, onAuthStateChanged } from 'firebase/auth';
import { auth } from './firebase.js';

const provider = new GoogleAuthProvider();

export async function loginWithGoogle() {
  try {
    const result = await signInWithPopup(auth, provider);
    return result.user;
  } catch (error) {
    if (error.code === 'auth/popup-closed-by-user') return null;
    throw error;
  }
}

export async function logout() {
  await signOut(auth);
}

export function onAuthChange(callback) {
  return onAuthStateChanged(auth, callback);
}

export function getCurrentUser() {
  return auth.currentUser;
}
```

---

## Firestore Schema Design

### Rules
1. **Flat structure** — avoid deep nesting (max 2 levels)
2. **Denormalize** — duplicate data to avoid extra reads
3. **Every document has**: `createdAt`, `updatedAt`, `ownerId`/`createdBy`
4. **Use subcollections** only when data grows independently

### CRUD Pattern (state.js)
```javascript
import { collection, doc, addDoc, getDocs, updateDoc, deleteDoc, query, where, orderBy, serverTimestamp } from 'firebase/firestore';
import { db } from './firebase.js';
import { getCurrentUser } from './auth.js';

export async function addItem(collectionName, data) {
  const user = getCurrentUser();
  if (!user) throw new Error('Not authenticated');
  
  return addDoc(collection(db, collectionName), {
    ...data,
    ownerId: user.uid,
    createdBy: user.uid,
    createdAt: serverTimestamp(),
    updatedAt: serverTimestamp(),
  });
}

export async function getItems(collectionName) {
  const user = getCurrentUser();
  if (!user) throw new Error('Not authenticated');
  
  const q = query(
    collection(db, collectionName),
    where('ownerId', '==', user.uid),
    orderBy('createdAt', 'desc')
  );
  
  const snapshot = await getDocs(q);
  return snapshot.docs.map(doc => ({ id: doc.id, ...doc.data() }));
}
```

---

## Security Rules Template

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    
    // Helper functions
    function isAuthenticated() {
      return request.auth != null;
    }
    
    function isOwner(ownerId) {
      return isAuthenticated() && request.auth.uid == ownerId;
    }
    
    function isValidCreate() {
      return request.resource.data.ownerId == request.auth.uid
          && request.resource.data.createdAt is timestamp;
    }
    
    // Per-collection rules
    match /{collection}/{docId} {
      allow read: if isOwner(resource.data.ownerId);
      allow create: if isAuthenticated() && isValidCreate();
      allow update: if isOwner(resource.data.ownerId);
      allow delete: if isOwner(resource.data.ownerId);
    }
    
    // User settings (userId = doc ID)
    match /settings/{userId} {
      allow read, write: if isOwner(userId);
    }
    
    // Deny everything else
    match /{document=**} {
      allow read, write: if false;
    }
  }
}
```

---

## API Key Security

| Key Type | Where to Store | Restriction |
|----------|---------------|-------------|
| Firebase API Key | `.env` (VITE_ prefix) | HTTP referrer + API scope |
| Gemini/OpenAI Key | Firestore per-user settings | HTTP referrer |
| Social API Tokens | Firestore per-user settings | Never in source code |

### Firebase API Key is PUBLIC by design
- Protected by Security Rules, not secrecy
- Restrict in Google Cloud Console → HTTP referrers
- Restrict API scope → only Firebase APIs

---

## Common Patterns

### Email Whitelist (Internal Apps)
```javascript
const ALLOWED_DOMAINS = ['@company.com'];

function isAuthorized(email) {
  return ALLOWED_DOMAINS.some(d => email.endsWith(d));
}
```

### Real-time Listener
```javascript
import { onSnapshot, query, where, collection } from 'firebase/firestore';

export function listenToItems(collectionName, userId, callback) {
  const q = query(
    collection(db, collectionName),
    where('ownerId', '==', userId)
  );
  
  return onSnapshot(q, (snapshot) => {
    const items = snapshot.docs.map(doc => ({ id: doc.id, ...doc.data() }));
    callback(items);
  });
}
```

### Composite Index
When querying with `where()` + `orderBy()` on different fields, Firestore requires a composite index. Check console errors for the auto-generated link to create it.

---

## Deployment Checklist

- [ ] Security Rules deployed (NOT test mode)
- [ ] Production domain added to Auth → Authorized domains
- [ ] API Key restricted (HTTP referrer + API scope)
- [ ] .env values set in hosting platform (Cloudflare Pages / Vercel)
- [ ] Analytics enabled (optional)
