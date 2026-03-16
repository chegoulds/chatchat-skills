---
id: apollo-prod-checklist
name: Apollo Prod Checklist
description: Prepare Apollo.io integrations for production with a structured readiness checklist covering security, reliability, observability, and go-live controls.
category: DevOps
requires: []
examples:
  - "Create a production readiness checklist for our Apollo integration launch."
  - "Review this Apollo go-live plan and flag missing controls."
author: "Jeremy Longshore <jeremy@intentsolutions.io>"
version: "1.0.0"
---

# Apollo Production Checklist

Use this skill as a guidance-only readiness framework for Apollo integration deployments.

## Use this skill when

- The team is preparing to deploy Apollo integration changes.
- You need pre-launch validation gates and rollback planning.
- You need a structured production-readiness audit.

## Do not use this skill when

- The request is unrelated to deployment readiness.
- The user expects direct script execution from this skill.

## Guardrails

- Treat this as planning and validation guidance; do not claim to run checks automatically.
- Never expose API keys or secrets in outputs.
- Require explicit go/no-go criteria before launch recommendation.

## Readiness workflow

1. **Define deployment scope**
   - Features being released
   - Impacted services and dependencies
   - Rollout strategy (full, staged, canary)

2. **Security and secrets checks**
   - Confirm production secrets are in approved secret storage.
   - Verify least-privilege API access.
   - Confirm PII handling and log redaction policy.

3. **Reliability checks**
   - Validate retry strategy and timeout boundaries.
   - Confirm rate-limit handling and backoff behavior.
   - Define failure fallback and degradation behavior.

4. **Observability checks**
   - Ensure key metrics and error rates are tracked.
   - Define alert thresholds and on-call ownership.
   - Confirm health indicators and incident escalation path.

5. **Operational readiness**
   - Validate environment variables and release configuration.
   - Verify runbook, rollback steps, and communication plan.
   - Confirm post-deploy smoke-test checklist.

6. **Go/no-go decision**
   - List blocking issues (must-fix)
   - List warnings (acceptable with mitigation)
   - Declare launch recommendation with rationale

## Deployment checklist

- [ ] Scope and release owner confirmed
- [ ] Secrets and API permissions verified
- [ ] Error handling and retry logic validated
- [ ] Rate-limit and timeout behavior reviewed
- [ ] Monitoring dashboards and alerts ready
- [ ] Rollback plan tested/documented
- [ ] Smoke-test plan prepared
- [ ] Stakeholder communication plan ready
- [ ] Go/no-go criteria documented

## Output format

```markdown
## Apollo Production Readiness Summary
- Release scope:
- Owner:
- Environment:

## Checklist Status
- Security:
- Reliability:
- Observability:
- Operations:

## Blocking Issues (Must Fix)
1.
2.

## Warnings (Mitigate)
1.
2.

## Rollback and Incident Plan
- Rollback trigger:
- Rollback steps:
- Incident owner:

## Recommendation
- Go/No-Go:
- Reasoning:
- Next actions:
```
