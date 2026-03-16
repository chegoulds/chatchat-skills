---
id: braintree-automation
name: Braintree Automation
description: Guidance-only playbook for running Braintree payment operations with safe checks, audit controls, and support workflows.
category: Business
requires: []
examples:
  - "Help me build a Braintree operations checklist for my team."
  - "Use braintree-automation to design a subscription and refunds workflow."
---

# Braintree Automation

Use this skill to design and run reliable Braintree operations workflows. This is a guidance-only skill and does not execute tools.

## When to use

- You need a standard process for customer setup, subscriptions, payment methods, and refunds.
- You want to reduce billing incidents and improve payment support handoffs.
- You need an audit-friendly operating model for payment changes.

## Operating principles

- Verify customer identity before changing billing details.
- Record every billing-impacting action with actor, timestamp, and reason.
- Prefer reversible changes and clear rollback paths.
- Escalate suspicious activity and never bypass fraud review steps.

## Core workflows

### 1) Customer onboarding and updates

- Collect only required billing information.
- Confirm tax region, billing currency, and invoice recipient.
- Validate customer communication preferences for payment notices.
- For profile updates, capture old value, new value, and approval source.

### 2) Subscription lifecycle

- Define plan, billing cadence, trial terms, and cancellation policy before activation.
- Before plan changes, preview proration impact and communicate it to the customer.
- Use clear effective dates for upgrades, downgrades, pauses, and resumes.
- At cancellation, confirm end-of-term vs immediate cancellation and notify stakeholders.

### 3) Payment method management

- Keep at least one valid default payment method where policy allows.
- Verify ownership and authorization before attaching or replacing a method.
- Never store sensitive card data in notes, tickets, or chat transcripts.
- On failed updates, retain the prior default until the new method is confirmed.

### 4) Refunds and disputes

- Require a reason code and approver for every refund.
- Document whether refund is full or partial and whether fees are recoverable.
- For disputes, preserve evidence timelines and assign an owner immediately.
- Track root causes (fraud, product issue, duplicate charge, misunderstanding).

## Support runbook template

Use this lightweight template when handling requests:

1. Request summary
2. Customer verification completed (yes/no)
3. Business policy checks passed (yes/no)
4. Action taken
5. Customer communication sent
6. Audit note recorded
7. Follow-up owner and due date

## Output format

When asked for help, provide:

- A step-by-step workflow tailored to the user's business model.
- A risk checklist with required approvals.
- A short customer communication draft for major billing changes.
