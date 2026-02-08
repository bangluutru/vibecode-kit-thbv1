---
name: prd-scope-control
description: PRD templates and scope management for preventing scope creep. Use when defining product requirements or managing feature boundaries.
version: 1.0.0
source: custom
rating: 10/10
---

# PRD & Scope Control

Product Requirements Document templates and scope management practices.

## When to Use This Skill

- Starting a new feature or product
- Defining requirements for engineering
- Managing scope creep during development
- Communicating with stakeholders
- Pre-development scope freeze

---

## PRD Template

```markdown
# Product Requirements Document: [Feature Name]

**Author**: [Name]
**Last Updated**: [Date]
**Status**: [Draft | Review | Approved | Frozen]
**Version**: [1.0]

---

## 1. Overview

### 1.1 Problem Statement
[What problem are we solving? For whom?]

### 1.2 Goals
| Goal | Metric | Target |
|------|--------|--------|
| [Goal 1] | [Metric] | [Target] |
| [Goal 2] | [Metric] | [Target] |

### 1.3 Non-Goals (Explicit Exclusions)
- ❌ [What we are NOT doing]
- ❌ [What is out of scope for THIS release]
- ❌ [What might seem related but isn't included]

### 1.4 Success Metrics
| Metric | Current | Target | Measurement |
|--------|---------|--------|-------------|
| [Metric 1] | [X] | [Y] | [How measured] |

---

## 2. User Stories

### Primary User: [Persona Name]

| Priority | User Story | Acceptance Criteria |
|----------|------------|---------------------|
| P0 | As a [user], I want to [action] so that [benefit] | Given [context], when [action], then [result] |
| P0 | ... | ... |
| P1 | ... | ... |
| P2 | ... | ... |

### Priority Definitions
- **P0**: Must have for launch (blocking)
- **P1**: Should have for launch (important)
- **P2**: Nice to have (future iteration)
- **P3**: Future consideration

---

## 3. Functional Requirements

### 3.1 [Feature Area 1]

| ID | Requirement | Priority | Notes |
|----|-------------|----------|-------|
| FR-001 | [Requirement description] | P0 | |
| FR-002 | [Requirement description] | P1 | |

### 3.2 [Feature Area 2]
...

---

## 4. Non-Functional Requirements

| Category | Requirement | Target |
|----------|-------------|--------|
| Performance | Page load time | < 2 seconds |
| Scalability | Concurrent users | 10,000 |
| Availability | Uptime | 99.9% |
| Security | Authentication | OAuth 2.0 |
| Accessibility | WCAG compliance | Level AA |

---

## 5. Constraints & Assumptions

### Constraints
- [Technical constraint 1]
- [Business constraint 1]
- [Timeline constraint 1]

### Assumptions
| Assumption | Risk if Wrong | Mitigation |
|------------|---------------|------------|
| [Assumption 1] | [Impact] | [Plan B] |

---

## 6. Dependencies

| Dependency | Owner | Status | Blocker Risk |
|------------|-------|--------|--------------|
| [External API] | [Team] | [Status] | High/Med/Low |
| [Design] | [Design Team] | [Status] | ... |

---

## 7. Timeline

| Milestone | Date | Deliverable |
|-----------|------|-------------|
| Scope Freeze | [Date] | PRD approved |
| Design Complete | [Date] | UI/UX finalized |
| Dev Complete | [Date] | Feature coded |
| QA Complete | [Date] | Testing done |
| Launch | [Date] | Production release |

---

## 8. Approval

| Role | Name | Approval | Date |
|------|------|----------|------|
| Product | | ☐ | |
| Engineering | | ☐ | |
| Design | | ☐ | |
| Stakeholder | | ☐ | |

---

## Appendix

### A. Wireframes/Mockups
[Link to designs]

### B. Technical Specifications
[Link to tech spec]

### C. Open Questions
| Question | Owner | Due Date | Resolution |
|----------|-------|----------|------------|
| [Question] | [Name] | [Date] | [Answer] |
```

---

## Scope Freeze Process

### 1. Scope Freeze Checklist

```markdown
## Pre-Freeze Verification

### Requirements Complete
- [ ] All P0 user stories defined
- [ ] Acceptance criteria for each story
- [ ] Non-functional requirements specified
- [ ] Dependencies identified

### Stakeholder Alignment
- [ ] Product approval obtained
- [ ] Engineering feasibility confirmed
- [ ] Design approved
- [ ] Timeline agreed

### Risk Assessment
- [ ] Assumptions documented
- [ ] Risks identified and mitigation planned
- [ ] Blockers escalated

### SCOPE IS NOW FROZEN ❄️
Date: [DATE]
Approver: [NAME]
```

### 2. Change Request Process

```markdown
## Scope Change Request

**Requestor**: [Name]
**Date**: [Date]
**Feature Affected**: [Feature Name]

### Change Description
[What change is being requested?]

### Justification
[Why is this change necessary?]

### Impact Analysis

| Aspect | Impact | Details |
|--------|--------|---------|
| Timeline | +[X] days | [Explanation] |
| Resources | +[X] people | [Explanation] |
| Dependencies | [Yes/No] | [What changes] |
| Risk | [Low/Med/High] | [New risks] |

### Trade-off Options
1. Add this, remove [X]
2. Add this, delay launch by [Y]
3. Add this in next iteration

### Decision

| Role | Decision | Date |
|------|----------|------|
| Product | Accept/Reject/Defer | |
| Engineering | Accept/Reject/Defer | |

**Final Decision**: [Accept/Reject/Defer]
**Rationale**: [Why]
```

---

## Scope Creep Warning Signs

| 🚩 Warning Sign | Response |
|-----------------|----------|
| "Can we just add..." | Evaluate against PRD, require change request |
| "It's a small change" | Small changes add up, still require process |
| "The customer asked for..." | Is it P0? Add to backlog if not |
| "Tech debt" during feature work | Separate ticket, different scope |
| "While we're at it..." | Classic scope creep, enforce boundaries |

---

## Scope Control Rules

### Iron Triangle

```
        SCOPE
         /\
        /  \
       /    \
    TIME ---- COST
```

**Rule**: If one changes, at least one other must change.

| Change | Impact |
|--------|--------|
| Add scope | Extend time OR add resources |
| Reduce time | Cut scope OR add resources |
| Reduce cost | Cut scope OR extend time |

### MoSCoW Prioritization

| Category | Definition | Rule |
|----------|------------|------|
| **Must** | Critical for launch | Cannot ship without |
| **Should** | Important but not critical | Ship without if needed |
| **Could** | Nice to have | Include if time permits |
| **Won't** | Explicitly out of scope | Not in this release |

---

## Anti-Patterns

| ❌ Don't | ✅ Do |
|---------|-------|
| Verbal requirements | Document everything |
| "We'll figure it out" | Define upfront |
| Unlimited revisions | Version-controlled PRD |
| No sign-off | Formal approval process |
| Scope changes without impact analysis | Change request required |

---

## Human Intervention Required

🔴 **STOP AND ASK USER** before:
- Freezing scope (final approval needed)
- Accepting scope change requests
- Removing P0 requirements
- Changing timeline commitments
