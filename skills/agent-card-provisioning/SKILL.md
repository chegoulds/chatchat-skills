---
id: agent-card-provisioning
name: "Agent Card Provisioning"
description: "Provision virtual payment cards for AI agents on-demand. Create single-use or limited cards with spending controls, merchant restrictions, and automatic expiration. Cards are issued instantly when policy allows."
category: Business
requires: []
examples:
  - "Design a policy for agent card provisioning for office purchases under $200."
  - "Create a safe approval workflow for high-value autonomous card spend."
---

# Agent Card Provisioning

Use this skill as a guidance-only framework for designing controlled card provisioning workflows for autonomous agents.

## Use this skill when

- You need policy design for autonomous card spending.
- You need approval thresholds and escalation paths for payment requests.
- You need audit-ready controls for card lifecycle and transaction tracking.

## Do not use this skill when

- The request expects direct card issuance or API execution from this skill.
- The task is unrelated to payment controls or autonomous spending governance.

## Guardrails

- Treat this as planning guidance only; do not claim to perform real card operations.
- Do not output or request sensitive payment data (full PAN, CVV, or secrets).
- If implementation is requested, provide a checklist for the user to run in their own approved system.

## Policy Design Workflow

1. **Define spend scope**
   - Approved use cases (for example: office supplies, software subscriptions).
   - Excluded categories and merchants.
   - Geographic and currency constraints.

2. **Set risk controls**
   - Per-transaction cap.
   - Daily and monthly cumulative limits.
   - Velocity controls (frequency limits).
   - Automatic expiration rules.

3. **Define approval logic**
   - Auto-approve zone (low-risk amounts and known categories).
   - Manual-approval zone (high-value, unusual merchants, policy exceptions).
   - Rejection rules with clear reasons.

4. **Design audit model**
   - One request per purchase intent.
   - Consistent naming convention for reconciliation.
   - Required fields: requester, purpose, amount, category, approver outcome.
   - Retention and review cadence for logs.

5. **Plan incident handling**
   - Suspicious pattern detection triggers.
   - Temporary freeze and revoke criteria.
   - Escalation owner and response SLAs.

## Recommended Controls Checklist

- [ ] Allowed and blocked spend categories documented
- [ ] Per-transaction and cumulative caps defined
- [ ] Manual approval thresholds and owners defined
- [ ] Time-based expiry and deactivation rules documented
- [ ] Audit fields and logging requirements standardized
- [ ] Exception handling and incident playbook defined

## Output format

```markdown
## Provisioning Policy Summary
- Use cases:
- Exclusions:
- Risk profile:

## Limits and Controls
- Per-transaction cap:
- Daily/monthly caps:
- Velocity limits:
- Expiry policy:

## Approval Workflow
- Auto-approve conditions:
- Manual review triggers:
- Rejection conditions:
- Escalation owner:

## Audit and Monitoring
- Required log fields:
- Review cadence:
- Incident triggers:
- Response actions:
```
