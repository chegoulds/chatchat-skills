---
id: lambda-labs-gpu-cloud
name: Lambda Labs GPU Cloud
description: Provision and operate Lambda Labs GPU Cloud instances for ML workloads with reproducible setup, cost awareness, and safe teardown. Use when launching training or inference infrastructure on Lambda GPU.
category: DevOps
requires: []
examples:
  - Set up a Lambda GPU instance for PyTorch training with persistent storage and SSH access.
  - Troubleshoot CUDA driver mismatch and recover a failing training job on Lambda Cloud.
---

# Lambda Labs GPU Cloud

Provision, validate, and operate Lambda GPU instances for training or inference with reproducible setup and safe teardown.

## When to Use

- You need a fast GPU environment for training or inference.
- You want a repeatable setup for CUDA, frameworks, and datasets.
- You need to manage runtime cost and avoid idle GPU burn.

## Lambda Cloud Patterns

- **API provisioning**: Use Lambda Cloud API or web console. API: `POST /instances` with `instance_type_name`, `region`, `ssh_key_names`. Store instance ID for lifecycle management.
- **SSH key management**: Add SSH keys in dashboard; reference by name in API. Use `ssh ubuntu@<instance-ip> -i ~/.ssh/lambda_key` for access.
- **Persistent filesystem**: Instance storage is ephemeral. Attach a Filesystem (NFS) or use S3/GCS for datasets and checkpoints. Mount at launch; sync checkpoints periodically.
- **Framework compatibility matrix**: Lambda provides CUDA 11.8/12.x on Ubuntu 22.04. PyTorch 2.x, TensorFlow 2.x, JAX supported. Match framework CUDA build to instance driver: `torch.cuda.is_available()` and `nvidia-smi` must agree.

## CUDA Troubleshooting Guide

- **Driver mismatch**: `nvidia-smi` shows driver version; PyTorch/TF expect matching CUDA. If `torch.cuda.is_available()` is False, reinstall framework with correct CUDA variant: `pip install torch --index-url https://download.pytorch.org/whl/cu118`.
- **Multi-GPU NCCL issues**: Set `NCCL_DEBUG=INFO`, `NCCL_IB_DISABLE=1` if InfiniBand issues. Ensure all GPUs visible: `torch.cuda.device_count()`.
- **VRAM OOM strategies**: Reduce batch size; use gradient accumulation; enable `torch.cuda.empty_cache()`; use mixed precision (AMP); offload optimizer states (DeepSpeed ZeRO).

## Common Pitfalls

- **Forgetting checkpoints on ephemeral instances**: Instance storage is lost on stop/terminate. Sync checkpoints to S3/GCS or attached Filesystem every N steps.
- **CUDA version mismatch after OS update**: Avoid `apt upgrade` that changes kernel/driver. Pin CUDA toolkit; use conda or venv for isolation.
- **Not monitoring GPU utilization**: Use `nvidia-smi -l 1` or `gpustat`. Low utilization may indicate data loading bottleneck or misconfigured dataloader workers.
- **Leaving idle instances running**: Lambda bills by the hour. Stop instances when not in use; use `lambda-cloud stop <instance-id>` or API.

## Workflow

1. Define workload requirements:
   - GPU model/VRAM target
   - CPU/RAM and disk needs
   - region and expected runtime
2. Provision instance and attach storage sized for dataset + checkpoints.
3. Bootstrap environment:
   - Install exact CUDA/toolkit-compatible framework versions
   - Configure package cache and virtual environment
   - Validate GPU visibility (`nvidia-smi`, framework device check)
4. Run a short smoke workload before full job submission.
5. Add checkpoints/log syncing to durable storage.
6. Record exact launch command and env manifest for reproducibility.
7. Stop or delete idle instances immediately after completion.

## Validation Checklist

- [ ] GPU detected and driver compatible with framework.
- [ ] Data path mounted and read/write tested.
- [ ] Checkpoint and logs persist off-instance.
- [ ] Resume from checkpoint succeeds on a restarted instance.
- [ ] Startup and shutdown steps documented for reruns.
- [ ] CUDA/framework versions pinned and documented.
- [ ] Idle instance stop procedure defined.

## Output Format

```markdown
## Provisioning Summary
- Instance spec: <type, GPU, VRAM>
- Region: <region>
- Storage: <attached Filesystem or S3 path>
- Instance ID: <for lifecycle commands>

## Environment Manifest
- OS: Ubuntu 22.04
- CUDA: <version>
- Python: <version>
- Framework: <PyTorch/TF/JAX version and CUDA variant>

## Smoke Test Result
- nvidia-smi: <output snippet>
- torch.cuda.is_available(): <True/False>
- Job launch command: <exact command>

## Checkpoint/Recovery
- Checkpoint path: <local and remote>
- Sync frequency: <e.g., every 1000 steps>
- Resume command: <example>

## Teardown Instruction
- Stop: `lambda-cloud stop <id>` or API
- Verify billing stopped
```

## Constraints

- Pin versions; avoid "latest" for CUDA-dependent stacks.
- Assume preemption or failure; require checkpointing.
- Prefer smallest viable GPU profile that meets throughput/latency targets.