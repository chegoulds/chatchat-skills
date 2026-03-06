---
id: devops-troubleshooter
name: DevOps Troubleshooter
description: Troubleshoot DevOps failures across CI/CD, infrastructure, runtime services, and deployment pipelines using a structured incident workflow.
category: DevOps
requires: []
examples:
  - Help me troubleshoot a failing deployment pipeline and identify root cause.
  - Build a step-by-step incident triage plan for this infrastructure outage.
---

# DevOps Troubleshooter

Diagnose and resolve DevOps failures using evidence-driven triage and remediation steps.

## When to Use

- CI/CD, deployment, or infrastructure behavior is failing unexpectedly.
- You need a clear path from symptom to validated root cause.
- You want mitigation and recovery actions prioritized by impact.

## Troubleshooting Techniques

### Systematic Elimination

List possible failure domains (build, test, deploy, runtime, network, config, permissions). For each domain, define a binary check (pass/fail). Run checks in order of likelihood or ease. Eliminate domains that pass. Narrow to the failing domain before drilling deeper.

### Network Partition Testing

Verify connectivity between components: build agent to registry, deployer to cluster, service to service, service to DB. Use `curl`, `nc`, `telnet`, or service-specific health endpoints. Check firewall rules, security groups, and network policies. Isolate whether the failure is "can't reach" vs "reached but failed."

### Resource Contention Analysis

Check CPU, memory, disk I/O, and network saturation. Correlate with failure timing. Look for throttling, OOM kills, disk full, or bandwidth limits. Compare resource usage during failure vs baseline. Identify the bottleneck before adding capacity.

### Permission Audit Flow

Trace the identity used at each step (CI job, deploy service account, runtime pod). Verify that identity has required permissions (registry push, cluster deploy, secret access, DB credentials). Check for expired tokens, rotated keys, or changed RBAC. Document the permission chain from trigger to failure point.

## Domain-Specific Diagnosis Guides

### CI Failures

Check: (1) source checkout and branch; (2) dependency install (cache, registry); (3) build step (compiler, env vars); (4) test step (flaky tests, timeouts, test env); (5) artifact publish. Compare failing job config to last successful. Check for infra changes (runner image, network, secrets).

### Deployment Failures

Check: (1) artifact availability and integrity; (2) deploy target reachability; (3) rollout strategy and health checks; (4) config and secrets; (5) resource limits and scheduling. Inspect deploy logs, events, and pod status. Verify image pull, init containers, and readiness probes.

### Runtime Crashes

Check: (1) OOM, segfault, or unhandled exception; (2) startup failure (config, dependencies); (3) liveness/readiness probe failure; (4) dependency unavailable. Review pod logs, events, and exit codes. Check resource limits vs actual usage.

### Networking Issues

Check: (1) DNS resolution; (2) connectivity (firewall, security groups, network policies); (3) TLS/certificates; (4) load balancer and ingress. Test from the failing component's perspective. Verify service discovery and endpoint configuration.

## Workflow

1. Capture incident scope, impact, and timeline.
2. Verify recent changes (config, code, infra, dependencies).
3. Narrow failure domain (build, network, runtime, data, permissions).
4. Test hypotheses with focused checks.
5. Apply least-risk fix and verify recovery.
6. Add follow-up hardening actions to prevent recurrence.

## Common Pitfalls

- **Shotgun debugging**: Trying many changes at once. Makes it impossible to know what fixed the issue. Change one variable at a time; verify after each change.
- **Changing multiple things at once**: Same as above. Roll back or isolate to single change before declaring success.
- **Not capturing state before fixing**: Fixing without saving logs, config, or metrics. Post-incident analysis and postmortem suffer. Snapshot state (logs, `kubectl describe`, config dump) before any remediation.
- **Ignoring intermittent failures**: Treating flaky behavior as "fixed" after one success. Intermittent failures need pattern analysis (timing, load, specific inputs) and regression guards.

## Output Format

```markdown
## Incident Context
- Impact: <scope>
- First observed: <time>
- Failure domain: <CI|deploy|runtime|network|other>

## Triage Findings
- Suspected domain: <area>
- Evidence: <logs/metrics/events>
- Checks performed: <list with pass/fail>
- Ruled out: <domains eliminated with rationale>

## Domain-Specific Notes
- <relevant diagnosis from guide>

## Proposed Fix
- Change: <what to do>
- Risk: <low|medium|high>
- Rollback: <how to revert>
- State captured before fix: <what was saved for postmortem>

## Validation
- [ ] Service health restored
- [ ] Error rate normalized
- [ ] Regression checks pass
- [ ] Intermittent? <if yes, pattern and monitoring added>

## Follow-up
- Hardening actions: <list>
- Unknowns: <assumptions, edge cases>
```

## Constraints

- Prioritize service restoration before deep optimization.
- Validate every fix with measurable outcomes.
- Document assumptions and unresolved unknowns.