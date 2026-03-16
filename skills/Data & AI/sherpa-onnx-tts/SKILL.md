---
id: sherpa-onnx-tts
name: Sherpa ONNX TTS
description: Build, package, and troubleshoot Sherpa ONNX text-to-speech pipelines across local and CI environments. Use when integrating model assets, validating runtime output quality, or fixing deployment/runtime failures.
category: Data & AI
requires: [".onnx"]
examples:
  - "Package Sherpa ONNX TTS model files and app runtime into a reproducible container image."
  - "Debug missing model artifacts or runtime crashes in a TTS deployment pipeline."
---

# Sherpa ONNX TTS

Operate Sherpa ONNX TTS integrations with reproducible builds, predictable runtime behavior, and clear validation gates.

## When to Use

- You are deploying or updating Sherpa ONNX TTS in development, staging, or production.
- You need to verify model/artifact packaging and runtime inference output.
- You are investigating startup errors, missing assets, or audio quality regressions.

## Workflow

1. Identify target runtime (local binary, container, serverless, or edge).
2. Validate model asset set and paths:
   - Encoder/decoder/token files are present and version-matched.
   - Runtime points to explicit, environment-safe paths.
3. Pin runtime dependencies and build inputs for reproducibility.
4. Run a smoke test with fixed sample text and capture output artifacts/logs.
5. Add CI checks for artifact presence, startup health, and basic synthesis success.
6. Record expected latency, memory envelope, and output sample rate for regression checks.
7. Promote only after smoke test and configuration checks pass.

## Concrete TTS Pipeline Patterns

**Model asset validation with checksums.** Before packaging or deploying, compute SHA-256 (or equivalent) for each model file. Store checksums in a manifest. At runtime or in CI, verify files against the manifest. Reject deployment if any checksum mismatches. This catches corrupted downloads, partial copies, or wrong-version substitutions.

**Container packaging for model + runtime.** Use a multi-stage Dockerfile: stage 1 fetches and validates model assets, stage 2 builds the runtime, stage 3 copies only validated assets and runtime into the final image. Use explicit COPY paths and avoid wildcards. Pin base image and dependency versions. Document the model version in image labels or env vars.

**ONNX runtime configuration tuning.** Set `intra_op_num_threads` and `inter_op_num_threads` based on target CPU cores. For low-latency use cases, consider `ExecutionMode.ORT_PARALLEL`. Disable unused providers (e.g., CUDA if CPU-only) to reduce startup time. Record the configuration used for each benchmark.

**Audio quality benchmarking against reference samples.** Generate audio from a fixed set of test phrases. Compare against reference samples using objective metrics (e.g., PESQ, STOI if available) or at minimum a manual listen checklist. Store reference samples in version control. Regressions in quality must block release.

## Common Pitfalls

**Model/runtime version mismatch.** Deploying a model trained or exported for one Sherpa ONNX version with a different runtime. Always pin both model and runtime versions. Document compatibility matrix. Test upgrades in staging before production.

**Implicit path assumptions in production.** Using relative paths, `~`, or hardcoded dev paths. Production environments have different layouts. Use environment variables (e.g., `MODEL_DIR`) or config files with explicit, absolute-style paths. Validate paths at startup and fail fast if missing.

**Skipping synthesis quality check.** Assuming "no crash" means "good output." Audio can be distorted, truncated, or silent. Always run at least one synthesis and verify output: non-empty file, expected duration, listenable quality. Add this to CI.

**Large model files without caching strategy.** Pulling multi-GB models on every deploy or container build. Use layer caching, external model storage (S3, GCS), or init containers that populate a volume. Avoid baking models into images if they change frequently unless you have a clear versioning strategy.

**Missing latency benchmarks.** Deploying without baseline latency numbers. Establish p50, p95, p99 latency and memory usage for representative inputs. Use these for regression detection and capacity planning.

## Output Format

Return:

```markdown
## Environment and Deployment Target
- Runtime: <local|container|serverless|edge>
- Sherpa ONNX version: <version>
- Model: <name, version, source>

## Artifact and Dependency Validation
| Asset | Path | Checksum | Status |
|-------|------|----------|--------|
| encoder.onnx | <path> | <sha256> | ok |
| decoder.onnx | <path> | <sha256> | ok |

## Smoke Test Evidence
- Sample text: "<fixed phrase>"
- Output: <path or inline>
- Pass/fail: <pass|fail>
- Logs: <summary or link>

## Benchmarks
- Latency (p50/p95): <ms>
- Memory: <MB>
- Sample rate: <Hz>

## Risks and Follow-up
- <risk 1>
- <risk 2>
- Recommended next step: <action>
```

## Constraints

- Do not rely on implicit relative paths for model files in production.
- Keep model/runtime versions pinned to avoid silent behavior drift.
- Do not mark deployment successful without synthesis verification.
- Treat audio quality checks as required, not optional, before release.
