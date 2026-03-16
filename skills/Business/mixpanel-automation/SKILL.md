---
id: mixpanel-automation
name: Mixpanel Automation
description: Guidance-only playbook for Mixpanel analytics operations, including event governance, funnel monitoring, and decision-oriented reporting.
category: Business
requires: []
examples:
  - "Help me improve Mixpanel event taxonomy and dashboard standards."
  - "Use mixpanel-automation to design funnel monitoring and alerts."
---

# Mixpanel Analytics Operations

Use this guidance-only skill to design and improve analytics workflows in Mixpanel. It does not execute API calls or scripts.

## When to use

- You need trustworthy event tracking and naming standards.
- You want conversion and retention insights tied to business actions.
- You need a reporting rhythm for product and growth teams.

## Core workflows

### 1) Event taxonomy governance

- Define canonical event names and required properties.
- Document ownership for each tracked event and dashboard.
- Add change-control for schema updates to avoid reporting breaks.

### 2) Funnel and retention management

- Build critical funnels with clear stage definitions and exclusions.
- Segment by acquisition source, plan type, and user cohort.
- Track drop-off causes and convert findings into experiments.

### 3) KPI reporting cadence

- Establish weekly and monthly KPI reviews with owners.
- Report activation, conversion, retention, and feature adoption together.
- Use thresholds for alerts and predetermined response actions.

### 4) Data quality assurance

- Run regular checks for missing properties and unexpected spikes.
- Compare analytics trends with finance and CRM indicators for sanity.
- Keep a known-issues log for instrumentation gaps.

## Risk controls

- Avoid duplicate events and ambiguous naming.
- Prevent dashboard sprawl with a curated source-of-truth set.
- Separate exploratory metrics from decision-grade KPIs.

## Output format

When asked for help, provide:

- An event governance matrix.
- A funnel and retention review template.
- A KPI dashboard specification with action triggers.
