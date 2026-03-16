---
id: agile-product-owner
name: Agile Product Owner
description: Agile product ownership toolkit for Senior Product Owner including INVEST-compliant user story generation, sprint planning, backlog management, and velocity tracking. Use for story writing, sprint planning, stakeholder communication, and agile ceremonies.
category: Business
requires: []
examples:
  - "Break this epic into INVEST compliant user stories with acceptance criteria."
  - "Help me build a sprint-ready backlog from these requirements."
---

# Agile Product Owner

Use this skill as a guidance-only framework for product ownership work: refining scope, creating user stories, prioritizing backlog, and preparing sprint-ready plans.

## Use this skill when

- You need to convert ideas or epics into implementation-ready user stories.
- You need to prioritize backlog items with clear rationale.
- You need to prepare sprint scope based on team capacity.
- You need structured stakeholder-ready product update output.

## Do not use this skill when

- The task is purely technical implementation with no product planning component.
- The user expects direct tool execution from this skill.

## Guardrails

- Treat this as guidance-only; do not claim to run planning scripts or external tools.
- Ask clarifying questions when goals, constraints, or dependencies are unclear.
- Keep recommendations tied to user outcomes, delivery risk, and measurable value.

## Product ownership workflow

1. **Clarify problem and outcome**
   - Identify user segment, pain point, and business objective.
   - Define success metrics and constraints (timeline, compliance, dependencies).

2. **Decompose into stories**
   - Break epics into small, testable user stories.
   - Apply INVEST checks (independent, negotiable, valuable, estimable, small, testable).

3. **Define acceptance criteria**
   - Use clear pass/fail behavior.
   - Include edge cases and non-functional expectations where relevant.

4. **Prioritize backlog**
   - Rank by impact, urgency, risk reduction, and dependency order.
   - Mark must-have vs should-have vs could-have.

5. **Sprint readiness**
   - Compare candidate scope against available capacity.
   - Surface blockers, open questions, and sequencing risks.

6. **Stakeholder communication**
   - Summarize what is planned now, later, and why.
   - Make trade-offs explicit.

## Output format

```markdown
## Product Goal
- User problem:
- Business outcome:
- Success metric:

## Story Backlog
### Story 1
- As a:
- I want:
- So that:
- Priority:
- Estimate:
- Dependencies:

#### Acceptance Criteria
- [ ] Criterion 1
- [ ] Criterion 2

### Story 2
- As a:
- I want:
- So that:
- Priority:
- Estimate:
- Dependencies:

#### Acceptance Criteria
- [ ] Criterion 1
- [ ] Criterion 2

## Sprint Proposal
- Capacity assumption:
- Proposed stories:
- Risks/blockers:
- Out of scope:

## Stakeholder Update
- Planned this sprint:
- Deferred:
- Reasoning:
- Decisions needed:
```
