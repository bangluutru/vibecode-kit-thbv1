---
name: usability-testing
description: Usability testing protocols with user recruitment, test scripts, and analysis templates. Use when validating UX design with real users.
version: 1.0.0
source: custom
rating: 10/10
---

# Usability Testing Framework

Comprehensive usability testing from recruitment to analysis.

## When to Use This Skill

- Validating new designs before development
- Testing major UX changes
- Identifying usability issues
- Gathering user feedback
- Iterating on prototypes

---

## Core Principle

> **Test with 5 users to find 85% of usability problems.**

Nielsen Norman Group research shows diminishing returns after 5 users per round.

```
Testing Approach:
1. Test early (with prototypes)
2. Test often (each iteration)
3. Test with real users (not colleagues)
4. Fix issues and test again
```

---

## Test Planning

### Study Plan Template

```markdown
## Usability Study Plan: [Feature/Product Name]

### Overview
- **Objective**: [What are we trying to learn?]
- **Test Date**: [Date]
- **Duration**: [X minutes per session]
- **Sessions**: [N users]

### Research Questions
1. Can users complete [primary task]?
2. How long does it take to [action]?
3. What confuses users about [feature]?
4. How do users expect [flow] to work?

### Participants

| Criteria | Requirement |
|----------|-------------|
| Target user type | [Persona] |
| Experience level | [Novice/Intermediate/Expert] |
| Age range | [Range] |
| Recruiting source | [Where to find them] |
| Compensation | [$X gift card] |

### Tasks to Test

| # | Task | Success Criteria | Target Time |
|---|------|-----------------|-------------|
| 1 | [Task description] | [What = success] | [X min] |
| 2 | [Task description] | [What = success] | [X min] |
| 3 | [Task description] | [What = success] | [X min] |

### Metrics to Collect
- Task success rate
- Time on task
- Error count
- User satisfaction (SUS/NPS)
- Qualitative observations
```

---

## Participant Recruitment

### Screener Template

```markdown
## Recruiting Screener: [Study Name]

Thank you for your interest! Please answer a few questions to see if you qualify.

**Q1. How often do you [relevant behavior]?**
- Daily (QUALIFY)
- Weekly (QUALIFY)
- Monthly (MAYBE)
- Rarely/Never (DISQUALIFY)

**Q2. Which best describes your experience with [product category]?**
- I use it for work (QUALIFY)
- I use it personally (QUALIFY)
- I've never used this type of product (DISQUALIFY)

**Q3. What is your role?**
- [Target role 1] (QUALIFY)
- [Target role 2] (QUALIFY)
- [Non-target role] (DISQUALIFY)

**Q4. Are you or immediate family employed in:**
- UX Design (DISQUALIFY)
- Market Research (DISQUALIFY)
- Our company (DISQUALIFY)
- None of the above (QUALIFY)

**Q5. Availability**
- [Date options for scheduling]
```

### Recruitment Channels

| Channel | Cost | Quality | Speed |
|---------|------|---------|-------|
| UserTesting.com | $$$ | High | Fast |
| Respondent.io | $$ | High | Medium |
| Social media | $ | Variable | Slow |
| Existing users | Free | High | Medium |
| Craigslist/local | $ | Variable | Medium |

---

## Test Session

### Facilitator Script

```markdown
## Test Script: [Study Name]

### Introduction (5 min)

"Hi [Name], thank you for participating today. My name is [Facilitator].

Before we begin, I want to explain what we'll be doing:
- We're testing a [product/prototype], not testing you
- There are no right or wrong answers
- If something is confusing, that's valuable feedback for us
- Please think out loud - tell me what you're looking at, what you're thinking, what you expect to happen
- I might not answer questions during tasks - I want to see how you'd figure things out naturally
- We'll record this session for our team to review. Is that okay?

Do you have any questions before we start?"

### Warm-up Questions (3 min)

"Before we look at the prototype, I'd like to learn a bit about you:
1. Tell me about your experience with [relevant topic]..."
2. "How do you currently [solve the problem we're addressing]?"

### Task Instructions

**Task 1: [Task Name]**

"Imagine you want to [scenario]. Using this [prototype/product], please try to [complete specific goal].

Remember to think out loud as you go."

*Observe silently. Note:*
- Path taken
- Hesitations
- Clicks/taps
- Verbal comments
- Facial expressions

*If stuck for > 30 seconds, prompt:*
"What are you thinking right now?"
"What do you expect to happen?"

*After task:*
"On a scale of 1-7, how difficult was that task? (1=very easy, 7=very difficult)"

[Repeat for each task]

### Post-Task Questions (5 min)

"Now I have a few follow-up questions:
1. What was the most confusing part of what you just did?"
2. "What would you change about this?"
3. "How does this compare to [competitor/current method]?"
4. "Is there anything else you'd like to share?"

### Closing (2 min)

"Thank you so much for your time and feedback. It's incredibly valuable.

Here is your [compensation]. Do you have any final questions for me?"
```

### Observation Template

```markdown
## Session Notes: Participant [#]

**Date**: [Date]
**Duration**: [X min]
**Facilitator**: [Name]

### Task 1: [Task Name]

| Metric | Result |
|--------|--------|
| Success | ☐ Yes ☐ No ☐ Partial |
| Time | [X:XX] |
| Difficulty (1-7) | [X] |
| Errors | [Count] |

**Path taken**:
[Step 1] → [Step 2] → [Step 3]

**Observations**:
- [What happened, what they said]

**Quotes**:
> "[Direct quote from user]"

### Overall Impressions
- [Summary of session]
- [Key issues observed]
- [Positive feedback]
```

---

## Analysis & Reporting

### Issue Severity Matrix

| Severity | Definition | Action |
|----------|------------|--------|
| **Critical** | Prevents task completion | Must fix before launch |
| **Major** | Significant difficulty, workarounds exist | Should fix before launch |
| **Minor** | Small inconvenience | Fix if time permits |
| **Enhancement** | Not a problem, improvement idea | Add to backlog |

### Findings Report Template

```markdown
## Usability Test Report: [Study Name]

**Date**: [Date]
**Participants**: [N]
**Focus**: [What was tested]

---

### Executive Summary

[2-3 sentence summary of key findings]

**Overall Success Rate**: [X]%
**Average SUS Score**: [X]/100

---

### Key Findings

#### Finding 1: [Issue Title]
- **Severity**: Critical / Major / Minor
- **Affected Users**: [N] of [Total]
- **Description**: [What happened]
- **Evidence**: 
  > "[User quote]"
  > [Screenshot/video timestamp]
- **Recommendation**: [How to fix]

#### Finding 2: [Issue Title]
...

---

### Task Results

| Task | Success Rate | Avg Time | Difficulty (1-7) |
|------|-------------|----------|------------------|
| [Task 1] | [X]% | [X:XX] | [X.X] |
| [Task 2] | [X]% | [X:XX] | [X.X] |
| [Task 3] | [X]% | [X:XX] | [X.X] |

---

### Recommendations Priority

| Priority | Issue | Recommendation | Effort |
|----------|-------|----------------|--------|
| P0 | [Issue] | [Fix] | [Effort] |
| P1 | [Issue] | [Fix] | [Effort] |
| P2 | [Issue] | [Fix] | [Effort] |

---

### Next Steps
1. Address P0 issues before launch
2. Schedule follow-up testing after fixes
3. [Other action items]

---

### Appendix
- Full session recordings: [Link]
- Raw notes: [Link]
- Participant demographics: [Summary]
```

---

## Quick Tests

### 5-Second Test
Show design for 5 seconds, ask:
- "What is this page about?"
- "What can you do here?"
- "What do you remember?"

### First-Click Test
Give task, measure:
- Where user clicks first
- Success rate of first click predicting task success

### A/B Preference Test
Show two designs, ask:
- "Which do you prefer?"
- "Why?"

---

## Human Intervention Required

🔴 **STOP AND ASK USER** before:
- Finalizing test plan and recruiting
- Compensating participants
- Sharing recordings externally
- Making design decisions based on findings
