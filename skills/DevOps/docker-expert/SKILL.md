---
id: docker-expert
name: Docker Expert
description: Build production-ready Docker images and Compose setups with reproducible builds, secure defaults, and efficient caching. Use when containerizing services or debugging container runtime issues.
category: DevOps
requires: []
examples:
  - Create a multi-stage Dockerfile for a Node.js API with minimal runtime image size and non-root execution.
  - Diagnose why a container works locally but fails in CI due to missing build context files.
---

# Docker Expert

Create containers that are small, secure, and predictable across local and CI environments.

## When to Use

- You are creating or hardening a `Dockerfile`.
- Builds are slow, non-reproducible, or failing in CI.
- Runtime behavior differs between local machine and container.

## Workflow

1. **Base image selection**: Prefer `alpine` for minimal size when musl compatibility is acceptable; use `distroless` for maximum security (no shell, minimal attack surface); use `debian-slim` when glibc or common tools are required. Pin major.minor version (e.g. `node:20-alpine3.19`) and avoid `latest`.

2. **Multi-stage builds**: Use a builder stage for compilation and a minimal runtime stage. Copy only artifacts with `COPY --from=builder`:

```dockerfile
FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci --omit=dev
COPY . .
RUN npm run build

FROM node:20-alpine
RUN addgroup -g 1000 app && adduser -u 1000 -G app -D app
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/node_modules ./node_modules
USER app
CMD ["node", "dist/index.js"]
```

3. **Layer ordering**: Copy dependency manifests (`package.json`, `requirements.txt`, `go.mod`) before source code. Install dependencies in a separate RUN. This maximizes cache reuse when only source changes.

4. **BuildKit cache mounts**: For package managers, use cache mounts to speed repeated builds:

```dockerfile
RUN --mount=type=cache,target=/root/.npm \
    npm ci --omit=dev
```

5. **Configure `.dockerignore`**: Exclude `node_modules`, `.git`, `.env*`, `*.log`, and build artifacts. Missing entries bloat context and can leak secrets.

6. **Non-root execution**: Create a dedicated user, set `USER`, and use explicit `WORKDIR`. Prefer `CMD` with exec form `["executable", "arg1"]` over shell form for proper signal handling.

7. **Compose**: Define `depends_on` with `condition: service_healthy` where applicable. Use env files for overrides, not hardcoded values.

## Common Pitfalls

- **apt-get without cleanup**: Always combine `apt-get update && apt-get install -y ... && rm -rf /var/lib/apt/lists/*` in one RUN to avoid bloating layers.
- **COPY . before dependency install**: Copying source before `npm install` or `pip install` invalidates cache on every code change. Copy manifests first, install, then copy source.
- **Missing `.dockerignore`**: Without it, `node_modules`, `.git`, and local `.env` can be sent as build context, causing slow builds and potential secret leakage.
- **ENTRYPOINT without signal handling**: Use exec form so PID 1 receives SIGTERM. Wrap scripts with `exec` or use `tini`/`dumb-init` if a shell script is required.
- **ADD vs COPY**: Prefer `COPY`; `ADD` has URL and archive-extraction behavior that can surprise. Use `COPY` for deterministic, explicit behavior.

## Security Hardening

- **Scan images**: Run `trivy image <image>` or `grype <image>` in CI. Fix critical/high CVEs before release.
- **Read-only root filesystem**: Add `securityContext: { readOnlyRootFilesystem: true }` in Kubernetes; ensure writable paths use tmpfs or volumes.
- **Drop capabilities**: Run with `--cap-drop=ALL` and add back only what is needed. Avoid `--privileged`.
- **Minimal base**: Prefer distroless or alpine. Fewer packages mean fewer vulnerabilities.

## Quality Checklist

- [ ] Final image excludes build tools and caches.
- [ ] Secrets are not baked into image layers.
- [ ] Ports, volumes, and env vars are explicitly documented.
- [ ] Image runs with least privilege.

## Output Format

Return:

- Recommended `Dockerfile`/Compose changes with inline examples where helpful
- Build commands: `docker build -t <tag> .` and verification: `docker run --rm <tag>`
- Security scan commands: `trivy image <tag>` or equivalent
- Performance and security risks identified
- Follow-up hardening tasks (scan results, capability drops, read-only FS)

## Constraints

- Avoid `latest` tags in production workflows.
- Prefer deterministic lockfile-based installs (`npm ci`, `pip install -r requirements.txt` with pinned versions).
- Do not rely on host-specific paths or tooling assumptions.
- Do not use `ADD` when `COPY` suffices.
- Ensure entrypoint/CMD use exec form for proper signal propagation.