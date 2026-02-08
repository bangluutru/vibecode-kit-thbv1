# 🚀 Hướng Dẫn Sử Dụng THB Skill Stack v1 — Từ Ý Tưởng Đến Vận Hành

## 📋 Mục Lục
1. [Tổng Quan](#tổng-quan)
2. [Chuẩn Bị](#chuẩn-bị)
3. [Phase 1: Discovery & Research](#phase-1-discovery--research)
4. [Phase 2: Design & Architecture](#phase-2-design--architecture)
5. [Phase 3: Implementation](#phase-3-implementation)
6. [Phase 4: Testing & Quality](#phase-4-testing--quality)
7. [Phase 5: Deployment](#phase-5-deployment)
8. [Phase 6: Operations & Monitoring](#phase-6-operations--monitoring)
9. [Phase 7: Security](#phase-7-security)
10. [Master Prompts (Chuyên Sâu)](#master-prompts)
11. [Quick Reference](#quick-reference)

---

## Tổng Quan

Bộ **THB Skill Stack v1** gồm **31 skills tinh gọn** (15 Custom + 16 AGT-IDE) và **4 Master Prompts** chuyên sâu, được chọn lọc từ ~90 skills qua 6 nguồn khác nhau.

**Triết lý:** Ít nhưng sắc > Nhiều nhưng mỏng.

### 🗺️ Skill Flow Overview

```
┌──────────────────────────────────────────────────────────────────┐
│                         IDEA / YÊU CẦU                           │
└─────────────────────────────┬────────────────────────────────────┘
                              ▼
┌──────────────────────────────────────────────────────────────────┐
│  PHASE 1: DISCOVERY         │  Skills:                           │
│  - Nghiên cứu, brainstorm   │  • multi-agent-brainstorming       │
│  - Xác định scope, PRD      │  • product-discovery               │
│  - Kiểm soát phạm vi        │  • prd-scope-control               │
└─────────────────────────────┬────────────────────────────────────┘
                              ▼
┌──────────────────────────────────────────────────────────────────┐
│  PHASE 2: DESIGN            │  Skills:                           │
│  - Orchestrate thiết kế     │  • design-orchestration            │
│  - Frontend/UX              │  • frontend-design                 │
│  - Mobile UX                │  • mobile-design                   │
│  - SEO baseline             │  • seo-fundamentals                │
└─────────────────────────────┬────────────────────────────────────┘
                              ▼
┌──────────────────────────────────────────────────────────────────┐
│  PHASE 3: IMPLEMENTATION    │  Skills:                           │
│  - AI Agent orchestration   │  • intelligent-routing             │
│  - Subagent execution       │  • subagent-driven-development     │
│  - Agent modes              │  • behavioral-modes                │
│  - Parallel execution       │  • parallel-agents                 │
│  - Clean code               │  • clean-code (AGT-IDE)            │
│  - Language-specific:       │  • python-patterns                 │
│                             │  • nodejs-best-practices           │
│                             │  • nextjs-react-expert             │
│  - Styling                  │  • tailwind-patterns               │
│  - i18n                     │  • i18n-localization               │
│  - Game dev                 │  • game-development                │
│  - MCP tooling              │  • mcp-builder                     │
└─────────────────────────────┬────────────────────────────────────┘
                              ▼
┌──────────────────────────────────────────────────────────────────┐
│  PHASE 4: TESTING           │  Skills:                           │
│  - TDD enforcement          │  • test-driven-development         │
│  - E2E browser testing      │  • playwright-skill                │
│  - UX validation            │  • usability-testing               │
│  - Debug (systematic)       │  • systematic-debugging            │
│  ──────────────────────────────────────────────────────────────  │
│  Master Prompt (chuyên sâu):│  • DEBUG-MASTER-v4                 │
│                             │  • QA-MASTER-v4                    │
└─────────────────────────────┬────────────────────────────────────┘
                              ▼
┌──────────────────────────────────────────────────────────────────┐
│  PHASE 5: DEPLOYMENT        │  Skills:                           │
│  - CI/CD pipeline           │  • github-actions-templates        │
└─────────────────────────────┬────────────────────────────────────┘
                              ▼
┌──────────────────────────────────────────────────────────────────┐
│  PHASE 6: OPERATIONS        │  Skills:                           │
│  - Metrics                  │  • prometheus-configuration        │
│  - Dashboards               │  • grafana-dashboards              │
│  - Log aggregation          │  • loki-mode                       │
│  - Incident handling        │  • incident-runbook-templates      │
└─────────────────────────────┬────────────────────────────────────┘
                              ▼
┌──────────────────────────────────────────────────────────────────┐
│  PHASE 7: SECURITY          │  Skills:                           │
│  - Vulnerability scanning   │  • vulnerability-scanner           │
│  - Privacy/compliance       │  • privacy-compliance              │
│  - Offensive testing        │  • red-team-tactics                │
└──────────────────────────────────────────────────────────────────┘
```

---

## Chuẩn Bị

### Copy Skills vào Project

```bash
# Copy toàn bộ 31 skills
cp -r vibecode-kit-thbv1/skills/* .agent/skills/

# Hoặc chỉ copy skills cho phase cụ thể
cp -r vibecode-kit-thbv1/skills/multi-agent-brainstorming .agent/skills/
cp -r vibecode-kit-thbv1/skills/prd-scope-control .agent/skills/
```

### Cấu Trúc Thư Mục Đề Xuất

```
your-project/
├── .agent/
│   └── skills/              # Skills đang active
├── docs/
│   ├── prd.md              # Product Requirements
│   ├── design.md           # Design decisions
│   └── architecture.md     # System architecture
├── src/
└── tests/
```

---

## Phase 1: Discovery & Research

> **Mục tiêu:** Hiểu vấn đề, brainstorm đa chiều, xác định scope

### Step 1.1: Product Discovery

**Khi nào dùng:** Bắt đầu từ ý tưởng thô

```
Prompt: "Sử dụng skill product-discovery cho ý tưởng: [mô tả]"
```

**Quy trình:**
1. Problem Statement → Ai dùng? Vấn đề gì?
2. Success Metrics → Đo lường thành công thế nào?
3. Constraints → Giới hạn thời gian, budget, tech?

### Step 1.2: Multi-Agent Brainstorming

**Khi nào dùng:** Thiết kế phức tạp cần nhiều góc nhìn

```
Prompt: "Sử dụng skill multi-agent-brainstorming để review [ý tưởng/thiết kế]"
```

**5 Agent Roles:**
| Agent | Vai trò |
|-------|---------|
| Primary Designer | Lead design, maintain decision log |
| Skeptic | Challenge assumptions, tìm điểm yếu |
| Constraint Guardian | Kiểm tra non-functional requirements |
| User Advocate | Đại diện end user |
| Integrator | Phân xử conflicts, chốt quyết định |

### Step 1.3: PRD & Scope Control

**Khi nào dùng:** Sau discovery, cần kiểm soát scope chặt chẽ

```
Prompt: "Sử dụng skill prd-scope-control để tạo PRD cho [project]"
```

**Chống scope-creep bằng:**
- MoSCoW prioritization (Must/Should/Could/Won't)
- INVEST criteria cho user stories
- Change gates — mọi thay đổi phải qua approval

---

## Phase 2: Design & Architecture

> **Mục tiêu:** Thiết kế UI/UX, frontend, mobile trước khi code

### Step 2.1: Design Orchestration

**Khi nào dùng:** Thiết kế cần review đa góc nhìn

```
Prompt: "Sử dụng skill design-orchestration để thiết kế [feature]"
```

Gọi panel gồm Product, UX, Tech Lead cùng review bản thiết kế.

### Step 2.2: Frontend Design (UX Psychology)

**Khi nào dùng:** Thiết kế web UI/UX chuyên sâu

```
Prompt: "Sử dụng skill frontend-design cho [screen/component]"
```

**UX Principles:**
- Hick's Law — giảm số lựa chọn
- Fitts' Law — CTA lớn, dễ click
- Cognitive Load Theory — chunking information
- 4.5:1 contrast ratio, focus states

### Step 2.3: Mobile Design

**Khi nào dùng:** Thiết kế cho mobile app hoặc responsive mobile

```
Prompt: "Sử dụng skill mobile-design cho [feature]"
```

**Checklist:**
- Touch targets ≥ 44×44px
- Safe areas (notch, home indicator)
- Platform conventions (iOS vs Android)
- Gesture patterns

### Step 2.4: SEO Fundamentals

**Khi nào dùng:** Đảm bảo SEO baseline từ đầu

```
Prompt: "Sử dụng skill seo-fundamentals cho [page/site]"
```

---

## Phase 3: Implementation

> **Mục tiêu:** Code hiệu quả với AI agent orchestration

### Step 3.1: Intelligent Routing (Meta-Skill)

**Khi nào dùng:** LUÔN dùng — AI tự chọn expert agent phù hợp

```
Prompt: "Sử dụng skill intelligent-routing để xử lý [task]"
```

Đây là **meta-skill**: AI phân tích yêu cầu → tự gọi skill chuyên gia phù hợp nhất.

### Step 3.2: Subagent-Driven Development

**Khi nào dùng:** Task lớn cần chia nhỏ

```
Prompt: "Sử dụng skill subagent-driven-development để execute [plan]"
```

**Quy trình:**
1. Dispatch implementer subagent per task
2. Two-stage review: Spec compliance → Code quality
3. Fix issues trước khi qua task tiếp
4. Final review khi hoàn thành

### Step 3.3: Language-Specific Skills

| Ngôn ngữ | Skill | Highlights |
|-----------|-------|-----------|
| Python | `python-patterns` (441L) | Async, type hints, project structure |
| Node.js | `nodejs-best-practices` (333L) | Error handling, streaming, security |
| Next.js/React | `nextjs-react-expert` (267L) | App Router, Server Components, RSC |
| Tailwind CSS | `tailwind-patterns` (269L) | v4 Oxide, container queries, CSS-first |
| i18n | `i18n-localization` (154L) | Multi-language patterns |
| Game | `game-development` (167L) | Game loop, physics, rendering |

### Step 3.4: Clean Code

**Khi nào dùng:** Luôn áp dụng

```
Prompt: "Áp dụng skill clean-code standards"
```

**Quick Rules:**
- Function max 20 lines, max 3 arguments
- Names reveal intent, no magic numbers
- SRP, DRY, KISS, YAGNI

### Step 3.5: Advanced Agent Skills

| Skill | Khi nào dùng |
|-------|-------------|
| `behavioral-modes` | Chuyển đổi persona AI (creative/analytical/systematic) |
| `parallel-agents` | Chạy nhiều agents song song cho task độc lập |
| `mcp-builder` | Xây MCP server (Model Context Protocol) |

---

## Phase 4: Testing & Quality

> **Mục tiêu:** Đảm bảo chất lượng với TDD, E2E, debug có hệ thống

### Step 4.1: Test-Driven Development

**Khi nào dùng:** LUÔN dùng khi viết code mới

```
Prompt: "Sử dụng skill test-driven-development để implement [feature]"
```

**The Iron Law:**
```
NO PRODUCTION CODE WITHOUT A FAILING TEST FIRST
```

**Red-Green-Refactor:**
1. 🔴 RED — Write failing test
2. 🟢 GREEN — Write minimal code to pass
3. 🔄 REFACTOR — Clean up, all tests still pass

### Step 4.2: E2E Browser Testing (Playwright)

**Khi nào dùng:** Test web UI, flows, responsive

```
Prompt: "Sử dụng skill playwright-skill để test [page/flow]"
```

**Capabilities:**
- Multi-viewport testing (375px → 1440px)
- Page Object Model patterns
- Visual regression testing
- Login/auth flows, form submissions

### Step 4.3: Usability Testing

**Khi nào dùng:** Validate UX trước release

```
Prompt: "Sử dụng skill usability-testing cho [feature]"
```

Skill unique — không có đối thủ trong AGT-IDE. Bao gồm user testing protocols, task completion analysis, heuristic evaluation.

### Step 4.4: Systematic Debugging

**Khi nào dùng:** Có bugs cần fix

```
Prompt: "Sử dụng skill systematic-debugging cho issue: [mô tả bug]"
```

**The Iron Law:**
```
NO FIXES WITHOUT ROOT CAUSE INVESTIGATION FIRST
```

**4 Phases:** Root Cause Investigation → Pattern Analysis → Hypothesis Testing → Implementation (with test first)

> 💡 **Cho debug chuyên sâu:** Dùng `master-prompts/DEBUG-MASTER-v4.txt` — 9-bước protocol, auto-trigger khi 3 lần quick fix fail, escalation path lên architect.

---

## Phase 5: Deployment

> **Mục tiêu:** CI/CD pipeline automation

### Step 5.1: GitHub Actions CI/CD

**Khi nào dùng:** Setup automation pipeline

```
Prompt: "Sử dụng skill github-actions-templates để tạo CI/CD cho [project]"
```

**Reusable workflows cho:**
- Test on push/PR
- Build & deploy staging/production
- Security scanning
- Docker image building

---

## Phase 6: Operations & Monitoring

> **Mục tiêu:** Vận hành, giám sát, xử lý sự cố

### Step 6.1: Prometheus Metrics

```
Prompt: "Sử dụng skill prometheus-configuration để setup monitoring"
```

Scrape configs, recording rules, alert rules.

### Step 6.2: Grafana Dashboards

```
Prompt: "Sử dụng skill grafana-dashboards để tạo dashboard cho [service]"
```

RED method (Rate/Errors/Duration) cho services, USE method cho resources.

### Step 6.3: Loki Log Aggregation

```
Prompt: "Sử dụng skill loki-mode để setup log pipeline"
```

Skill dày nhất (721L) — LogQL queries, label strategies, retention policies.

### Step 6.4: Incident Handling

```
Prompt: "Sử dụng skill incident-runbook-templates cho incident [type]"
```

Detect → Acknowledge → Investigate → Mitigate → Post-mortem.

---

## Phase 7: Security

> **Mục tiêu:** Bảo mật từ scanning đến compliance

### Step 7.1: Vulnerability Scanning

```
Prompt: "Sử dụng skill vulnerability-scanner để audit [codebase]"
```

OWASP patterns, automated CVE checking, dependency scanning.

### Step 7.2: Privacy & Compliance

```
Prompt: "Sử dụng skill privacy-compliance cho [project]"
```

GDPR/CCPA checklist, data mapping, consent management, DPA requirements.

### Step 7.3: Red Team Testing

```
Prompt: "Sử dụng skill red-team-tactics để test [system]"
```

MITRE ATT&CK phases — Reconnaissance → Initial Access → Persistence → Exfiltration.

---

## Master Prompts

4 Master Prompts chuyên sâu trong `master-prompts/`. Khác với Skills (AI tự kích hoạt), Master Prompts cần **copy-paste** vào chat khi muốn deep-dive.

| Master Prompt | Dòng | Khi nào dùng |
|---|---|---|
| `VIBECODE-MASTER-v4.txt` | 707 | Build project từ A-Z (Vision→Blueprint→Coder Pack) |
| `DEBUG-MASTER-v4.txt` | 753 | Debug chuyên sâu — 9-step evidence-based protocol |
| `QA-MASTER-v4.txt` | 919 | QA tiered testing — templates cho 5 loại project |
| `XRAY-MASTER-v4.txt` | 752 | Handover/onboarding — X-ray toàn bộ codebase |

---

## Quick Reference

### 🎯 Skill Selection by Task

| Task | Primary Skill | Supporting |
|------|---------------|------------|
| Ý tưởng mới | `product-discovery` | `multi-agent-brainstorming` |
| Kiểm soát scope | `prd-scope-control` | — |
| Thiết kế UI/UX | `frontend-design` | `design-orchestration`, `mobile-design` |
| Write code | `test-driven-development` | `clean-code`, `intelligent-routing` |
| Python project | `python-patterns` | `clean-code` |
| Next.js project | `nextjs-react-expert` | `tailwind-patterns`, `seo-fundamentals` |
| Node.js backend | `nodejs-best-practices` | `clean-code` |
| Task lớn | `subagent-driven-development` | `parallel-agents` |
| Test UI | `playwright-skill` | `usability-testing` |
| Fix bugs | `systematic-debugging` | `DEBUG-MASTER-v4` |
| QA nghiệm thu | `test-driven-development` | `QA-MASTER-v4` |
| Deploy | `github-actions-templates` | — |
| Monitoring | `prometheus-configuration` | `grafana-dashboards`, `loki-mode` |
| Security audit | `vulnerability-scanner` | `red-team-tactics`, `privacy-compliance` |
| Handover project | — | `XRAY-MASTER-v4` |

### 📊 Full 31-Skill Inventory

| # | Skill | Nguồn | Dòng | SDLC Phase |
|---|-------|-------|------|------------|
| 1 | multi-agent-brainstorming | Custom | 256 | Discovery |
| 2 | product-discovery | Custom | 240 | Discovery |
| 3 | prd-scope-control | Custom | 297 | Discovery |
| 4 | design-orchestration | Custom | 167 | Design |
| 5 | frontend-design | AGT-IDE | 418 | Design |
| 6 | mobile-design | AGT-IDE | 394 | Design |
| 7 | seo-fundamentals | AGT-IDE | 129 | Design |
| 8 | intelligent-routing | AGT-IDE | 335 | Implementation |
| 9 | subagent-driven-development | Custom | 240 | Implementation |
| 10 | behavioral-modes | AGT-IDE | 242 | Implementation |
| 11 | parallel-agents | AGT-IDE | 175 | Implementation |
| 12 | clean-code | AGT-IDE | 201 | Implementation |
| 13 | python-patterns | AGT-IDE | 441 | Implementation |
| 14 | nodejs-best-practices | AGT-IDE | 333 | Implementation |
| 15 | nextjs-react-expert | AGT-IDE | 267 | Implementation |
| 16 | tailwind-patterns | AGT-IDE | 269 | Implementation |
| 17 | i18n-localization | AGT-IDE | 154 | Implementation |
| 18 | game-development | AGT-IDE | 167 | Implementation |
| 19 | mcp-builder | AGT-IDE | 176 | Implementation |
| 20 | test-driven-development | Custom | 371 | Testing |
| 21 | playwright-skill | Custom | 453 | Testing |
| 22 | usability-testing | Custom | 342 | Testing |
| 23 | systematic-debugging | Custom | 296 | Testing |
| 24 | github-actions-templates | Custom | 345 | Deployment |
| 25 | prometheus-configuration | Custom | 404 | Operations |
| 26 | grafana-dashboards | Custom | 381 | Operations |
| 27 | loki-mode | Custom | 721 | Operations |
| 28 | incident-runbook-templates | Custom | 395 | Operations |
| 29 | vulnerability-scanner | AGT-IDE | 276 | Security |
| 30 | privacy-compliance | Custom | 295 | Security |
| 31 | red-team-tactics | AGT-IDE | 199 | Security |

### 🚫 Anti-Patterns

| ❌ Don't | ✅ Do |
|----------|-------|
| Code trước, design sau | Discovery → Design → Code |
| Skip tests | TDD always |
| Fix bugs bằng đoán mò | Systematic debugging (evidence-based) |
| Deploy Friday chiều | Deploy Mon-Thu sáng |
| Skip staging | Test staging trước production |
| Dùng 1 agent cho mọi thứ | Intelligent routing → agent chuyên gia |

---

*THB Skill Stack v1 — Curated 2026-02-08 | 31 Skills + 4 Master Prompts*
