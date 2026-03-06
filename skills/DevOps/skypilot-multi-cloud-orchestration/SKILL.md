---
id: skypilot-multi-cloud-orchestration
name: SkyPilot Multi-Cloud Orchestration
description: Plan and operate SkyPilot workloads across cloud providers with cost controls, capacity fallback, and deployment reliability checks.
category: DevOps
requires: []
examples:
  - Create a multi-cloud SkyPilot launch strategy with fallback regions and budgets.
  - Troubleshoot SkyPilot provisioning failures and recover workloads safely.
---

# SkyPilot Multi-Cloud Orchestration

Run portable SkyPilot workloads with predictable cost and resilience.

## When to Use

- You need to schedule jobs across multiple cloud providers.
- You need fallback behavior for quota, capacity, or region failures.
- You want explicit cost and reliability controls for long-running jobs.

## SkyPilot Task Definitions (sky.yaml)

Define tasks in `sky.yaml`:

```yaml
resources:
  cloud: aws  # or gcp, azure, lambda, etc.
  accelerators: V100:1
  disk_size: 100
  region: us-west-2

file_mounts:
  /data: s3://bucket/path
  ~/code: ./local_repo

setup: |
  pip install -r requirements.txt

run: python train.py
```

Use `sky launch -c <cluster> task.yaml` to provision. Use `sky exec` for one-off runs on existing clusters.

## Spot Instance Strategies

- **Spot with fallback**: Set `spot: true`; SkyPilot will retry on preemption. Add checkpointing in your workload.
- **Spot + on-demand fallback**: Use `--spot-recovery` or configure fallback regions. Specify multiple clouds/regions in resources.
- **On-demand only**: Set `spot: false` for critical jobs. Higher cost, no preemption.

Always pair spot with persistent storage and checkpoint/resume logic.

## Storage Mounts

- **S3/GCS/Azure Blob**: Use `file_mounts` with `s3://`, `gs://`, or `az://` URIs. Data is synced at launch.
- **Local to remote**: Mount local dirs with `./path`; SkyPilot rsyncs at launch.
- **Persistent disk**: Use `disk_size` for instance-attached storage. For cross-run persistence, use cloud object storage or a shared NFS.

## Autostop Policies

Set `idle_minutes_to_autostop` in `sky.yaml` or via `sky launch --idle-minutes-to-autostop 10`. Prevents runaway costs when jobs finish but the cluster stays up. Use `--no-autostop` only when explicitly needed (e.g., debugging).

## Capacity Fallback Configuration

In `sky.yaml` or CLI:

```yaml
resources:
  cloud: aws
  region: us-west-2
  # SkyPilot will try fallback regions if primary fails
```

Use `sky launch --cloud aws --region us-west-2,us-east-1` to specify fallback regions. For multi-cloud: list clouds in order; SkyPilot tries each until capacity is found.

## Common Pitfalls

- **No autostop**: Idle clusters burn budget. Always set `idle_minutes_to_autostop` unless debugging.
- **Spot preemption without checkpoint**: Jobs die on preemption with no resume. Implement checkpointing and `sky launch` retries.
- **Region-specific quota exhaustion**: Single-region strategy fails when quota is full. Configure fallback regions or clouds.
- **Data locality ignored**: Launching far from data causes slow sync and egress cost. Prefer regions close to S3/GCS buckets.

## Concrete SkyPilot CLI Commands

- `sky launch -c mycluster task.yaml` ΓÇö Launch cluster and run task
- `sky exec mycluster python train.py` ΓÇö Run command on existing cluster
- `sky stop mycluster` ΓÇö Stop cluster (preserves disk)
- `sky down mycluster` ΓÇö Terminate and delete cluster
- `sky status` ΓÇö List clusters and status
- `sky logs mycluster` ΓÇö Stream logs
- `sky launch --spot --idle-minutes-to-autostop 15 -c train task.yaml` ΓÇö Spot with autostop

## Workflow

1. Define workload requirements: compute shape, data locality, and runtime constraints.
2. Select primary and fallback clouds or regions based on capacity and cost.
3. Configure SkyPilot task and launch settings for retries and recovery.
4. Validate execution, logs, and artifact persistence across failures.
5. Document runbook steps for scaling, interruption handling, and cleanup.

## Output Format

```markdown
## Workload Profile
- Job type: <training/batch/service>
- Resource needs: <cpu/gpu/memory>
- Checkpoint strategy: <where, frequency>

## sky.yaml Snippet
- Include key resources, file_mounts, setup, run

## Placement Strategy
- Primary: <cloud/region>
- Fallback: <cloud/region list>
- Spot: <yes/no, recovery strategy>
- Budget guardrails: <limits>
- Autostop: <idle_minutes or disabled with reason>

## CLI Launch Command
`sky launch -c <name> [flags] task.yaml`

## Reliability Checklist
- [ ] Retry policy configured
- [ ] Persistent storage path defined
- [ ] Checkpoint/resume implemented
- [ ] Autostop configured
- [ ] Fallback regions/clouds set
- [ ] Cleanup and shutdown steps documented
```

## Constraints

- Avoid cloud-specific assumptions unless explicitly requested.
- Include both cost and reliability trade-offs in recommendations.
- Do not expose credentials or account identifiers.