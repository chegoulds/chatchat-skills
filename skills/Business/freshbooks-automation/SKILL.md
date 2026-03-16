---
id: freshbooks-automation
name: Freshbooks Automation
description: Guidance-only playbook for FreshBooks operations, including business setup, project tracking, time logging, and billing workflows.
category: Business
requires: []
examples:
  - "Help me design a FreshBooks project and billing workflow."
  - "Use freshbooks-automation to audit time tracking and invoicing operations."
---

# FreshBooks Operations

Use this guidance-only skill to design and improve accounting operations in FreshBooks. It does not execute integrations or API calls directly.

## When to use

- You need a repeatable process for project-based billing.
- You want stronger time-tracking discipline before invoicing.
- You need operational checks for active projects and budgets.

## Core workflows

### 1) Business and project setup

- Confirm business entity setup and billing ownership.
- Define project lifecycle states (active, paused, completed, archived).
- Standardize project naming and client mapping conventions.

### 2) Time tracking governance

- Define required fields for every time entry (owner, task, duration, client/project).
- Enforce same-day logging for billable work.
- Review outliers weekly (missing logs, unusually long entries, miscoded tasks).

### 3) Budget and utilization checks

- Compare logged time against budgeted effort by project.
- Flag projects over budget or with low utilization.
- Escalate projects with repeated estimate drift.

### 4) Billing and invoicing readiness

- Validate billable entries before invoice generation.
- Confirm tax/currency settings and client billing terms.
- Keep a pre-send invoice checklist (scope, rates, dates, adjustments).

## Reporting cadence

- **Daily:** Missing time entries and overdue tasks.
- **Weekly:** Active project burn, budget variance, invoice pipeline.
- **Monthly:** Revenue by client/project and margin trends.

## Common pitfalls

- Incomplete business/project mapping causing billing errors.
- Time logs entered too late, reducing accuracy.
- Invoices sent without budget variance review.
- Weak handoff between delivery and finance owners.

## Output format

When asked for help, provide:

- A step-by-step operations workflow for the user’s business model.
- A checklist for project tracking and invoice readiness.
- A short reporting template with key metrics and alert thresholds.
