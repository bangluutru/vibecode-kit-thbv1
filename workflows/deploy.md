---
description: Code xong rồi? Đẩy lên Server/Vercel thôi.
---

# /deploy - Production Deployment

$ARGUMENTS

---

## Purpose

This command handles production deployment with pre-flight checks, deployment execution, and verification.

---

## Sub-commands

```
/deploy            - Interactive deployment wizard
/deploy check      - Run pre-deployment checks only
/deploy preview    - Deploy to preview/staging
/deploy production - Deploy to production
/deploy rollback   - Rollback to previous version
```

---

## Pre-Deployment Checklist

Before any deployment:

```markdown
## 🚀 Pre-Deploy Checklist

### Code Quality
- [ ] No TypeScript errors (`npx tsc --noEmit`)
- [ ] ESLint passing (`npx eslint .`)
- [ ] All tests passing (`npm test`)

### Security
- [ ] No hardcoded secrets
- [ ] Environment variables documented
- [ ] Dependencies audited (`npm audit`)

### Performance
- [ ] Bundle size acceptable
- [ ] No console.log statements
- [ ] Images optimized

### Documentation
- [ ] README updated
- [ ] CHANGELOG updated
- [ ] API docs current

### Ready to deploy? (y/n)
```

---

## Deployment Flow

```
┌─────────────────┐
│  /deploy        │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Pre-flight     │
│  checks         │
└────────┬────────┘
         │
    Pass? ──No──► Fix issues
         │
        Yes
         │
         ▼
┌─────────────────┐
│  Build          │
│  application    │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Deploy to      │
│  platform       │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Health check   │
│  & verify       │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  ✅ Complete    │
└─────────────────┘
```

---

## Output Format

### Successful Deploy

```markdown
## 🚀 Deployment Complete

### Summary
- **Version:** v1.2.3
- **Environment:** production
- **Duration:** 47 seconds
- **Platform:** Vercel

### URLs
- 🌐 Production: https://app.example.com
- 📊 Dashboard: https://vercel.com/project

### What Changed
- Added user profile feature
- Fixed login bug
- Updated dependencies

### Health Check
✅ API responding (200 OK)
✅ Database connected
✅ All services healthy
```

### Failed Deploy

```markdown
## ❌ Deployment Failed

### Error
Build failed at step: TypeScript compilation

### Details
```
error TS2345: Argument of type 'string' is not assignable...
```

### Resolution
1. Fix TypeScript error in `src/services/user.ts:45`
2. Run `npm run build` locally to verify
3. Try `/deploy` again

### Rollback Available
Previous version (v1.2.2) is still active.
Run `/deploy rollback` if needed.
```

---

## Platform Support

| Platform | Command | Best For |
|----------|---------|----------|
| **Cloudflare Pages** | Git integration | Static/Vite sites, best free tier |
| Vercel | `vercel --prod` | Next.js, React |
| Railway | `railway up` | Full-stack, databases |
| Fly.io | `fly deploy` | Docker containers |
| Docker | `docker compose up -d` | Self-hosted |

### Cloudflare Pages Setup

1. [dash.cloudflare.com](https://dash.cloudflare.com) → Workers & Pages → Create → Pages
2. Connect to Git → select repo
3. Configure:
   - Framework preset: **None** (for Vite + Vanilla JS), or select matching framework
   - Build command: `npm run build`
   - Output directory: `dist`
4. **Environment Variables** → Add all vars from `.env`
5. Save and Deploy → URL: `{project}.pages.dev`

### Post-Deploy: Auth Domain Authorization

⚠️ **If using Firebase/Supabase Auth:**
1. Add production domain to Auth → Authorized domains
2. Without this, login will fail with "The requested action is invalid"

---

## Post-Deploy Verification

```markdown
## ✅ Post-Deploy Checklist

- [ ] Site loads correctly
- [ ] Login flow works on production domain
- [ ] Core features functional
- [ ] No console errors
- [ ] API connections working
- [ ] Mobile responsive
```

---

## Examples

```
/deploy
/deploy check
/deploy preview
/deploy production --skip-tests
/deploy rollback
```

