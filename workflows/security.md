---
description: Kiểm tra bảo mật toàn diện — scan secrets, hardening auth, database rules, API restrictions
---

# /security - Security Hardening (Full Audit)

$ARGUMENTS

---

## 🔴 CRITICAL: Chạy workflow này TRƯỚC MỖI LẦN DEPLOY

---

## Phase 1: Secrets Scan

### 1a. Scan hardcoded secrets trong source code

// turbo
```bash
grep -rnE "(AIzaSy|sk-|ghp_|password\s*=\s*['\"][^'\"]+|secret\s*=\s*['\"][^'\"]+|token\s*=\s*['\"][^'\"]+|PRIVATE_KEY)" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" --include="*.html" --include="*.json" . | grep -v node_modules | grep -v ".env.example" | grep -v package-lock | head -30
```

**Nếu tìm thấy secrets:**
- Di chuyển vào `.env` (cho build-time config)
- Hoặc lưu per-user trong database (cho user-specific secrets)
- KHÔNG BAO GIỜ hardcode trong source code

### 1b. Kiểm tra .gitignore

// turbo
```bash
cat .gitignore | grep -E "\.env|node_modules|dist" && echo "✅ .gitignore OK" || echo "⚠️ THIẾU entries trong .gitignore!"
```

Phải có tối thiểu:
```
.env
.env.local
.env.production
.env.*.local
node_modules/
dist/
```

### 1c. Scan git history

// turbo
```bash
git log --all --diff-filter=A --name-only --pretty=format: 2>/dev/null | grep -E "\.env$|\.env\.local$|\.env\.production$" | head -5 && echo "⚠️ .env TỪNG ĐƯỢC COMMIT! Phải rotate keys ngay!" || echo "✅ Không có .env trong git history"
```

### 1d. Scan build output

// turbo
```bash
npm run build 2>/dev/null && grep -rnE "(sk-|ghp_|password|PRIVATE_KEY)" dist/ --include="*.js" --include="*.html" | head -10 && echo "⚠️ Secrets trong build output!" || echo "✅ Build output sạch"
```

---

## Phase 2: Dependency Security

### 2a. npm audit

// turbo
```bash
npm audit --audit-level=high 2>/dev/null || echo "No npm audit available"
```

### 2b. Outdated packages

// turbo
```bash
npm outdated 2>/dev/null | head -20
```

---

## Phase 3: Authentication Hardening

### 3a. Authorized Domains (Firebase/Supabase)

Kiểm tra Auth provider → Settings → Authorized domains:
- ✅ `localhost` (dev only)
- ✅ Production domain
- ❌ Xoá tất cả domain lạ/không cần thiết

### 3b. Sign-in Methods

Chỉ bật methods thực sự cần:
- ✅ Google (nếu dùng)
- ❌ Tắt Email/Password nếu không dùng
- ❌ Tắt Anonymous nếu không cần

### 3c. Email Whitelist (cho app nội bộ)

Nếu app chỉ cho team:

```javascript
// auth guard — chặn đăng nhập trái phép
const ALLOWED_EMAILS = ['user@company.com'];
const ALLOWED_DOMAINS = ['@company.com'];

function isAuthorized(email) {
  return ALLOWED_EMAILS.includes(email)
    || ALLOWED_DOMAINS.some(d => email.endsWith(d));
}

// Trong login flow:
if (!isAuthorized(user.email)) {
  await signOut(auth);
  showError('Tài khoản không được phép truy cập');
  return;
}
```

---

## Phase 4: Database Security Rules

### 4a. Firestore Rules

❌ **NGUY HIỂM** (test mode):
```
allow read, write: if true;
```

✅ **Đúng cách** — Data isolation:
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{collection}/{docId} {
      allow read: if request.auth != null
                  && resource.data.ownerId == request.auth.uid;
      allow create: if request.auth != null
                    && request.resource.data.ownerId == request.auth.uid;
      allow update, delete: if request.auth != null
                            && resource.data.ownerId == request.auth.uid;
    }
    match /{document=**} {
      allow read, write: if false;
    }
  }
}
```

### 4b. Supabase RLS (nếu dùng)

```sql
ALTER TABLE items ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Users see own data" ON items
  FOR SELECT USING (auth.uid() = user_id);

CREATE POLICY "Users insert own data" ON items
  FOR INSERT WITH CHECK (auth.uid() = user_id);
```

---

## Phase 5: API Key Restrictions

### Firebase API Key
Google Cloud Console → APIs & Services → Credentials:
- **HTTP referrers**: `{domain}/*`, `localhost:*`
- **API scope**: chỉ Firebase Auth, Firestore, Installations

### Gemini/OpenAI API Key
- **HTTP referrers**: `{domain}/*`, `localhost:*`
- Đặt spending limit

### Social API Keys (Facebook, etc.)
- Lưu per-user trong database (KHÔNG trong source code)
- Set IP whitelist nếu có

---

## Phase 6: Headers & CORS

### 6a. Security Headers

Kiểm tra response headers:
```
Content-Security-Policy: default-src 'self'
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
Strict-Transport-Security: max-age=31536000
```

### 6b. CORS Configuration

Chỉ cho phép origins cần thiết:
```javascript
const allowedOrigins = [
  'https://your-domain.com',
  'http://localhost:3000', // dev only
];
```

---

## ✅ Security Checklist (Pre-Deploy)

```markdown
## 🔒 Security Checklist

### Secrets
- [ ] .env trong .gitignore
- [ ] Không có secrets hardcoded trong source
- [ ] .env chưa từng bị commit vào git
- [ ] Build output không chứa secret keys
- [ ] npm audit không có critical vulnerabilities

### Authentication
- [ ] Authorized domains chỉ có domain hợp lệ
- [ ] Chỉ bật sign-in methods cần thiết
- [ ] Email whitelist (nếu app nội bộ)

### Database
- [ ] KHÔNG dùng test mode trên production
- [ ] Data isolation — user chỉ thấy data mình

### API Keys
- [ ] Firebase key restricted by HTTP referrer
- [ ] Firebase key restricted by API scope
- [ ] AI API key restricted
- [ ] Social tokens lưu per-user

### Headers
- [ ] Security headers configured
- [ ] CORS chỉ cho phép origins cần thiết
```

---

## Usage

```
/security             # Full audit (6 phases)
/security scan        # Phase 1 only (secrets scan)
/security auth        # Phase 3 only (auth hardening)
/security rules       # Phase 4 only (database rules)
/security checklist   # Phase 6 only (pre-deploy checklist)
```
