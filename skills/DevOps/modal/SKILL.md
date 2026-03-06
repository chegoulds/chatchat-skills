---
id: modal
name: Modal
description: Build and deploy workloads on Modal using images, functions, schedules, and scalable execution patterns. Use when implementing Python-first serverless jobs, APIs, or batch tasks on Modal.
category: DevOps
requires: []
examples:
  - Deploy a scheduled data processing job on Modal.
  - Package dependencies and expose a simple API-backed function.
---

# Modal

Implement Modal workflows with repeatable setup, deployment, and validation.

## When to Use

- A workload fits serverless execution with Python and dynamic scaling.
- You want less platform overhead than managing full container orchestration.
- Jobs, APIs, or batch pipelines need fast deployment cycles.

## Modal Patterns

- **@app.function with cpu/memory/timeout**: `@app.function(cpu=2, memory=4096, timeout=3600)` ΓÇö Set resources explicitly. Use `timeout` to avoid runaway jobs.
- **Image.debian_slim().pip_install()**: `image = modal.Image.debian_slim().pip_install("torch", "requests")` ΓÇö Build minimal images. Add system deps with `.apt_install()` if needed.
- **@app.cls for stateful services**: Use `@app.cls` for long-lived processes (e.g., model servers). Lifecycle: `__enter__`/`__exit__` for setup/teardown.
- **Cron schedules**: `@app.function(schedule=modal.Cron("0 * * * *"))` ΓÇö Hourly. Use `modal.Period` for interval-based runs.
- **web_endpoint decorator**: `@app.function()` + `@modal.web_endpoint()` or `@modal.web_server(8000)` ΓÇö Expose HTTP endpoints. Use for APIs and webhooks.

## Common Pitfalls

- **Large image rebuilds from poor layer ordering**: Put rarely-changing deps first (base image, system packages), then `pip_install`. Use `.copy_local_dir()` for app code last. Leverage Modal's layer caching.
- **Missing secrets in deployment**: Add secrets via `modal secret create` or dashboard. Reference in functions: `@app.function(secrets=[modal.Secret.from_name("my-secret")])`. Verify secrets exist before deploy.
- **No retry on transient failures**: Use `@app.function(retries=modal.Retries(max_retries=3, backoff_coefficient=2))` for network or throttling errors.
- **Cold start from unoptimized images**: Large images and heavy imports delay first invocation. Use `image.run_commands()` to pre-warm; keep top-level imports minimal.

## Modal CLI Commands and Deployment

- `modal run app.py` ΓÇö Run app locally (functions execute on Modal)
- `modal deploy app.py` ΓÇö Deploy app for production (schedules, web endpoints)
- `modal shell` ΓÇö Interactive shell in Modal environment
- `modal secret create my-secret KEY=value` ΓÇö Create secret
- `modal app list` ΓÇö List deployed apps
- `modal token new` ΓÇö Create API token for CI/CD

Deploy pattern: `modal deploy app.py` registers the app; Cron and web endpoints become active. Use `modal run app.py::function_name` for one-off invocations.

## Workflow

1. Define workload type (job, API endpoint, scheduled task, queue consumer).
2. Specify runtime image and dependency model for reproducible execution.
3. Implement Modal functions with clear resource and timeout settings.
4. Configure deployment, secrets, and environment separation.
5. Validate with smoke tests, logs, and rollback-safe release steps.

## Output Format

```markdown
## Modal Deployment Plan
- App purpose: <job/api/batch>
- Entry points: <functions with decorators>
- Runtime image: <base, pip_install, apt_install, copy steps>
- Resource profile: <cpu/memory/timeout per function>
- Schedules: <cron or period if applicable>
- Web endpoints: <paths if applicable>

## Code Snippets
- Key @app.function, Image, and decorator patterns

## Deployment Checklist
- [ ] Secrets configured and referenced
- [ ] Environment-specific config applied
- [ ] Retries configured for transient failures
- [ ] Smoke test passed
- [ ] Observability/logging verified
- [ ] Image layer order optimized

## CLI Commands
- Deploy: `modal deploy app.py`
- Run one-off: `modal run app.py::function_name`
- Secret: `modal secret create <name> KEY=value`

## Operations Notes
- Trigger model: <event/schedule/manual>
- Retry policy: <strategy and backoff>
- Rollback path: <redeploy previous version, disable schedule>
- Cold start mitigation: <image optimization, pre-warm>
```

## Constraints

- Keep images lean to reduce cold start and build time.
- Explicitly define retries and idempotency for non-read operations.
- Separate dev/staging/prod configurations to avoid accidental cross-impact.