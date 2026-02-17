---
description: Setup Firebase/Supabase — Auth, Database, Security Rules, Analytics
---

# /init-baas - Backend-as-a-Service Setup

$ARGUMENTS

---

## Purpose

Cài đặt và cấu hình Backend-as-a-Service (Firebase, Supabase, PocketBase).

---

## Sub-commands

```
/init-baas firebase    # Setup Firebase (Auth + Firestore + Analytics)
/init-baas supabase    # Setup Supabase (Auth + PostgreSQL + RLS)
/init-baas check       # Verify current BaaS configuration
```

---

## Firebase Setup Flow

### Step 1: Create Project

1. Vào [Firebase Console](https://console.firebase.google.com)
2. Create Project → nhập tên
3. Enable Google Analytics (optional)
4. Project Settings → Web app → Register app
5. Copy config object

### Step 2: Install SDK

// turbo
```bash
npm install firebase
```

### Step 3: Create Firebase Module

```javascript
// js/firebase.js
import { initializeApp } from 'firebase/app';
import { getAuth } from 'firebase/auth';
import { getFirestore } from 'firebase/firestore';

const firebaseConfig = {
  apiKey: import.meta.env.VITE_FIREBASE_API_KEY,
  authDomain: import.meta.env.VITE_FIREBASE_AUTH_DOMAIN,
  projectId: import.meta.env.VITE_FIREBASE_PROJECT_ID,
  storageBucket: import.meta.env.VITE_FIREBASE_STORAGE_BUCKET,
  messagingSenderId: import.meta.env.VITE_FIREBASE_MESSAGING_SENDER_ID,
  appId: import.meta.env.VITE_FIREBASE_APP_ID,
};

const app = initializeApp(firebaseConfig);
export const auth = getAuth(app);
export const db = getFirestore(app);
```

### Step 4: Create .env

```bash
# Firebase Configuration
VITE_FIREBASE_API_KEY=your_api_key
VITE_FIREBASE_AUTH_DOMAIN=your_project.firebaseapp.com
VITE_FIREBASE_PROJECT_ID=your_project_id
VITE_FIREBASE_STORAGE_BUCKET=your_project.appspot.com
VITE_FIREBASE_MESSAGING_SENDER_ID=123456789
VITE_FIREBASE_APP_ID=your_app_id
VITE_FIREBASE_MEASUREMENT_ID=G-XXXXXXXXXX
```

### Step 5: Enable Authentication

Firebase Console → Authentication → Sign-in method:
- Enable Google (recommended for internal apps)
- Add authorized domains: `localhost`, production domain

### Step 6: Create Firestore

Firebase Console → Firestore Database → Create:
- Start in **test mode** (for development)
- Choose region closest to users

### Step 7: Auth Module

```javascript
// js/auth.js
import { GoogleAuthProvider, signInWithPopup, signOut, onAuthStateChanged } from 'firebase/auth';
import { auth } from './firebase.js';

export async function loginWithGoogle() {
  const provider = new GoogleAuthProvider();
  const result = await signInWithPopup(auth, provider);
  return result.user;
}

export async function logout() {
  await signOut(auth);
}

export function onAuthChange(callback) {
  return onAuthStateChanged(auth, callback);
}
```

### Step 8: Deploy Security Rules (BEFORE going to production)

⚠️ Run `/security rules` to set up proper Firestore Security Rules.

---

## Supabase Setup Flow

### Step 1: Create Project

1. [app.supabase.com](https://app.supabase.com) → New project
2. Set database password
3. Wait for provisioning

### Step 2: Install SDK

// turbo
```bash
npm install @supabase/supabase-js
```

### Step 3: Create Client

```javascript
import { createClient } from '@supabase/supabase-js';

const supabase = createClient(
  import.meta.env.VITE_SUPABASE_URL,
  import.meta.env.VITE_SUPABASE_ANON_KEY
);
export default supabase;
```

### Step 4: Enable RLS

```sql
ALTER TABLE your_table ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Users see own data" ON your_table
  FOR SELECT USING (auth.uid() = user_id);
```

---

## Verification

After setup, verify:

```markdown
## ✅ BaaS Setup Verification

- [ ] SDK installed and imported
- [ ] .env configured with correct values
- [ ] .env in .gitignore
- [ ] Auth enabled and working (test login)
- [ ] Database created
- [ ] Security Rules applied (NOT test mode)
- [ ] Production domain added to authorized domains
```

---

## Usage

```
/init-baas firebase       # Full Firebase setup guide
/init-baas supabase       # Full Supabase setup guide
/init-baas check          # Verify current setup
```
