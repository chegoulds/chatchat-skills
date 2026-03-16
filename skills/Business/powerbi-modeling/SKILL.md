---
id: powerbi-modeling
name: Power BI Modeling
description: Guidance-only playbook for Power BI semantic modeling, including data model design, DAX governance, and reporting quality controls.
category: Business
requires: []
examples:
  - "Help me design a robust Power BI semantic model."
  - "Use powerbi-modeling to improve DAX quality and dashboard reliability."
---

# Power BI Modeling

Use this guidance-only skill to design and audit Power BI modeling practices. It does not execute scripts or external tools.

## When to use

- You need a scalable semantic model for decision-making.
- You want cleaner measures and consistent KPI definitions.
- You need model quality checks before dashboard rollout.

## Core workflows

### 1) Model architecture

- Use star schema patterns with clear fact and dimension boundaries.
- Standardize grain definitions and surrogate key usage.
- Minimize many-to-many joins unless there is a documented exception.

### 2) DAX and KPI governance

- Separate base measures from presentation measures.
- Keep KPI definitions versioned and approved by business owners.
- Enforce naming conventions and consistent time-intelligence logic.

### 3) Performance and maintainability

- Reduce high-cardinality dimensions where possible.
- Audit relationship directions and inactive relationships regularly.
- Track report load and query latency after major model changes.

### 4) Documentation and ownership

- Assign owners per table, measure set, and dashboard domain.
- Maintain a short data dictionary for key fields and KPIs.
- Review stale or duplicate measures on a fixed cadence.

## Risk controls

- Avoid KPI drift by preventing ad-hoc measure rewrites.
- Prevent hidden data quality issues with source-to-report reconciliations.
- Keep report security and access policies auditable.

## Output format

When asked for help, provide:

- A semantic model review checklist.
- A KPI definition and DAX governance template.
- A performance and quality audit plan.
