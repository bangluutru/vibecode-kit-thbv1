---
description: Git workflow — init, commit, push, branch, PR với security scan tự động
---

# /git - Git Workflow Management

$ARGUMENTS

---

## Sub-commands

```
/git init         - Init repo + .gitignore + first commit
/git push         - Security scan → commit → push
/git branch       - Create/switch feature branch
/git pr           - Generate PR description
/git status       - Show git status + diff summary
```

---

## /git init

### Steps:

1. Create comprehensive .gitignore:

// turbo
```bash
cat > .gitignore << 'EOF'
# Dependencies
node_modules/
.pnp
.pnp.js

# Build outputs
dist/
build/
.next/
out/

# Environment
.env
.env.local
.env.production
.env.*.local

# IDE
.vscode/settings.json
.idea/
*.swp
*.swo

# OS
.DS_Store
Thumbs.db

# Logs
*.log
npm-debug.log*

# Testing
coverage/
.nyc_output/

# Misc
*.tgz
.cache/
EOF
```

2. Init and first commit:

```bash
git init
git branch -M main
git add .
git commit -m "🎉 init: project setup"
```

3. Add remote (if provided):

```bash
git remote add origin {remote-url}
git push -u origin main
```

---

## /git push

### Pre-push Security Scan (MANDATORY)

Before every push, automatically run:

// turbo
```bash
# 1. Check for secrets
grep -rnE "(AIzaSy|sk-|ghp_|password\s*=\s*['\"][^'\"]+)" --include="*.js" --include="*.ts" . | grep -v node_modules | grep -v ".env.example" | head -5

# 2. Verify .env not staged
git diff --cached --name-only | grep "\.env$" && echo "⚠️ STOP: .env is staged!" && exit 1 || echo "✅ No .env staged"
```

### Commit and Push

```bash
git add .
git commit -m "{type}: {message}"
git push
```

### Commit Message Convention

| Type | Usage |
|------|-------|
| `🎉 init:` | Initial commit |
| `✨ feat:` | New feature |
| `🐛 fix:` | Bug fix |
| `💄 style:` | UI/CSS changes |
| `♻️ refactor:` | Code restructure |
| `📝 docs:` | Documentation |
| `🔒 security:` | Security fix |
| `🚀 deploy:` | Deployment |
| `🔧 config:` | Configuration |

---

## /git branch

### Branch Strategy

```
main           ← Production-ready code
├── dev        ← Integration branch (optional)
├── feat/*     ← Feature branches
├── fix/*      ← Bug fix branches
└── hotfix/*   ← Emergency fixes
```

### Create Feature Branch

```bash
git checkout -b feat/{feature-name}

# After work is done:
git checkout main
git merge feat/{feature-name}
git branch -d feat/{feature-name}
```

---

## /git pr

Generate pull request description:

```markdown
## What Changed
[Summary of changes]

## Files Modified
[List of files with brief description]

## Testing
- [ ] Tested locally
- [ ] No console errors
- [ ] Responsive check
- [ ] Security scan passed

## Screenshots
[If UI changes]
```

---

## /git status

// turbo
```bash
echo "=== Git Status ===" && git status --short && echo "" && echo "=== Recent Commits ===" && git log --oneline -5 && echo "" && echo "=== Changed Files ===" && git diff --stat HEAD~1 2>/dev/null || echo "First commit"
```

---

## Usage

```
/git init
/git init https://github.com/user/repo.git
/git push "Added login feature"
/git branch feat/dark-mode
/git pr
/git status
```
