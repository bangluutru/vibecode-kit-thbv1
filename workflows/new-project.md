---
description: Quy trình tổng thể để bắt đầu dự án mới — huy động toàn bộ kit (agents, skills, rules, master prompts, shared templates)
---

# /new-project — Quy trình khởi tạo & phát triển dự án mới

> **Meta-Workflow**: Đây là workflow tổng thể kết nối TẤT CẢ thành phần trong vibecode-kit-thbv1 vào một quy trình nhất quán từ ý tưởng đến triển khai.

---

## 📋 Tổng quan quy trình

```
P0: SETUP     →  P1: VISION    →  P2: BLUEPRINT  →  P3: SCAFFOLD
P4: BUILD     →  P5: QUALITY   →  P6: DEPLOY     →  P7: HANDOFF
```

Mỗi Phase huy động các thành phần kit cụ thể:

| Phase | Agents chính | Skills | Master Prompt | Workflows liên quan |
|-------|-------------|--------|---------------|---------------------|
| P0 | — | — | — | — |
| P1 | `project-planner`, `product-manager` | `product-discovery`, `prd-scope-control` | `VIBECODE-MASTER-v4` | `/brainstorm` |
| P2 | `project-planner`, `backend-specialist`, `frontend-specialist` | `modern-web-architect`, `database-patterns` | `VIBECODE-MASTER-thbv1` | `/plan` |
| P3 | `codebase-expert` | `clean-code`, archetype từ `archetypes.json` | — | `/scaffold` |
| P4 | `frontend-specialist`, `backend-specialist`, `mobile-developer` | Theo stack | `VIBECODE-MASTER-v4` | `/create`, `/ui-ux-pro-max` |
| P5 | `test-engineer`, `security-auditor`, `accessibility-auditor`, `performance-engineer` | `testing-master`, `vulnerability-scanner`, `accessibility-patterns` | `QA-MASTER-v4`, `A11Y-MASTER-v4`, `PERF-MASTER-v4` | `/test`, `/security`, `/accessibility`, `/review` |
| P6 | `cloud-architect` | `deployment-engineer`, `cloudflare-workers` | — | `/deploy` |
| P7 | `technical-writer`, `codebase-expert` | `api-documentation`, `clean-code` | `XRAY-MASTER-v4` | `/document`, `/update-docs` |

---

## P0: SETUP — Cấu hình ban đầu (5 phút)

### Bước 0.1: Copy kit vào dự án

```bash
# Tạo thư mục dự án mới
mkdir my-new-project && cd my-new-project
git init

# Copy bộ kit vào thư mục .agent/
cp -r /path/to/vibecode-kit-thbv1/ .agent/

# Hoặc clone trực tiếp
git clone https://github.com/bangluutru/vibecode-kit-thbv1.git .agent/
```

### Bước 0.2: Chọn archetype

Xem danh sách archetype có sẵn:
```bash
cat .agent/core/archetypes.json
```

Archetype có sẵn:
| Archetype | Mô tả | Khi nào dùng |
|-----------|--------|-------------|
| `web-nextjs-standard` | Next.js App Router | SaaS, Dashboard |
| `web-vite-vanilla` | Vite + Vanilla JS | Landing page, SPA nhẹ |
| `backend-nodejs-express` | Express REST API | API backend |
| `backend-python-fastapi` | Python FastAPI DDD | ML API, Data service |
| `mobile-react-native` | React Native | Mobile app |

### Bước 0.3: Kiểm tra rules

Rules tự động áp dụng cho toàn dự án — đọc qua để hiểu các ràng buộc:

```bash
ls .agent/rules/
```

| Rule | Tác dụng |
|------|----------|
| `frontend.md` | Chuẩn UI/UX, responsive, accessibility |
| `backend.md` | Clean architecture, API design |
| `security.md` | Input validation, auth, secrets management |
| `debug.md` | Error handling, logging patterns |
| `business.md` | Business logic constraints |
| `compliance.md` | Legal, privacy requirements |
| `architecture-review.md` | Architecture review gates |
| `error-logging.md` | Structured logging format |
| `malware-protection.md` | Dependency safety |
| `docs-update.md` | Documentation standards |
| `GEMINI.md` | AI-specific rules |

---

## P1: VISION — Xác định ý tưởng (30-60 phút)

### Agents được huy động
- 🎯 `project-planner` — Chiến lược & yêu cầu
- 📋 `product-manager` / `product-owner` — MVP scope

### Skills cần tải
- `product-discovery` — Phương pháp khám phá sản phẩm
- `prd-scope-control` — Kiểm soát scope PRD

### Master Prompt
- **`VIBECODE-MASTER-v4.txt`** — 6 bước: Vision → Context → Blueprint → Contract → Build → Refine

### Quy trình

**Bước 1.1: Brainstorm** (chạy `/brainstorm`)
```
Mô tả ý tưởng sản phẩm: [Mô tả ngắn]
Đối tượng người dùng: [Ai sẽ dùng?]
Vấn đề cần giải quyết: [Pain point chính]
```

**Bước 1.2: Xác định MVP**
```markdown
## MVP Definition
- Core features (max 3): 
  1. [Feature 1]
  2. [Feature 2]  
  3. [Feature 3]
- Out of scope (Phase 2+): [...]
- Tech stack decision: [...]
```

**Bước 1.3: Tạo PRD**

Sử dụng `prd-scope-control` skill để tạo document:
```markdown
## PRD — [Tên dự án]
### Problem Statement
### User Stories
### Success Metrics
### Technical Requirements
### Non-functional Requirements (Performance, Security, A11y)
```

### ✅ Gate Check P1
- [ ] MVP features xác định rõ (max 3)
- [ ] Tech stack đã chọn
- [ ] PRD viết xong
- [ ] Stakeholder đồng ý

---

## P2: BLUEPRINT — Thiết kế kiến trúc (1-2 giờ)

### Agents được huy động
- 🏗️ `project-planner` — Phân rã tác vụ
- 🔧 `backend-specialist` — API & Database design
- 🎨 `frontend-specialist` — UI/UX architecture
- 💾 `database-architect` — Schema design

### Skills cần tải
- `modern-web-architect` — Kiến trúc Next.js/React
- `database-patterns` — Schema design, indexing
- `api-integration` — REST/GraphQL patterns
- `firebase-patterns` — Nếu dùng Firebase

### Master Prompt
- **`VIBECODE-MASTER-thbv1.txt`** — P0-P8 execution lifecycle

### Quy trình

**Bước 2.1: Thiết kế hệ thống** (chạy `/plan`)
```markdown
## Architecture Decision Record
- Frontend: [framework + justification]
- Backend: [framework + justification]  
- Database: [type + justification]
- Auth: [strategy]
- Hosting: [platform]
```

**Bước 2.2: Thiết kế Database**
- Schema diagram (ERD)
- Bảng + quan hệ + indexes
- Tham khảo: `.agent/shared/database-master/`

**Bước 2.3: Thiết kế API**
- Endpoint list với request/response
- Tham khảo: `.agent/shared/api-standards/` và `.agent/shared/api-documentation/`

**Bước 2.4: Thiết kế UI/UX**
- Wireframe hoặc mô tả layout
- Design tokens (colors, typography, spacing)
- Tham khảo: `.agent/shared/design-system/` và `.agent/shared/ui-ux-pro-max/`

### ✅ Gate Check P2
- [ ] Architecture documented
- [ ] Database schema designed
- [ ] API endpoints listed
- [ ] UI wireframes/mockups ready
- [ ] Task breakdown created

---

## P3: SCAFFOLD — Tạo cấu trúc dự án (15-30 phút)

### Agents được huy động
- 🔬 `codebase-expert` — Project structure

### Skills cần tải
- `clean-code` — Naming conventions, file organization
- Archetype từ `archetypes.json`

### Quy trình

**Bước 3.1: Khởi tạo project** (chạy `/scaffold`)
```bash
# Ví dụ: Vite + Vanilla JS
npm create vite@latest ./ -- --template vanilla

# Ví dụ: Next.js
npx create-next-app@latest ./ --ts --tailwind --app --src-dir

# Ví dụ: FastAPI
pip install fastapi uvicorn
```

**Bước 3.2: Tạo cấu trúc thư mục**
Áp dụng archetype đã chọn ở P0:
```bash
# Tự động tạo folders theo archetype
mkdir -p src/{pages,components,services,utils,config,assets,styles}
```

**Bước 3.3: Cấu hình cơ bản**
- [ ] `.env.example` với placeholder values
- [ ] `.gitignore` đầy đủ
- [ ] `README.md` theo template từ `technical-writer` agent
- [ ] ESLint/Prettier config
- [ ] CI/CD pipeline cơ bản

**Bước 3.4: Cài dependencies**
```bash
npm install
# + dev dependencies: testing, linting, formatting
```

### ✅ Gate Check P3
- [ ] Project chạy được (`npm run dev`)
- [ ] Folder structure theo archetype
- [ ] Config files đầy đủ
- [ ] Git initialized + first commit

---

## P4: BUILD — Phát triển tính năng (Phần lớn thời gian)

### Agents được huy động (theo task)
- 🎨 `frontend-specialist` — UI components
- 🔧 `backend-specialist` — API endpoints
- 📱 `mobile-developer` — Mobile features
- 🎮 `game-developer` — Game mechanics
- 🐛 `debugger` — Khi gặp lỗi
- 🔍 `code-reviewer` — Review code

### Skills cần tải (theo stack)

| Stack | Skills |
|-------|--------|
| React/Next.js | `nextjs-react-expert`, `tailwind-patterns` |
| Vanilla JS | `vanilla-js-vite`, `css-design-system` |
| Node.js | `nodejs-best-practices` |
| Python | `python-patterns` |
| Firebase | `firebase-patterns` |
| AI/LLM | `ai-integration`, `prompt-engineering` |
| Mobile | `mobile-design` |
| i18n | `i18n-localization` |

### Master Prompt
- **`VIBECODE-MASTER-v4.txt`** — Build phase

### Quy trình

**Bước 4.1: Xây dựng từng feature** (chạy `/create` cho mỗi feature)

Thứ tự build recommended:
```
1. Auth (đăng nhập / đăng ký)       → backend-specialist + firebase-patterns
2. Layout (header, sidebar, footer)  → frontend-specialist + design-system
3. Core Feature 1                    → backend + frontend specialists
4. Core Feature 2                    → backend + frontend specialists
5. Core Feature 3                    → backend + frontend specialists
6. Settings / Profile                → backend-specialist
```

**Bước 4.2: UI Premium** (chạy `/ui-ux-pro-max` nếu cần)
- Tham khảo: `.agent/shared/design-philosophy/`
- Tham khảo: `.agent/shared/ui-ux-pro-max/`

**Bước 4.3: Debug khi gặp lỗi** (chạy `/debug`)
- Kích hoạt `debugger` agent
- Sử dụng **`DEBUG-MASTER-v4.txt`** — 9-step debug protocol
- Skill: `systematic-debugging`

**Bước 4.4: Code review** (chạy `/review`)
- Kích hoạt `code-reviewer` agent
- Checklist 3 tiers: Must-Fix → Should-Fix → Nice-to-Fix

### Handshake Protocol (Chuyển giao giữa agents)
Theo `core/personality.md`:
```
1. Input:       Yêu cầu gốc từ user
2. Context:     Kết quả của agent trước (mockup, schema, API...)
3. Constraints: Ràng buộc kỹ thuật (stack, rules, performance budget)
```

### ✅ Gate Check P4
- [ ] Tất cả MVP features hoạt động
- [ ] UI responsive (mobile + desktop)
- [ ] No console errors
- [ ] Code reviewed

---

## P5: QUALITY — Kiểm tra chất lượng (2-4 giờ)

### Agents được huy động
- 🧪 `test-engineer` — Unit/integration tests
- 🔒 `security-auditor` — Security audit
- ♿ `accessibility-auditor` — WCAG compliance
- ⚡ `performance-engineer` — Performance audit
- 🔍 `quality-inspector` — Final gate

### Skills cần tải
- `testing-master` — TDD, test pyramid
- `vulnerability-scanner` — OWASP scanning
- `accessibility-patterns` — WCAG 2.2 AA
- `performance-optimization` — Bundle, caching
- `red-team-tactics` — Adversarial testing

### Master Prompts
- **`QA-MASTER-v4.txt`** — 6-step QA process
- **`A11Y-MASTER-v4.txt`** — 6-step accessibility protocol
- **`PERF-MASTER-v4.txt`** — 7-step performance protocol

### Shared Templates
- `.agent/shared/testing-master/` — Test patterns
- `.agent/shared/security-armor/` — Security checklists
- `.agent/shared/accessibility/` — WCAG checklist, ARIA patterns
- `.agent/shared/vitals-templates/` — Web Vitals budgets

### Quy trình

**Bước 5.1: Testing** (chạy `/test`)
```bash
# Unit tests
npm run test:unit -- --coverage

# Integration tests  
npm run test:integration

# E2E tests
npx playwright test
```

**Bước 5.2: Security audit** (chạy `/security`)
```bash
npm audit
# + manual checklist từ security-auditor agent
# + Firebase Security Rules review
# + Secret scanning
```

**Bước 5.3: Accessibility audit** (chạy `/accessibility`)
```bash
npx @axe-core/cli <url>
npx lighthouse <url> --only-categories=accessibility
# + keyboard testing
# + screen reader testing
```

**Bước 5.4: Performance audit**
```bash
npx lighthouse <url> --only-categories=performance
npx vite-bundle-visualizer
```

**Bước 5.5: Code quality review** (chạy `/review`)
- Final review bởi `quality-inspector` agent

### ✅ Gate Check P5
- [ ] Test coverage ≥ 70%
- [ ] `npm audit` — no critical vulnerabilities
- [ ] Lighthouse Performance ≥ 80
- [ ] Lighthouse Accessibility ≥ 90
- [ ] Security checklist passed
- [ ] No blocking code review issues

---

## P6: DEPLOY — Triển khai (30-60 phút)

### Agents được huy động
- ☁️ `cloud-architect` — CI/CD & deployment
- 🔒 `security-auditor` — Pre-deploy security check

### Skills cần tải
- `cloudflare-workers` — Edge deployment
- `github-actions-templates` — CI/CD pipelines

### Quy trình

**Bước 6.1: Pre-deploy checklist** (chạy `/security`)
- [ ] No secrets in source code
- [ ] No secrets in build output
- [ ] Environment variables configured
- [ ] Database rules deployed
- [ ] API keys restricted

**Bước 6.2: Deploy** (chạy `/deploy`)

| Platform | Command |
|----------|---------|
| Cloudflare Pages | `npx wrangler pages deploy dist/` |
| Vercel | `npx vercel --prod` |
| Firebase Hosting | `npx firebase deploy` |
| Docker | `docker build . && docker push` |

**Bước 6.3: Post-deploy verification**
```bash
# Smoke test production
curl -s https://your-app.com/health
npx lighthouse https://your-app.com
```

### ✅ Gate Check P6
- [ ] App accessible on production URL
- [ ] HTTPS working
- [ ] Auth flow working
- [ ] Core features functional
- [ ] No console errors in production

---

## P7: HANDOFF — Bàn giao & tài liệu (1-2 giờ)

### Agents được huy động
- 📝 `technical-writer` — Documentation
- 🔬 `codebase-expert` — Code analysis

### Skills cần tải
- `api-documentation` — OpenAPI spec
- `clean-code` — Code organization

### Master Prompt
- **`XRAY-MASTER-v4.txt`** — 5-step codebase analysis

### Shared Templates
- `.agent/shared/api-documentation/` — API doc templates
- `.agent/shared/prompt-templates/` — Prompt docs (nếu có AI features)

### Quy trình

**Bước 7.1: Documentation** (chạy `/document` hoặc `/update-docs`)
- [ ] README.md hoàn chỉnh (setup, features, deployment)
- [ ] API documentation (OpenAPI hoặc Markdown)
- [ ] CHANGELOG.md
- [ ] Architecture diagram
- [ ] `.env.example` up-to-date

**Bước 7.2: X-Ray report** (optional, cho handover)
- Chạy X-Ray protocol từ `XRAY-MASTER-v4.txt`
- Tạo `PROJECT_XRAY.md` — phân tích toàn bộ codebase

**Bước 7.3: Final commit & tag**
```bash
git add -A
git commit -m "v1.0.0 — MVP release"
git tag -a v1.0.0 -m "First production release"
git push origin main --tags
```

### ✅ Gate Check P7
- [ ] README.md complete
- [ ] API docs generated
- [ ] CHANGELOG up-to-date
- [ ] Git tagged with version

---

## 🔄 Chu kỳ PDCA (sau khi launch)

Sau P7, dự án chuyển sang chu kỳ cải tiến liên tục:

```
Plan  → Lên kế hoạch feature mới (/plan)
Do    → Phát triển (/create)
Check → Kiểm tra chất lượng (/test, /security, /accessibility)
Act   → Triển khai & cải thiện (/deploy, /review)
```

Mỗi iteration quay lại **P4 → P5 → P6** với feature mới.

---

## 📌 Quick Reference — Agent Routing

Khi gặp task cụ thể, huy động agent phù hợp:

| Task | Agent | Skill |
|------|-------|-------|
| Thiết kế UI | `frontend-specialist` | `frontend-design`, `ui-ux-pro-max` |
| Viết API | `backend-specialist` | `api-integration`, `nodejs-best-practices` |
| Thiết kế DB | `database-architect` | `database-patterns` |
| Fix bug | `debugger` | `systematic-debugging` |
| Viết test | `test-engineer` | `testing-master`, `test-driven-development` |
| Audit bảo mật | `security-auditor` | `vulnerability-scanner`, `red-team-tactics` |
| Audit a11y | `accessibility-auditor` | `accessibility-patterns` |
| Tối ưu tốc độ | `performance-engineer` | `performance-optimization` |
| Viết docs | `technical-writer` | `api-documentation` |
| Review code | `code-reviewer` | `clean-code`, `production-code-audit` |
| Thiết kế prompt | `prompt-engineer` | `prompt-engineering`, `ai-integration` |
| CI/CD | `cloud-architect` | `github-actions-templates`, `cloudflare-workers` |
| SEO | `seo-specialist` | `seo-fundamentals` |
| Mobile | `mobile-developer` | `mobile-design` |
| Game | `game-developer` | `game-development` |
| Orchestrate | `orchestrator` | `intelligent-routing`, `parallel-agents` |
