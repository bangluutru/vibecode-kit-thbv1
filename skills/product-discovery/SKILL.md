---
name: product-discovery
description: Product discovery and validation framework. Use when validating product ideas, conducting customer research, or determining product-market fit.
version: 1.0.0
source: custom
rating: 10/10
---

# Product Discovery Framework

Systematic approach to validate product ideas before building.

## When to Use This Skill

- Starting a new product or feature
- Validating assumptions about user needs
- Before committing engineering resources
- Pivoting or changing direction
- Prioritizing roadmap items

---

## Core Principle

> **Build the RIGHT thing before building the thing RIGHT.**

```
❌ Build → Hope users come
✅ Validate → Then build → Measure → Learn
```

---

## Discovery Process

### Phase 1: Problem Definition

```markdown
## Problem Statement Template

### 1. Who has the problem?
- Primary user: [Specific persona]
- Size of market: [Number of potential users]
- User segments: [List of distinct groups]

### 2. What is the problem?
- Current behavior: [How users solve it now]
- Pain points: [Specific frustrations]
- Frequency: [How often does this occur]
- Severity: [1-10, impact on user's life/work]

### 3. Why is this problem worth solving?
- Alternative solutions: [What exists today]
- Why alternatives fail: [Gaps in current solutions]
- Willingness to pay: [Evidence of budget]
```

### Phase 2: Customer Interviews

#### Interview Script Template

```markdown
## Customer Interview Guide

### Opener (2 min)
"Thanks for taking the time. I'm trying to understand how you [do X].
There are no right or wrong answers - I just want to learn from your experience."

### Context Questions (5 min)
1. "Tell me about the last time you [did X]..."
2. "Walk me through your typical process for [Y]..."
3. "How often do you need to [do Z]?"

### Problem Questions (10 min)
4. "What's the hardest part about [doing X]?"
5. "Why is that hard?" (Ask 5 times - 5 Whys)
6. "What happens if you don't solve this?"
7. "How much time/money does this cost you?"

### Current Solution Questions (5 min)
8. "What do you currently use to solve this?"
9. "What do you like about your current solution?"
10. "What do you wish it did better?"

### Validation Questions (5 min)
11. "If you could wave a magic wand, what would the ideal solution look like?"
12. "What would you pay for that solution?"
13. "Who else should I talk to about this?"

### Closing (3 min)
"This was incredibly helpful. Can I follow up if I have more questions?"
```

#### Interview Analysis Template

| Interview # | Pain Point | Severity (1-10) | Current Solution | Willing to Pay? |
|-------------|-----------|-----------------|------------------|-----------------|
| 1 | | | | |
| 2 | | | | |
| 3 | | | | |

**Pattern Observed**: [What themes emerge?]

---

### Phase 3: Validation Experiments

#### Experiment Types

| Type | Effort | Evidence Strength | When to Use |
|------|--------|-------------------|-------------|
| **Landing Page** | Low | Medium | Test interest |
| **Fake Door** | Low | High | Test intent |
| **Concierge** | Medium | High | Test value prop |
| **Wizard of Oz** | Medium | High | Test UX |
| **MVP** | High | Highest | Test full flow |

#### Landing Page Test

```markdown
## Landing Page Experiment

**Hypothesis**: [X% of visitors will sign up for waitlist]

**Setup**:
1. Create landing page with value proposition
2. Add email signup form
3. Drive traffic via [channel]
4. Track conversion rate

**Success Criteria**:
- Conversion rate > 5% = Strong signal
- Conversion rate 2-5% = Moderate signal
- Conversion rate < 2% = Weak signal

**Results**:
- Visitors: [N]
- Signups: [N]
- Conversion: [X%]

**Conclusion**: [Continue/Pivot/Kill]
```

#### Willingness to Pay Test

```markdown
## Pricing Validation

### Van Westendorp Questions
1. At what price would you consider this too expensive?
2. At what price would you consider this too cheap (suspect quality)?
3. At what price would you consider this expensive but worth it?
4. At what price would you consider this a bargain?

### Results Analysis
| Question | Avg Price | Range |
|----------|-----------|-------|
| Too expensive | $ | $-$ |
| Too cheap | $ | $-$ |
| Expensive but worth it | $ | $-$ |
| Bargain | $ | $-$ |

**Optimal Price Point**: $[X] - $[Y]
```

---

### Phase 4: Go/No-Go Decision

#### Decision Framework

```markdown
## Product Discovery Scorecard

| Criteria | Score (1-5) | Evidence |
|----------|-------------|----------|
| **Problem Severity** | | |
| Clear, frequent pain point | | [Evidence] |
| **Market Size** | | |
| Large enough to matter | | [Evidence] |
| **Willingness to Pay** | | |
| Users will spend money | | [Evidence] |
| **Competition** | | |
| Room for differentiation | | [Evidence] |
| **Feasibility** | | |
| Can we build this? | | [Evidence] |

**Total Score**: [X]/25

### Decision Thresholds
- 20-25: 🟢 Strong GO - Proceed to build
- 15-19: 🟡 Conditional GO - Address gaps first
- 10-14: 🔴 PAUSE - More validation needed
- <10: ⛔ NO-GO - Kill or major pivot
```

---

## Assumption Mapping

```markdown
## Assumption Tracker

| Assumption | Risk Level | Evidence Needed | Status |
|------------|-----------|-----------------|--------|
| Users need X | High | 10+ interviews | ✅ Validated |
| Users will pay $Y | High | Purchase intent | 🟡 Testing |
| We can build in Z weeks | Medium | Technical spike | ❌ Unknown |
| Market size is N | Low | Secondary research | ✅ Validated |

### Riskiest Assumptions First
1. [Most critical unvalidated assumption]
2. [Second most critical]
3. [Third most critical]

**This week's focus**: Validate assumption #1
```

---

## Anti-Patterns to Avoid

| ❌ Don't | ✅ Do |
|---------|-------|
| Build and pray | Validate then build |
| Survey 1000 people | Interview 10 deeply |
| Ask "would you use this?" | Ask "tell me about last time..." |
| Lead the witness | Listen more than talk |
| Skip to solution | Understand problem first |
| Trust opinions | Trust behaviors |

---

## Human Intervention Required

🔴 **STOP AND ASK USER** before:
- Concluding discovery phase (GO/NO-GO decision)
- Spending money on validation experiments
- Making pivot recommendations
- Defining target market or pricing
