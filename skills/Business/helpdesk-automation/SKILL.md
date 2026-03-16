---
id: helpdesk-automation
name: Helpdesk Automation
description: Guidance-only playbook for support ticket workflows, contact/company operations, SLA management, and escalation design.
category: Business
requires: []
examples:
  - "Help me build a helpdesk ticket triage workflow."
  - "Use helpdesk-automation to define support escalation rules."
---

# Helpdesk Operations

Use this guidance-only skill to design support workflows and service quality controls. It does not execute integration tools directly.

## When to use

- You need standard operating procedures for ticket handling.
- You want better SLA compliance and escalation discipline.
- You need structured contact/company data governance.

## Core workflows

### 1) Ticket lifecycle management

- Define intake fields (issue type, severity, customer context).
- Standardize status transitions (open, pending, resolved, closed).
- Define assignment rules by queue, priority, and expertise.

### 2) Search and triage

- Establish query patterns for urgent, stale, and high-risk tickets.
- Build triage rules for first-response and resolution-time targets.
- Use tags and categories consistently for reliable reporting.

### 3) Replies and internal notes

- Separate customer-facing responses from internal notes.
- Enforce response templates for high-frequency issue types.
- Require clear next action and owner on each update.

### 4) Contact and company operations

- Maintain canonical customer identifiers and domain mapping rules.
- Audit duplicates and stale records monthly.
- Ensure company-level attributes support routing and prioritization.

## Escalation framework

- **Immediate escalation:** security, legal, outages, executive complaints.
- **Timed escalation:** unresolved critical tickets beyond SLA threshold.
- **Functional escalation:** issues requiring engineering or billing specialists.

## Output format

When asked for help, provide:

- A ticket workflow SOP (triage -> resolve -> close).
- SLA and escalation matrix by severity.
- A quality checklist for responses and data hygiene.
