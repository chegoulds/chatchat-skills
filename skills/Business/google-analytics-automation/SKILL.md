---
id: google-analytics-automation
name: Google Analytics Automation
description: Guidance-only playbook for planning GA4 reporting workflows, KPI definitions, and analytics operations.
category: Business
requires: []
examples:
  - "Help me design a GA4 reporting workflow."
  - "Use google-analytics-automation to define weekly analytics checks."
---

# Google Analytics Operations

Use this guidance-only skill to design and improve Google Analytics workflows. It does not execute integrations or API calls directly.

## When to use

- You need recurring KPI reporting from GA4.
- You want a plan for event-level and funnel-level analysis.
- You need governance for dimensions, metrics, and naming standards.

## Core workflows

### 1) Account and property planning

- Define which properties and views map to business units.
- Create naming standards for reports and dashboards.
- Document ownership for metric quality and maintenance.

### 2) Standard reporting design

- Define key dimensions (channel, page, geo, device).
- Define key metrics (users, sessions, conversion, revenue, retention).
- Set reporting windows (daily/weekly/monthly) and comparison baselines.

### 3) Funnel and conversion analysis

- Map each funnel step to a measurable event.
- Track drop-off between steps and investigate root causes.
- Separate acquisition issues from product experience issues.

### 4) Data quality controls

- Validate event naming consistency and parameter completeness.
- Audit metric/dimension compatibility before publishing dashboards.
- Flag anomalies with an escalation owner and response window.

## Common pitfalls

- Mixing business definitions across teams (e.g., different conversion logic).
- Overloaded dashboards with low-actionability metrics.
- Reporting without time-window comparability or context.
- Missing QA for event instrumentation changes.

## Output format

When asked for help, provide:

- A reporting blueprint (what to track, at what cadence, by whom).
- A KPI glossary with definitions and data-source ownership.
- A short action plan for optimization based on observed trends.
