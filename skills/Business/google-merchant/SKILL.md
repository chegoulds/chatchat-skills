---
id: google-merchant
name: Google Merchant
description: Guidance-only playbook for Google Merchant catalog operations, product quality checks, data source governance, and shopping performance analysis.
category: Business
requires: []
examples:
  - "Help me improve my Merchant Center catalog workflow."
  - "Use google-merchant to plan product quality and feed operations."
---

# Google Merchant Center Operations

Use this guidance-only skill to design and manage Merchant Center processes. It does not execute API calls directly.

## When to use

- You need product feed and catalog governance.
- You want to improve approval rates and product visibility.
- You need a repeatable reporting cadence for shopping performance.

## Core workflows

### 1) Catalog quality management

- Define required product attributes and validation rules.
- Standardize naming, taxonomy, and feed labeling conventions.
- Track disapprovals by root cause and assign fix owners.

### 2) Data source operations

- Define feed refresh cadence and failure escalation policy.
- Document source-of-truth for price, availability, and inventory.
- Create pre-publish checks before feed pushes.

### 3) Inventory and promotion governance

- Keep inventory updates synchronized with pricing updates.
- Define promotion eligibility, start/end timing, and fallback behavior.
- Audit low-stock and mismatch scenarios weekly.

### 4) Reporting and optimization

- Track clicks, impressions, CTR, conversion, and revenue by product group.
- Identify low-visibility and low-conversion SKUs for optimization.
- Prioritize title/image/price competitiveness improvements.

## Common pitfalls

- Inconsistent product identifiers across systems.
- Delayed feed refresh leading to price/stock mismatch.
- Promotion windows not aligned with inventory reality.
- Reporting that lacks action owners and due dates.

## Output format

When asked for help, provide:

- A Merchant operations checklist by lifecycle stage.
- A product-quality triage plan with severity levels.
- A KPI review template with next actions by owner.
