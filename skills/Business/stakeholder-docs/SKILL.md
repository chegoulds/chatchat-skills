---
category: Business
id: stakeholder-docs
name: Stakeholder Docs
description: Execute a sprint per agile principles. Invoke with a sprint plan folder path (e.g. docs/project/sprints/Sprint-01). Uses delivery-orchestrator to enforce agent order, governance, TDD, and mandatory Sprint Package. Delivers UAT checklist for stakeholder manual testing, Sprint Review, Audit, and release per release-playbook.
---

# Sprint Execution

**Purpose**: Run sprint **execution** (not planning) for an existing sprint. You **invoke this skill and point to a sprint plan folder**; the **delivery-orchestrator** runs the sequence, enforces governance, and produces the full Sprint Package including UAT, Sprint Review, Audit, and release deliverables.

**Planning vs execution**: Planning is done by **sprint-planning-facilitator** (creates/updates sprint-plan.md, scope.md, tasks-by-agent.md, etc.). This skill **executes** that plan: who does what in order, what cannot be skipped, when escalation happens, and what artifacts are mandatory.

---

## 1. How to Invoke

**User says** (examples):

- *"Run sprint execution for docs/project/sprints/Sprint-01"*
- *"Execute the sprint in docs/project/sprints/Sprint-02"*
- *"Use sprint-execution skill on Sprint-03"*

**Input**: Path to the sprint folder (e.g. `docs/project/sprints/Sprint-01`).

**Process**:

1. Resolve the sprint folder; confirm it contains at least sprint-plan.md, scope.md, tasks-by-agent.md, deliverables.md, references.md (from planning).
2. Invoke **delivery-orchestrator** (or act as orchestrator) and run the **Execution Sequence** (§2).
3. Enforce **Governance Rules** (§3); block any violation.
4. Produce **Mandatory Sprint Package** (§4) in the same sprint folder.
5. Ensure **End-of-Sprint** flow: UAT checklist, Sprint Review, Audit, and Release readiness (§5).

---

## 2. Execution Sequence (Strict Order)

Follow [delivery-orchestrator](../../../.cursor/agents/delivery-orchestrator.md). Delegate to agents in this order; collect outputs into the sprint folder.

| Step | Agent(s) | Output |
|------|----------|--------|
| 1 | pm | Sprint goal confirmed; scope and timeline for execution |
| 2 | po | Scope validated against roadmap; backlog items confirmed |
| 3 | business | Refined stories; BRD/epic traceability |
| 4 | architect | Validation against approved architecture; no redesign without approval |
| 5 | design-authority | Design scope and component specs (if UI in scope) |
| 6 | **qa-lead** | **Test strategy and test cases first** (TDD); TC-ids, traceability |
| 7 | backend-squad, frontend-squad, dev-mid, dev-junior | Unit test matrix from QA cases; then implementation plan and tasks |
| 8 | devops | CI/CD impact; deployment plan; branch/tag strategy |
| 9 | development-sm | Definition of Done validated; mandatory artifacts check |
| 10 | delivery-orchestrator | Sprint Package compiled; UAT, Review, Audit, Release readiness |

**TDD enforcement**: Step 6 (QA) must complete before Step 7 (Dev). Devs produce unit test matrix from QA test cases, then implementation plan.

---

## 3. Governance Rules (No Exceptions)

- **No dev output without QA test strategy first.** QA defines test cases; dev converts to unit tests, then implements.
- **No architecture redesign** unless Architect + Design Authority approve.
- **No DB change** without migration plan.
- **No deployment** without DevOps approval.
- **No story complete** without QA sign-off.

Escalation: Dev→Architect; Architect→Design Authority; Business impact→PO+Business. Document in risk-log.md or escalation log.

---

## 4. Mandatory Sprint Package (Sprint Folder Contents)

By end of execution, the sprint folder must contain:

| Artifact | File | Purpose |
|----------|------|---------|
| Sprint goal | sprint-plan.md | Already from planning; confirm/update if needed |
| Refined user stories | scope.md | Epics, US-XXX, TS-XXX, dependencies |
| Technical breakdown | tasks-by-agent.md, deliverables.md | Per-agent tasks; code/tests/docs to produce |
| **Test matrix** | **test-matrix.md** | Test cases, TC-ids, traceability to US/TS |
| **CI/CD impact** | **cicd-impact.md** | Pipeline changes, branch/tag, gates |
| **Deployment plan** | **deployment-plan.md** | Steps, environment, rollback |
| **Risk log** | **risk-log.md** | Risks, blockers, mitigations |
| **Carryover items** | **carryover.md** | Incomplete or deferred items |
| **UAT checklist** | **uat-checklist.md** | Scenarios for **stakeholder manual testing** as regular user |
| **Sprint review** | **sprint-review.md** | What was delivered; demo; stakeholder feedback |
| **Audit** | **audit.md** | Process compliance; traceability; release readiness |
| Release | docs/project/releases.md | Per release-playbook: tag, log entry (when release is cut) |

Create any missing files from the template in `docs/project/sprints/_template/`. No freestyle responses; all structured.

---

## 5. End-of-Sprint: UAT → Sprint Review → Audit → Release

### 5.1 UAT (Stakeholder)

- **Owner**: Main stakeholder (you) does **manual testing as a regular user**.
- **Input**: uat-checklist.md with scenarios derived from user stories and acceptance criteria.
- **Rule**: Release should not be signed off until UAT is done (or explicitly deferred by PM). QA Lead and PM prepare the checklist; stakeholder executes and signs off.

### 5.2 Sprint Review

- **Artifact**: sprint-review.md in the sprint folder.
- **Content**: What was delivered; demo notes; stakeholder feedback; any “done” vs “not done” list.
- **Owner**: SM + PM.

### 5.3 Audit

- **Artifact**: audit.md in the sprint folder.
- **Content**: Compliance with process (traceability, DoD, governance); release readiness; link to release log when released.
- **Owner**: Orchestrator / SM.

### 5.4 Release (Per Release Playbook)

- **Process**: [docs/development/release-playbook.md](../../../docs/development/release-playbook.md)
- **Pre-release gate**: PM, SM, QA Lead, Dev-Senior agree.
- **Steps**: Freeze, checklist, version, tag on main, record in docs/project/releases.md, unfreeze.
- **Deliverable**: Release tag (e.g. v1.1.0) and one row in the release log.

---

## 6. Responsibility Boundaries

Use [docs/development/responsibility-boundaries.md](../../../docs/development/responsibility-boundaries.md). Orchestrator must prevent agents from overstepping (e.g. Architect does not implement; Dev does not change architecture without approval).

---

## 7. Invocation Summary

1. **User**: *"Run sprint execution for docs/project/sprints/Sprint-01"*
2. **You**: Load sprint folder; run delivery-orchestrator sequence (Steps 1–10); enforce governance; create/update test-matrix.md, cicd-impact.md, deployment-plan.md, risk-log.md, carryover.md, uat-checklist.md, sprint-review.md, audit.md; ensure UAT, Sprint Review, Audit, and Release deliverables are ready.
3. **Output**: Sprint folder contains full Sprint Package; stakeholder has uat-checklist.md for manual testing; sprint-review.md and audit.md ready; release path documented per release-playbook.
