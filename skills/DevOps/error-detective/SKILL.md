---
id: error-detective
name: Error Detective
description: Triage production and CI errors methodically using logs, traces, recent changes, and reproducible validation steps.
category: DevOps
requires: []
examples:
  - Help me debug this production error with a structured triage flow.
  - Build an error investigation checklist from these logs and stack traces.
---

# Error Detective

Investigate failures systematically to isolate root cause and propose safe fixes.

## When to Use

- You are diagnosing runtime, deployment, or CI errors.
- You need to separate symptoms from root cause.
- You want reproducible verification before declaring resolution.

## Debugging Techniques

### 5 Whys Analysis

For each observed symptom, ask "why" five times. Document each answer with evidence. Stop when you reach a controllable cause (code, config, infra) or an external dependency. Use this to distinguish proximate cause from contributing factors.

### Fault Tree Construction

Build a top-down tree: failure at top, possible causes as branches. Assign likelihood and evidence status (confirmed / suspected / ruled out) to each branch. Prune branches with negative evidence. This prevents tunnel vision on a single hypothesis.

### Change Correlation Matrix

List recent changes (commits, config, deploys, dependency updates) in columns and symptoms in rows. Mark which changes correlate temporally with symptom onset. Prioritize changes that touch the failing code path or resource.

### Binary Search Through Recent Commits

When a regression is suspected, bisect commits between last-known-good and first-known-bad. Run the failing scenario at each bisection point. Narrow to the smallest change that introduces the failure. Document the exact diff for root cause.

## Error Classification Guide

- **Infrastructure**: Host, network, storage, orchestration (k8s, VMs). Look for resource exhaustion, connectivity failures, DNS, TLS/certificate issues. Evidence: infra logs, metrics, health checks.
- **Application**: Code bugs, logic errors, unhandled exceptions. Look for stack traces, error messages, unexpected state. Evidence: application logs, traces, core dumps.
- **Data**: Schema mismatch, corrupt data, migration failures, quota limits. Look for validation errors, constraint violations, null/missing fields. Evidence: DB logs, migration logs, data samples.
- **Dependency**: Upstream services, libraries, external APIs. Look for timeouts, 5xx from dependencies, version mismatches. Evidence: dependency logs, network traces, dependency health.

## Log and Trace Investigation Patterns

- **Error burst pattern**: Cluster errors by message and timestamp. Identify the first occurrence; earlier errors often cause cascading failures.
- **Trace span analysis**: Follow a single trace_id through spans. Identify the span where latency spikes or errors originate. Check upstream vs downstream.
- **Correlation ID propagation**: Verify correlation IDs flow through all services. Gaps indicate missing instrumentation or dropped requests.
- **Log level escalation**: Check if DEBUG/INFO logs near the error provide context (input values, state) that the error message omits.

## Workflow

1. Capture error signature (message, stack trace, timestamp, affected scope).
2. Correlate with recent code/config/dependency changes.
3. Check service health signals and upstream/downstream dependencies.
4. Build a minimal reproduction path.
5. Identify likely root cause and rank fix options by risk.
6. Define validation steps and regression guards.

## Common Pitfalls

- **Fixing symptoms not causes**: Addressing the error message without understanding why it occurs. The fix may mask the real issue or recur elsewhere.
- **Ignoring contributing factors**: Multiple factors (e.g., load + bug + config) can combine. Fixing one may not fully resolve the issue.
- **Premature root cause declaration**: Declaring root cause before reproducible validation. Always verify with a minimal reproduction or controlled test.
- **Not testing the fix**: Deploying a fix without regression checks or monitoring. Validate in staging and watch production metrics post-deploy.

## Output Format

```markdown
## Incident Snapshot
- Error signature: <message/trace>
- Affected scope: <users/services/env>
- Error classification: <infrastructure|application|data|dependency>
- First occurrence: <timestamp>

## Investigation Summary
- Fault tree / 5 whys summary
- Change correlation: <which changes align with onset>
- Log/trace evidence: <key excerpts with timestamps>

## Likely Causes (ranked)
1. <cause 1> ΓÇö evidence: <brief>
2. <cause 2> ΓÇö evidence: <brief>

## Validation Plan
- [ ] Reproduce failure (minimal steps)
- [ ] Apply fix in controlled environment
- [ ] Confirm recovery metrics/logs
- [ ] Add regression protection
- [ ] Monitor post-deploy for N hours

## Proposed Fix
- Change: <what to modify>
- Risk: <low/medium/high>
- Rollback: <fallback action>
- Unknowns: <assumptions, edge cases not verified>
```

## Constraints

- Do not declare root cause without supporting evidence.
- Prefer smallest safe fix first, then harden.
- Explicitly call out unknowns and assumptions.