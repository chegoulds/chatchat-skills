---
id: modal-serverless-gpu
name: Modal Serverless GPU
description: Design and run GPU workloads on Modal with serverless scaling, model packaging, and cost-aware execution controls. Use when deploying inference or batch AI pipelines that need burst GPU capacity.
category: DevOps
requires: []
examples:
  - Deploy a GPU inference endpoint with autoscaling on Modal.
  - Run batch embedding generation jobs with controlled GPU concurrency.
---

# Modal Serverless GPU

Deploy GPU-backed workloads on Modal while balancing latency, throughput, and cost.

## When to Use

- Inference or model processing needs GPU acceleration.
- Workload demand is bursty and does not justify always-on GPU servers.
- You need clear controls for model loading, scaling, and spend.

## Workflow

1. Characterize workload profile (latency-sensitive endpoint vs throughput batch jobs).
2. Select GPU type, memory profile, and concurrency settings for target SLAs.
3. Package model artifacts and optimize startup/caching behavior.
4. Implement autoscaling thresholds and timeout/retry policies.
5. Validate performance and cost with representative load tests.

## Modal Patterns

**@app.function decorators**: Use `@app.function(gpu="A10G", timeout=300)` for GPU functions. Specify `gpu="T4"`, `"A10G"`, `"A100"`, etc. Add `concurrency_limit=10` to cap parallel executions.

**Image.from_registry with model caching**: Build a custom image that pre-downloads model weights so cold starts avoid network fetch. Example: `image = modal.Image.debian_slim().run_commands("pip install transformers", "python -c 'from transformers import AutoModel; AutoModel.from_pretrained(\"model-id\")'")`. Attach with `image=image` on the function.

**Volume mounts for model weights**: Use `modal.Volume.from_name("model-cache")` and mount at a path. Persist downloaded models with `volume.commit()` after download. Reduces cold start time when the image does not bundle the model.

**keep_warm for latency-sensitive endpoints**: Add `keep_warm=1` (or higher) to maintain warm instances. Trade cost for lower P95 latency. Use only when traffic justifies it.

## Common Pitfalls

- **Cold start from unoptimized images**: Large base images or runtime model downloads cause 30ΓÇô60+ second cold starts. Fix: bake model into image or use a Volume with pre-populated weights.
- **Unbounded concurrency**: No `concurrency_limit` can spawn hundreds of GPU instances and blow the budget. Fix: set `concurrency_limit` and monitor with `modal app run` or dashboard.
- **Model not cached in image**: Every cold start re-downloads from Hugging Face or S3. Fix: use `Image.run_commands` or a Volume to cache; verify with a cold-start test.
- **Missing keep_warm for latency-sensitive endpoints**: First request pays full cold start. Fix: set `keep_warm=1` for APIs with strict latency SLAs; tune based on traffic.

## Modal CLI for Testing

- `modal run app.py` ΓÇö run a script or function once.
- `modal serve app.py` ΓÇö run a web endpoint locally for development.
- `modal deploy app.py` ΓÇö deploy to production.
- `modal app list` ΓÇö list deployed apps.
- `modal run app.py::batch_job` ΓÇö run a specific function. Use to validate model load and cold start before deploying.

## Output Format

```markdown
## Modal GPU Plan
- Workload type: <online inference/batch>
- Model artifacts: <source and size>
- GPU class: <type>
- Concurrency: <min/max>

## Function Configuration
- Decorator: @app.function(gpu=..., timeout=..., concurrency_limit=...)
- Image: <from_registry|custom with model cache>
- Volume: <yes|no> for model weights
- keep_warm: <0|1|N> for endpoints

## Pitfall Checklist
- [ ] Model cached in image or Volume
- [ ] concurrency_limit set
- [ ] keep_warm configured for low-latency endpoints
- [ ] Cold start measured

## Performance Targets
- P95 latency: <target>
- Throughput: <target>
- Max cost per run/request: <target>

## CLI Validation
- modal run: <command>
- modal serve: <command>

## Validation Checklist
- [ ] Model loads successfully
- [ ] Warm/cold start behavior measured
- [ ] Autoscaling thresholds tested
- [ ] Cost estimate documented
```

## Constraints

- Start with smallest viable GPU class, then scale up by evidence.
- Avoid unbounded concurrency; protect downstream dependencies.
- Track both latency and cost, not one in isolation.