---
id: nginx-to-highgress-migration
name: Nginx to Highgress Migration
description: Migrate NGINX ingress or reverse proxy configurations to Highgress with route parity, policy mapping, and rollout safety checks. Use when modernizing gateway infrastructure or consolidating traffic management.
category: DevOps
requires: []
examples:
  - Convert NGINX location blocks into Highgress route definitions.
  - Plan a zero-downtime migration from NGINX ingress to Highgress.
---

# Nginx to Highgress Migration

Move from NGINX to Highgress with clear mapping, staged rollout, and validation.

## When to Use

- Existing traffic rules in NGINX must be replicated in Highgress.
- You need safer rollout steps than a single cutover.
- TLS, auth, or rate-limit behavior must remain consistent after migration.

## Workflow

1. Inventory current NGINX config (routes, rewrites, headers, TLS, auth, rate limits).
2. Map each behavior to Highgress resources and identify unsupported directives.
3. Build equivalent Highgress config for non-production and run route parity tests.
4. Deploy in shadow/canary mode and compare responses, latency, and error rates.
5. Cut over gradually, keep rollback artifacts ready, and validate post-cutover metrics.

## NGINX-to-Highgress Mapping

| NGINX Directive | Highgress Equivalent | Notes |
|-----------------|----------------------|-------|
| `location /api` | Route with path match `/api` | Use path prefix or exact match |
| `rewrite ^/old(.*)$ /new$1` | Route rewrite or path rewrite policy | Verify capture groups and order |
| `proxy_pass http://upstream` | Backend reference / service | Map upstream blocks to backends |
| `proxy_set_header X-Forwarded-For` | Header manipulation policy | Add/modify headers per route |
| `ssl_certificate` / `ssl_certificate_key` | TLS secret / cert reference | TLS termination config differs |
| `limit_req zone=one burst=5` | Rate limit policy | Map zone + burst to Highgress rate limit |
| `auth_basic` | Auth policy (OIDC/JWT/basic) | May require different auth backend |
| `proxy_redirect` | Redirect policy | Explicit redirect rules |

Build a compatibility matrix for your config: list each NGINX block, its purpose, and the Highgress resource that achieves it. Flag any directive with no direct mapping for manual handling.

## Common Pitfalls

- **Missing rewrite semantics**: NGINX `rewrite` with `last`, `break`, or `redirect` behaves differently. Highgress may use path rewrites or redirect policies. Test that URLs and redirects match exactly.
- **TLS termination differences**: NGINX may terminate TLS with specific ciphers or client cert validation. Ensure Highgress TLS config (SNI, cert chain, client auth) matches.
- **Rate limit translation**: NGINX `limit_req` zones and burst do not map 1:1. Verify requests-per-second and burst behavior under load.
- **Header manipulation gaps**: `proxy_set_header`, `add_header`, and conditional headers can be easy to miss. Audit all header changes and replicate in Highgress policies.

## Output Format

```markdown
## Migration Mapping
- Source NGINX config: <path>
- Target Highgress config: <path>
- Unmapped directives: <list or none>

## Compatibility Matrix
| NGINX Block | Directive | Highgress Resource | Status |
|-------------|-----------|-------------------|--------|
| location /api | proxy_pass, rewrite | Route + backend + rewrite | mapped |
| server | ssl_* | TLS config | verify |

## Route Parity Table
| Route | NGINX Behavior | Highgress Behavior | Status |
|---|---|---|---|
| /api/* | ... | ... | pass/fail |

## Pitfall Checklist
- [ ] Rewrite semantics verified (redirect vs internal)
- [ ] TLS termination parity (certs, SNI, client auth)
- [ ] Rate limits tested under load
- [ ] All header manipulations replicated

## Rollout Plan
1. Stage: <dev/staging/prod>
2. Traffic split: <5/25/50/100>
3. Observability checks: <latency, 4xx, 5xx>
4. Rollback trigger: <thresholds>
```

## Constraints

- Preserve security behavior (TLS, authn/authz) before tuning performance.
- Do not cut over without parity validation for critical routes.
- Keep rollback path active until stability window is complete.