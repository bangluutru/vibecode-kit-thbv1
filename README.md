# 🎯 THB Skill Stack v1

> **Vibecode Kit — Curated for THB** | 31 Skills + 4 Master Prompts + 23 Agents + 20 Workflows

---

## Cấu trúc

```
vibecode-kit-thbv1/
├── skills/           # 31 curated skills (15 Custom + 16 AGT-IDE)
├── agents/           # 23 agent personas (orchestrator, frontend, backend, debug...)
├── workflows/        # 20 workflow definitions (orchestrate, deploy, test, plan...)
├── rules/            # 11 rule files (GEMINI core constitution, security, compliance...)
├── core/             # Personality + archetypes
├── shared/           # 17 shared template libraries (design-system, security, testing...)
├── master-prompts/   # 4 deep-dive Master Prompts (copy-paste vào chat)
├── instruction.md    # Hướng dẫn SDLC — skill nào dùng ở phase nào
├── AGENT_FLOW.md     # Flow diagram AI agent end-to-end
├── ARCHITECTURE.md   # Kiến trúc hệ thống (EN)
└── ARCHITECTURE.vi.md # Kiến trúc hệ thống (VI)
```

## Nguồn gốc

Được chọn lọc từ ~90 skills qua 6 nguồn:
- **antigravity-awesome-skills** → 15 skills (Custom, mạnh Operations/Monitoring)
- **antigravity-ide** → 16 skills + agents + workflows + rules + shared (mạnh Frontend/Mobile/Lang-specific)
- **vibecode-kit-nclv4** → 4 Master Prompts (DEBUG 753L, QA 919L, VIBECODE 707L, XRAY 752L)

## Cách dùng

```bash
# Copy skills vào project
cp -r vibecode-kit-thbv1/skills/* your-project/.agent/skills/

# Copy agents
cp -r vibecode-kit-thbv1/agents/* your-project/.agent/agents/

# Copy workflows
cp -r vibecode-kit-thbv1/workflows/* your-project/.agent/workflows/

# Copy rules
cp -r vibecode-kit-thbv1/rules/* your-project/.agent/rules/
```

Xem `instruction.md` để biết skill nào dùng ở SDLC phase nào.

---

*Created: 2026-02-08 | THB Skill Stack v1*
