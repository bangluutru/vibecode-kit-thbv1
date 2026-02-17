---
description: Tạo khung sườn project trước khi code — skeleton files, design system, config
---

# /scaffold - Project Scaffolding

$ARGUMENTS

---

## Purpose

Tạo **tất cả file skeleton** theo architecture TRƯỚC KHI implement.
AI sẽ hiểu toàn bộ cấu trúc → code chính xác hơn, không bị drift.

> ⚠️ Chạy SAU `/plan` và TRƯỚC `/create`

---

## Steps

### 1. Init Project

// turbo
```bash
mkdir -p {project-dir}
```

Detect tech stack từ plan file và init:

| Stack | Init Command |
|-------|-------------|
| Vite + Vanilla JS | `npm init -y` + add vite |
| Vite + React | `npx -y create-vite@latest ./ --template react` |
| Next.js | `npx -y create-next-app@latest ./ --use-npm` |
| Express | `npm init -y` + add express |

### 2. Create Config Files

// turbo
Tạo các file cấu hình:
- `package.json` — dependencies
- Build config (vite.config.js / next.config.js / tsconfig.json)
- `.env.example` — environment variable template  (placeholder values only!)
- `.gitignore` — exclude .env, node_modules, dist, .DS_Store
- `README.md` — project overview

### 3. Create Design System (Web projects)

// turbo
```
css/
├── variables.css   ← Design tokens (colors, spacing, fonts, shadows)
├── base.css        ← Reset, typography, animations, utilities
├── layout.css      ← App shell (sidebar, header, main content)
└── components.css  ← Buttons, cards, forms, modals, toasts
```

**Requirements:**
- Dark theme với premium feel
- CSS Variables for all tokens
- Responsive breakpoints
- Smooth transitions (0.2-0.3s)

### 4. Create HTML Shell (SPA)

// turbo
```html
<!-- index.html -->
- Meta tags (viewport, charset, description)
- Google Fonts link
- CSS imports
- App shell: sidebar + header + main content area
- Login root element
- Toast container
- JS entry point
```

### 5. Create Core JS Modules (Skeleton)

// turbo
Mỗi file chỉ chứa:
- Export functions với JSDoc (params, return type)
- `// TODO: implement` trong mỗi function body
- Comment mô tả trách nhiệm của module

```
js/
├── app.js          ← Entry point, init, wiring
├── config.js       ← Routes, constants, API endpoints
├── auth.js         ← Authentication (login, logout, state)
├── state.js        ← Data layer / CRUD operations
├── router.js       ← SPA routing
├── services/       ← 1 file per external API
├── pages/          ← 1 file per route/page
├── components/     ← Reusable UI components
└── utils/          ← DOM helpers, formatters, storage
```

### 6. Create Utility Files

// turbo
- `utils/dom.js` — DOM helper functions
- `utils/format.js` — Date, number, text formatting
- `utils/storage.js` — LocalStorage/SessionStorage helpers
- `components/toast.js` — **Implement fully** (simple, reusable)

### 7. Verify Structure

// turbo
```bash
find . -type f -not -path '*/node_modules/*' -not -path '*/.git/*' -not -name '.DS_Store' | sort
```

Report file count and structure to user for approval.

---

## Output

```markdown
## 🏗️ Scaffold Complete

### Files Created: {count}
### Structure:
{tree output}

### Next Steps:
- Review the file structure
- Run `/create` to start implementing features
- Build order: config → auth → state → router → services → pages → app
```

---

## Key Principles

- **Skeleton first, code later** — understand the whole before coding parts
- **1 file = 1 responsibility** — max 200-400 lines per file
- **Design system upfront** — consistent UI from day 1
- **Toast/utils fully implemented** — small utilities, implement immediately
- **User checkpoint** — approve structure before implementation

---

## Usage

```
/scaffold                        # Interactive (asks questions)
/scaffold vite-firebase          # Preset: Vite + Firebase
/scaffold nextjs-supabase        # Preset: Next.js + Supabase
/scaffold express-postgresql     # Preset: Express + PostgreSQL
```
