---
id: multi-cloud-architecture
name: Multi-Cloud Architecture
description: Design and evaluate multi-cloud architectures with explicit goals, shared control planes, and failover patterns. Use when balancing resiliency, compliance, vendor risk, and workload placement across cloud providers.
category: DevOps
requires: []
examples:
  - Propose active-passive failover between two cloud providers.
  - Define a service placement strategy across clouds by workload type.
---

# Multi-Cloud Architecture

Create practical multi-cloud designs that optimize for business goals, not provider symmetry.

## When to Use

- Uptime, data sovereignty, or procurement constraints require multiple providers.
- Teams need clear boundaries on what runs where and why.
- You need a migration/failover strategy that can be tested regularly.

## Workload Classification Framework

Classify workloads before placement:

- **Stateless compute**: Web tiers, API gateways, batch workers. Highest portability; run anywhere with minimal config drift. Prefer container images and IaC parity.
- **Stateful data**: Databases, queues, object stores. Lowest portability; replication and failover are complex. Decide: replicate, backup-only, or single-cloud with tested restore.
- **Edge/CDN**: Cache, DDoS protection, low-latency delivery. Often provider-specific (CloudFront, Cloud CDN). Use provider-native unless multi-CDN is a goal.
- **ML training**: GPU clusters, distributed jobs. Heavy data locality and quota constraints. Often single-cloud per job; use SkyPilot or similar for portability.

Map each workload to one class and document statefulness, data gravity, and compliance requirements.

## Shared Control Plane Design Patterns

- **Federated identity**: Use OIDC/SAML with a central IdP (Okta, Auth0, Azure AD) or provider-native federation (AWS IAM Identity Center, GCP Workload Identity). Avoid per-cloud user stores; sync groups and roles via SCIM or automation.
- **Cross-cloud observability**: Centralize logs and metrics in one place (Datadog, Grafana Cloud, self-hosted Prometheus/Loki). Use agent-based collection or provider-native export to avoid tool sprawl. Define SLOs and alerting once.
- **Unified secrets**: HashiCorp Vault, Doppler, or provider-native (AWS Secrets Manager, GCP Secret Manager) with replication or sync. Never duplicate secrets manually; use automation and rotation.
- **CI/CD**: Single pipeline (GitHub Actions, GitLab CI, Jenkins) that deploys to multiple targets via provider SDKs or Terraform. Use environment-specific variables and approval gates.

## Failover Pattern Details

- **Active-active with global load balancing**: Traffic split across clouds via Route53, Cloudflare, or GSLB. Requires data replication and conflict resolution. Use for stateless or eventually-consistent workloads. Validate session affinity and data consistency.
- **Active-passive with DNS failover**: Primary in cloud A; standby in cloud B. Failover via DNS TTL and health checks. RTO depends on TTL and automation. Requires periodic failover drills.
- **Pilot-light**: Minimal standby (DB replica, config only) in secondary cloud. Scale up compute on failover. Lower cost, higher RTO. Document scale-up runbook and data sync lag.

Choose based on RTO/RPO and budget. Document failover steps, ownership, and test frequency.

## Common Pitfalls

- **Symmetry bias**: Duplicating every service across clouds when only a subset needs resilience. Increases cost and operational load. Place only critical workloads in secondary cloud.
- **Untested failover**: Assuming failover works without drills. Run game days quarterly; validate DNS, networking, and data restore.
- **Cost explosion from data egress**: Cross-cloud data transfer is expensive. Keep data gravity in mind; avoid unnecessary replication or sync.
- **Tool sprawl**: Different monitoring, secrets, or CI per cloud. Consolidate control planes; use one tool per concern where possible.

## Workflow

1. Define architecture goals and non-goals (resilience, latency, compliance, cost, lock-in).
2. Classify workloads using the framework above.
3. Design shared foundations (identity, observability, networking, secrets, CI/CD).
4. Map service placement and failover pattern (active-active, active-passive, pilot-light, backup-only).
5. Validate with game days, cost modeling, and operational runbooks.

## Output Format

```markdown
## Multi-Cloud Architecture Proposal
- Primary goals: <list>
- Providers in scope: <list>
- Workload classes: <list with classification (stateless/stateful/edge/ML)>

## Workload Classification Summary
| Workload | Class | Statefulness | Data Gravity | Compliance |
|---|---|---|---|---|
| <service> | <stateless/stateful/edge/ML> | <none/replicated/single> | <location> | <requirements> |

## Placement Strategy
| Workload | Cloud | Pattern | Reason |
|---|---|---|---|
| <service> | <provider> | <active-active/active-passive/pilot-light/backup-only> | <why> |

## Shared Control Planes
- Identity: <approach, IdP, federation method>
- Observability: <tool, aggregation point, SLO coverage>
- Secrets/KMS: <tool, replication, rotation>
- CI/CD: <pipeline, deployment targets, approval gates>

## Failover Details
- Pattern: <active-active/active-passive/pilot-light>
- RTO target: <value>
- RPO target: <value>
- Test frequency: <e.g., quarterly game day>
- Runbook reference: <link or location>

## Risk and Readiness Checklist
- [ ] Failover tested
- [ ] RTO/RPO validated
- [ ] Cost guardrails defined (including egress)
- [ ] Ops ownership documented
- [ ] Symmetry bias avoided (only critical workloads in secondary)
- [ ] Control plane consolidated (no tool sprawl)
```

## Constraints

- Avoid duplicating everything across clouds unless required by goals.
- Keep operational complexity proportional to reliability benefit.
- Require tested failover; untested redundancy is not resilience.