---
id: data-engineering-data-driven-feature
name: Data Engineering Data Driven Feature
description: Guidance-only playbook for designing data-driven features with experiment design, feature engineering, model evaluation, and production reliability practices.
category: Development
requires: []
examples:
  - "Help me design a data-driven feature pipeline."
  - "Use data-engineering-data-driven-feature for this task."
---

# Senior Data Scientist

Guidance-only skill for production-grade AI/ML and data systems. Use it to plan architecture, quality controls, and rollout strategy; do not execute scripts or deployment commands from this skill.

## Core expertise

- Statistical modeling and experimentation strategy
- Feature engineering patterns for training and inference parity
- Model evaluation frameworks and decision thresholds
- Data quality, observability, and drift monitoring
- MLOps and DataOps operating practices
- Cross-functional communication of analytical findings

## Planning workflow

1. Define business objective and measurable success criteria.
2. Specify data contracts, ownership, freshness, and quality rules.
3. Design feature pipeline and offline/online consistency strategy.
4. Choose evaluation metrics and acceptance thresholds.
5. Plan rollout with guardrails, monitoring, and rollback conditions.

## Production patterns

### Scalable data processing

- Design for horizontal scaling and idempotent jobs.
- Separate batch and real-time paths when latency requirements differ.
- Add schema checks and quality gates before downstream consumption.

### Model deployment reliability

- Use staged rollouts with clear pass/fail gates.
- Track latency, error rate, drift, and business KPIs together.
- Define retraining triggers and model retirement criteria.

### Real-time inference

- Use caching and batching where latency budgets allow.
- Maintain feature consistency between training and serving.
- Set SLOs for p50/p95/p99 latency and monitor continuously.

## Governance and compliance

- Protect PII with minimization, masking, and access controls.
- Document assumptions, limitations, and known failure modes.
- Align experiment and data use with applicable policy/regulation.

## Team execution standards

- Keep experiment docs reproducible and versioned.
- Use clear handoffs between data science, engineering, and product.
- Prioritize interpretable reporting for non-technical stakeholders.

## Output format

When asked for help, provide:

- A feature delivery plan (data, model, serving, monitoring).
- A validation checklist (quality, offline/online metrics, risk tests).
- A rollout plan with fallback and incident response triggers.
