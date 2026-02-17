---
description: Code review workflow with PR standards and quality gates
---

# /review — Code Review Workflow

## When to use
- Before merging any PR
- During pair programming sessions
- For post-mortem code audits
- When onboarding new team members to codebase

---

## Step 1: Context & Scope

Before reviewing:
1. Read PR title and description
2. Understand the intent (what problem is being solved?)
3. Check PR size:
   - XS (<50L): Quick scan (5-10 min)
   - S (50-200L): Standard review (15-30 min)
   - M (200-500L): Deep review (30-60 min)
   - L (500+L): **Request split into smaller PRs**

---

## Step 2: Test Review (Read Tests First)

1. [ ] Tests exist for new functionality
2. [ ] Tests cover happy path + error cases
3. [ ] Test names describe expected behavior
4. [ ] No flaky tests introduced
5. [ ] Coverage didn't decrease

---

## Step 3: Code Quality Review

### Tier 1: Must-Fix (Blocking 🔴)

- [ ] **Security**: No hardcoded secrets, injection vulnerabilities, XSS
- [ ] **Correctness**: Logic matches requirements
- [ ] **Data safety**: No data loss or corruption risk
- [ ] **Breaking changes**: API contracts preserved (or versioned)

### Tier 2: Should-Fix (Important 🟡)

- [ ] **Error handling**: All error paths covered
- [ ] **Performance**: No N+1 queries, memory leaks, unnecessary renders
- [ ] **Types**: Proper typing (no `any`)
- [ ] **Tests**: Adequate coverage for changes

### Tier 3: Suggestions (Nice-to-have 💡)

- [ ] **Naming**: Clear and descriptive
- [ ] **Simplification**: Could be simpler?
- [ ] **DRY**: Duplicated logic?
- [ ] **Documentation**: Complex logic commented?

---

## Step 4: Comment Standards

Use these prefixes for all review comments:

```
🔴 BLOCKER: Must fix before merge
   Example: "This SQL query is vulnerable to injection"

🟡 SUGGESTION: Would improve code quality
   Example: "Consider extracting this to a shared utility"

💡 NIT: Minor style/preference
   Example: "Convention is camelCase here"

❓ QUESTION: Need clarification
   Example: "Why was this approach chosen over X?"

👍 PRAISE: Highlight good code
   Example: "Great error handling pattern here!"
```

---

## Step 5: Decision

| Decision | When |
|----------|------|
| ✅ **Approve** | No blockers, suggestions are minor |
| 🔄 **Request Changes** | Has blockers (Tier 1 issues) |
| 💬 **Comment** | Questions need answers, need more context |

---

## Step 6: Post-Merge

After merge:
1. [ ] CI/CD pipeline passes
2. [ ] No regression in staging/production
3. [ ] Documentation updated (if API changes)
4. [ ] CHANGELOG updated (if user-facing changes)

---

## PR Template

```markdown
## Summary
[1-2 sentence description of changes]

## Type
- [ ] 🆕 Feature
- [ ] 🐛 Bug fix
- [ ] ♻️ Refactor
- [ ] 📝 Documentation
- [ ] 🔒 Security fix

## Changes
- Changed X to support Y
- Added Z for W functionality

## Testing
- [ ] Unit tests added/updated
- [ ] Integration tests pass
- [ ] Manual testing completed

## Screenshots (if UI changes)

## Deployment Notes
[Any special steps needed]
```
