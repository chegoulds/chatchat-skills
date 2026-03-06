---
id: agent-performance-analyzer
name: Agent Performance Analyzer
description: Analyze AI agent performance using latency, success rate, cost, and quality signals to identify bottlenecks and improvement opportunities.
category: DevOps
requires: []
examples:
  - Analyze this agent workflow and identify the biggest performance bottlenecks.
  - Create a performance investigation checklist for latency, cost, and failure rate.
---

# Agent Performance Analyzer

Evaluate end-to-end agent execution quality and efficiency with actionable optimization guidance.

## When to Use

- You need to diagnose slow, expensive, or unreliable agent runs.
- You want to compare baseline performance against current behavior.
- You need concrete recommendations prioritized by impact.

## Workflow

1. Define performance goals (latency, quality, reliability, and cost).
2. Break execution into major stages and identify hotspots.
3. Quantify failure modes, retries, and low-quality outcomes.
4. Rank improvement candidates by impact, effort, and risk.
5. Propose a validation plan to measure post-change improvement.

## Concrete Analysis Techniques

**Flame graph-style step breakdown.** Visualize or tabulate time spent per step (LLM call, tool invocation, parsing, etc.). Identify which steps dominate total latency. Focus optimization on the top contributors first. Include both wall-clock time and percentage of total.

**Token usage profiling.** Track input and output tokens per step and per model. Compute cost from token counts and model pricing. Identify steps that consume disproportionate tokens. Consider caching, shorter prompts, or model downgrades for high-token steps.

**Cache hit rate analysis.** If the agent uses caching (e.g., for LLM responses or tool results), measure hit rate. Low hit rate may indicate poor cache key design, short TTL, or highly variable inputs. High hit rate with no latency improvement suggests cache lookup overhead or wrong cache layer.

**Retry cost accounting.** Count retries per step and per run. Multiply by average latency and cost per attempt. Retries can double or triple effective cost and latency. Factor retry cost into total run cost; do not report only successful-run metrics.

## Benchmark Methodology

**Controlled A/B testing.** Compare baseline vs. optimized configuration on the same input set. Use identical prompts, models, and environment. Run enough samples to reduce noise. Document the input set and any randomization.

**Statistical significance for latency comparisons.** Report confidence intervals or p-values for latency differences. Avoid concluding "faster" from a handful of runs. Use at least 30ΓÇô50 runs per configuration for basic significance; more for noisy workloads.

**Cost-per-quality-point metrics.** Combine cost and quality (e.g., accuracy, user satisfaction) into a single metric: cost per correct answer, or cost per quality point. Optimizing for cost alone can reduce quality; optimizing for quality alone can explode cost. Balance both.

## Common Pitfalls

**Optimizing for speed at cost of quality.** Reducing latency by shortening prompts, using weaker models, or skipping steps can degrade output quality. Always measure quality (accuracy, completeness, user rating) alongside latency. Document trade-offs explicitly.

**Measuring averages instead of percentiles.** Average latency hides tail behavior. Users hit p95 or p99. Report p50, p95, p99 (or p90, p99). Optimize for the percentiles that matter for user experience.

**Ignoring retry cost in latency measurements.** Reporting only successful-run latency undercounts the real user experience. Include retries in end-to-end latency and cost. A step with 20% retry rate effectively adds 20%+ to cost and latency.

**Comparing runs with different inputs.** Comparing baseline vs. optimized runs on different prompts or datasets invalidates the comparison. Use the same input set. If inputs must vary, use a large, representative set and report variance.

## Output Format

```markdown
## Performance Snapshot
- Latency: p50 / p95 / p99 (ms)
- Success rate: <percent>
- Retry rate: <percent>
- Cost per run: <baseline vs current>
- Cost per quality point: <if applicable>

## Step Breakdown (Flame-Style)
| Step | Time (ms) | % of Total | Tokens | Cost |
|------|-----------|------------|--------|------|
| LLM call 1 | 1200 | 45% | 2.1k | $0.02 |
| Tool: search | 800 | 30% | - | - |
| LLM call 2 | 600 | 22% | 1.5k | $0.015 |

## Bottlenecks
1. <bottleneck>: <evidence, metric>
2. <bottleneck>: <evidence, metric>

## Cache Analysis (if applicable)
- Hit rate: <percent>
- Impact on latency: <ms saved>

## Retry Analysis
- Retries per run: <avg>
- Cost impact: <percent>
- Primary causes: <list>

## Recommended Optimizations
| Change | Impact | Effort | Risk |
|--------|--------|--------|------|
| <change 1> | high | low | low |
| <change 2> | medium | medium | medium |

## Validation Plan
- [ ] Baseline: <metric, sample size>
- [ ] Apply optimization
- [ ] Compare: <same input set, statistical test>
- [ ] Quality check: <metric>
```

## Constraints

- Prefer evidence-based recommendations over assumptions.
- Separate measurement from interpretation to avoid bias.
- Call out trade-offs when reducing latency may reduce quality.