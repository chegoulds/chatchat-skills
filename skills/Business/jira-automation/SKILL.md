---
id: jira-automation
name: Jira Automation
description: Guidance-only playbook for Jira operating workflows, including intake, backlog hygiene, sprint governance, and delivery reporting.
category: Business
requires: []
examples:
  - "Help me set up Jira intake and prioritization workflow."
  - "Use jira-automation to improve sprint execution and reporting."
---

# Jira Workflow Operations

Use this guidance-only skill to design Jira processes and governance. It does not execute commands or use external tooling directly.

## When to use

- You need a consistent issue intake and prioritization model.
- You want better sprint planning and predictable delivery.
- You need visibility across roadmap, blockers, and throughput.

## Core workflows

### 1) Intake and prioritization

- Standardize ticket templates for bugs, tasks, incidents, and requests.
- Define mandatory fields (business impact, owner, due date, dependencies).
- Apply a scoring model for priority (urgency, impact, effort, risk).

### 2) Backlog hygiene

- Run weekly backlog grooming to remove duplicates and stale work.
- Keep acceptance criteria clear and testable for ready-to-start issues.
- Limit "unassigned" and "unknown priority" items to reduce planning drag.

### 3) Sprint governance

- Set sprint capacity with planned vs. unplanned work targets.
- Track carry-over rate and root causes (scope churn, blockers, estimate quality).
- Define a blocked-work escalation path with response SLAs.

### 4) Reporting and accountability

- Track cycle time, lead time, throughput, and defect escape rate.
- Publish weekly project health updates: status, risks, decisions, owners.
- Use post-sprint retros to convert recurring issues into process actions.

## Risk controls

- Avoid oversized tickets that hide delivery risk.
- Enforce change-control rules after sprint lock when possible.
- Require owner and next action for all blocked items.

## Output format

When asked for help, provide:

- A Jira workflow and status policy.
- A backlog audit checklist.
- A sprint KPI dashboard outline with action thresholds.
