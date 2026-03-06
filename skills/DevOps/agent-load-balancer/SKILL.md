---
id: agent-load-balancer
name: Agent Load Balancer
description: Design load balancing and traffic distribution strategies for AI agents to improve reliability, latency, and throughput under varying demand.
category: DevOps
requires: []
examples:
  - Propose a load balancing strategy for this agent system under peak traffic.
  - Build a rollout plan for weighted routing between two agent backends.
---

# Agent Load Balancer

Plan and validate traffic distribution patterns for resilient agent operations.

## When to Use

- You need to distribute request load across multiple agent services/models.
- You want failover behavior with minimal user impact.
- You need safe rollout of new backends using gradual traffic shifting.

## Workflow

1. Identify traffic profile, peak patterns, and critical paths.
2. Choose routing strategy (round robin, weighted, latency-aware, failover-first).
3. Define health checks and circuit-breaker conditions.
4. Set rollout weights and rollback triggers.
5. Validate with synthetic and real-traffic checkpoints.

## Concrete Load Balancing Patterns

**Weighted round-robin.** Distribute requests in proportion to backend capacity. Backend A gets 70%, B gets 30%. Use when backends have different capacity or you are doing gradual rollout. Update weights incrementally; avoid sudden 100% shifts.

**Least-connections.** Route to the backend with the fewest active connections. Suited for long-lived agent sessions where request duration varies. Requires accurate connection counting; ensure connections are properly tracked and released.

**Latency-based routing.** Route to the backend with lowest recent latency. Use percentile latency (e.g., p95) over a sliding window. Protects users from slow or degraded backends. Requires low-overhead latency measurement and a fallback when metrics are stale.

**Consistent hashing for session affinity.** When a user session must stick to one backend (e.g., for stateful context), use consistent hashing on session ID. Ensures the same backend handles follow-up requests. Balance with failover: if the backend is unhealthy, break affinity and route elsewhere.

## Health Check Design

**Active vs passive.** Active checks: periodic probes (HTTP, gRPC) to each backend. Passive checks: observe success/failure of real traffic. Use both: active for fast detection of dead backends, passive for detecting degradation under load. Tune frequency to avoid overwhelming backends.

**Failure threshold tuning.** Require N consecutive failures before marking unhealthy. Too low: transient blips cause unnecessary failover. Too high: users hit a bad backend for too long. Typical: 2ΓÇô3 failures over 10ΓÇô30 seconds. Document and test the chosen values.

**Circuit breaker integration.** When a backend is unhealthy, open the circuit: stop sending traffic for a cooldown period. After cooldown, send a probe (half-open). If it succeeds, close the circuit. Prevents cascade failures and gives the backend time to recover.

## Common Pitfalls

**No health checks causing traffic to dead backends.** Without health checks, the load balancer keeps routing to backends that are down or unresponsive. Always configure health checks. Validate that they run and that unhealthy backends are removed from the pool.

**Sticky sessions preventing failover.** Session affinity keeps traffic on one backend. If that backend fails, users with affinity to it get errors until the session expires. Implement failover: when the preferred backend is unhealthy, break affinity and route to a healthy one.

**Uneven weight distribution.** Setting weights that don't match capacity (e.g., 50/50 when one backend has 2x capacity) causes overload on the weaker backend. Base weights on measured capacity or start conservative and adjust from metrics.

**Missing circuit breaker causing cascade failures.** Sending continuous traffic to a failing backend can exhaust timeouts and threads, affecting the whole system. Use a circuit breaker to stop traffic to failing backends and allow recovery.

## Output Format

```markdown
## Routing Strategy
- Primary backend(s): <list with capacity>
- Policy: <weighted round-robin | least-connections | latency-based | consistent hashing>
- Weights (if applicable): <backend: weight>
- Session affinity: <none | consistent hash on X>

## Health and Failover
- Active checks: <protocol, path, interval, timeout>
- Passive checks: <observe success/failure, window>
- Failure threshold: <N failures in M seconds>
- Circuit breaker: <enabled, cooldown, half-open probe>
- Failover: <break affinity when unhealthy: yes|no>

## Rollout Plan
- Stage 1: <traffic split, duration>
- Stage 2: <traffic split, duration>
- Rollback trigger: <error rate > X% | latency p95 > Yms | manual>
- Rollback action: <revert weights, disable new backend>

## Validation
- [ ] Health checks remove unhealthy backends
- [ ] Failover works when primary is down
- [ ] Weights match capacity under load
- [ ] Circuit breaker opens and recovers correctly
```

## Constraints

- Prefer incremental traffic shifts over full cutovers.
- Keep failover criteria explicit and measurable.
- Define rollback before rollout.