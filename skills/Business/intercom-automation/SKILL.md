---
id: intercom-automation
name: Intercom Automation
description: Guidance-only playbook for Intercom support operations, including inbox triage, assignment rules, SLA management, and quality reviews.
category: Business
requires: []
examples:
  - "Help me design Intercom triage and escalation workflows."
  - "Use intercom-automation to improve response quality and SLA performance."
---

# Intercom Support Operations

Use this guidance-only skill to design and improve Intercom workflows. It does not execute commands or call external tools.

## When to use

- You need repeatable triage and routing rules for support conversations.
- You want stronger SLA adherence and escalation policies.
- You need a quality framework for support replies and follow-ups.

## Core workflows

### 1) Inbox triage

- Define severity levels (P1-P4) with target first-response and resolution times.
- Route by issue type (billing, product bug, onboarding, access).
- Add explicit ownership rules for handoffs between support and engineering.

### 2) Assignment and escalation

- Set assignment rules by team, timezone, and expertise.
- Escalate high-risk conversations quickly (revenue impact, outages, legal requests).
- Require escalation notes with context, customer history, and expected next step.

### 3) Reply quality controls

- Use consistent tone and structure (acknowledge, diagnose, next action, ETA).
- Add a pre-send checklist for accuracy, policy alignment, and customer clarity.
- Review reopened conversations weekly to identify preventable miscommunication.

### 4) Performance and reporting

- Track first response time, resolution time, reopen rate, and CSAT.
- Segment results by queue and issue category to find bottlenecks.
- Run weekly retros and convert findings into SOP updates.

## Risk controls

- Avoid unresolved ownership during escalations.
- Prevent SLA breaches by setting alert thresholds before deadlines.
- Maintain a clear audit trail for customer-impacting decisions.

## Output format

When asked for help, provide:

- A triage matrix (severity, owner, SLA, escalation path).
- A reply quality rubric and review cadence.
- A weekly KPI review template with intervention thresholds.
