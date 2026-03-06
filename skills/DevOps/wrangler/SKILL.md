---
id: wrangler
name: Wrangler
description: Build and deploy Cloudflare Workers with Wrangler, covering local iteration, environment configuration, and release verification.
category: DevOps
requires: []
examples:
  - Create a Wrangler release workflow for a Worker with staging and production environments.
  - Troubleshoot a failed `wrangler deploy` and identify the likely configuration issue.
---

# Wrangler

Plan and execute reliable Worker deployments with Wrangler.

## When to Use

- You need to initialize, configure, or deploy a Cloudflare Worker.
- You need a repeatable process for environment-specific `wrangler.toml` settings.
- You are debugging deploy, route, binding, or secret configuration errors.

## Workflow

1. **Confirm runtime target, environments, and required bindings.** Identify KV namespaces, R2 buckets, D1 databases, and secrets. Map each binding to the correct environmentΓÇöKV namespace IDs differ per environment.

2. **Validate wrangler.toml structure.** Ensure `compatibility_date` is set (required for Workers); without it, deploys may fail or behave unpredictably. Use `compatibility_flags` for opt-in features. Define routes or `workers_dev` for development. Use `[env.<name>]` sections for staging and production overrides.

3. **Local iteration with wrangler dev.** Run `wrangler dev` for local development with live bindings (KV, R2, etc.) when configured. Test routes and bindings before deploying.

4. **Production debugging with wrangler tail.** After deploy, use `wrangler tail` to stream real-time logs from production. Filter by status code or sampling rate as needed.

5. **Deploy with environment targeting.** Use `wrangler deploy` for default env, or `wrangler deploy --env production` for named envs. Verify secrets are set per environment: `wrangler secret put SECRET_NAME --env production`.

6. **Post-deploy verification.** Confirm routes resolve, health endpoints respond, and bindings (KV, R2, D1, secrets) work. Check for route conflicts (overlapping patterns or duplicate routes across workers).

7. **Capture follow-up actions.** Document any configuration changes, new secrets, or monitoring setup.

## Common Pitfalls

- **Forgetting to set compatibility_date.** Workers require `compatibility_date` in `wrangler.toml`. Omitting it can cause deploy failures or undefined runtime behavior.
- **Deploying to wrong environment.** Using `wrangler deploy` without `--env` deploys to the default config. Always specify `--env production` when targeting production.
- **KV namespace ID mismatches between environments.** Staging and production use different KV namespace IDs. Ensure `wrangler.toml` env sections reference the correct `id` for each environment.
- **Missing secret bindings after deploy.** Secrets are not in `wrangler.toml`; they must be set via `wrangler secret put` per environment. A new Worker or new env may have no secrets until explicitly set.
- **Route conflicts.** Overlapping routes (e.g., `example.com/*` and `example.com/api/*`) or duplicate routes across workers can cause unexpected routing. Resolve conflicts in the Cloudflare dashboard or by adjusting route patterns.

## wrangler.toml Patterns and CLI

```toml
name = "my-worker"
main = "src/index.js"
compatibility_date = "2024-01-01"

[vars]
ENVIRONMENT = "development"

[[kv_namespaces]]
binding = "MY_KV"
id = "abc123"

[env.staging]
name = "my-worker-staging"
vars = { ENVIRONMENT = "staging" }
[[env.staging.kv_namespaces]]
binding = "MY_KV"
id = "def456"

[env.production]
name = "my-worker"
vars = { ENVIRONMENT = "production" }
routes = [{ pattern = "api.example.com/*", zone_name = "example.com" }]
[[env.production.kv_namespaces]]
binding = "MY_KV"
id = "ghi789"
```

CLI commands:

```bash
wrangler init
wrangler dev
wrangler deploy
wrangler deploy --env production
wrangler tail
wrangler secret put SECRET_NAME --env production
wrangler kv:key list --namespace-id=<id>
```

## Output Format

```markdown
## Deployment Plan
- Worker: <name>
- Environments: <dev/staging/prod>
- Bindings: <kv/r2/d1/secrets>
- compatibility_date: <date>

## Commands
1. wrangler dev                    # Local test
2. wrangler deploy --env staging   # Staging deploy
3. wrangler secret put X --env staging  # If new secrets
4. wrangler deploy --env production
5. wrangler tail                   # Post-deploy log stream

## Preflight Checklist
- [ ] compatibility_date set in wrangler.toml
- [ ] KV/R2/D1 IDs correct per environment
- [ ] Secrets configured for target env
- [ ] Routes do not conflict

## Verification
- [ ] Health endpoint responds
- [ ] Routes map correctly
- [ ] Secrets and bindings resolve
- [ ] wrangler tail shows expected traffic
```

## Constraints

- Use only documented Wrangler and Cloudflare Worker features.
- Do not expose secrets or account identifiers in examples.
- Keep steps deterministic and environment-specific.
- Always set compatibility_date in wrangler.toml.
- Verify binding IDs and secrets per environment before deploy.