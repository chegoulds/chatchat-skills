---
id: netalertx-devcontainer-configs
name: NetAlertX Devcontainer Configs
description: Configure and maintain NetAlertX devcontainer setups for consistent local development across contributors. Use when onboarding developers, standardizing toolchains, or debugging environment drift.
category: DevOps
requires: []
examples:
  - Build a devcontainer that includes NetAlertX runtime dependencies.
  - Align local setup across macOS, Linux, and Windows contributors.
---

# NetAlertX Devcontainer Configs

Define a reliable devcontainer workflow so contributors get the same environment on first run.

## When to Use

- Team members have setup issues caused by differing local environments.
- Onboarding takes too long due to manual dependency installation.
- You need reproducible debugging across machines and CI-like behavior locally.

## Workflow

1. Identify required runtimes, packages, ports, and background services.
2. Configure `devcontainer.json` with extensions, mounts, env vars, and post-create steps.
3. Add Dockerfile/custom image layers for NetAlertX-specific dependencies.
4. Validate startup on clean machines and confirm app health checks pass.
5. Document rebuild/update steps and version pinning strategy.

## devcontainer.json Patterns

**features**: Add pre-built tooling without custom Dockerfile layers. Example: `"ghcr.io/devcontainers/features/docker-in-docker:2"` or `"ghcr.io/devcontainers/features/git:1"`. Pin versions to avoid drift.

**forwardPorts**: Expose app ports so the host can reach them. Example: `"forwardPorts": [3000, 5432]`. Use `portsAttributes` for labels and default visibility.

**postCreateCommand**: Runs once after container creation. Chain multiple commands with `&&` or use an array. Prefer idempotent scripts: `pip install -r requirements.txt` is safe; `pip install package` without `--upgrade` or version pin can behave differently on rebuild.

**VS Code extensions**: List in `customizations.vscode.extensions`. Example: `"ms-python.python"`, `"ms-python.debugpy"`. Pin versions when stability matters: `"ms-python.python@2024.1.0"`.

**mounts**: Persist data across rebuilds. Example: `"source=${localWorkspaceFolder}/.devcontainer-data,target=/workspace/data,type=bind"`. Use for databases, caches, or model weights that should survive container recreation.

## Common Pitfalls

- **Non-idempotent postCreate scripts**: Scripts that assume a clean state or run `rm -rf` can break on partial rebuilds. Fix: make commands safe to run multiple times; use `--force` or check-before-write patterns.
- **Missing volume mounts for data persistence**: SQLite DBs, downloaded models, or caches live only in the container; rebuild wipes them. Fix: mount a host or named volume for persistent data.
- **Platform-specific Docker socket paths**: `/var/run/docker.sock` on Linux may differ on macOS (Docker Desktop) or Windows. Fix: use `"mounts": ["source=/var/run/docker.sock,target=/var/run/docker.sock"]` and document Docker Desktop requirements for non-Linux hosts.

## Cross-Platform Compatibility

Test on macOS, Linux, and Windows (WSL2). Use environment variables for paths when needed: `"${localWorkspaceFolder}"` works across platforms. Avoid hardcoded `/tmp` or `/home` assumptions. For Docker-in-Docker, ensure Docker Desktop is running on macOS/Windows before build. Document any host prerequisites (Docker, WSL2, disk space) in a README.

## Output Format

```markdown
## Devcontainer Specification
- Base image: <image>
- Required tools: <list>
- Exposed ports: <list>
- Volumes/mounts: <list>

## features
- <feature-id>: <version or latest>

## forwardPorts and portsAttributes
- Ports: <list>
- Attributes: <visibility, label>

## postCreateCommand Chain
1. <command 1>
2. <command 2>
3. <health check command>

## Pitfall Checklist
- [ ] postCreateCommand is idempotent
- [ ] Data persistence mounts configured
- [ ] Docker socket path documented for macOS/Windows

## Cross-Platform Notes
- Linux: <notes>
- macOS: <notes>
- Windows/WSL2: <notes>

## Validation Checklist
- [ ] Container builds from scratch
- [ ] NetAlertX starts successfully
- [ ] Debug/test tools are available
- [ ] Rebuild instructions are documented
```

## Constraints

- Prefer pinned versions for core tooling to reduce drift.
- Keep startup scripts idempotent; container rebuilds must be safe.
- Avoid storing secrets directly in committed devcontainer files.