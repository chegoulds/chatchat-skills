---
id: environment-variables
name: Environment Variables
description: Manage environment variables safely across local, staging, and production environments with validation and secret-handling best practices.
category: DevOps
requires: []
examples:
  - Help me set up environment variables for local, staging, and production safely.
  - Create an env variable validation checklist before deploying this service.
---

# Environment Variables

Define, validate, and maintain environment variables with clear separation by environment and risk level.

## When to Use

- You are introducing or changing configuration via environment variables.
- You need a secure method to handle secrets and required config.
- You want to prevent runtime failures caused by missing or invalid values.

## Workflow

1. Enumerate required variables and classify each as secret or non-secret.
2. Define source of truth per environment (local/staging/production).
3. Validate naming consistency and default value policy.
4. Add startup validation for required variables and format checks.
5. Document rotation and rollback steps for secret values.

## Validation Patterns

Use a schema-based validator at startup so failures surface immediately, not deep in request handling. With Zod:

```typescript
import { z } from "zod";

const envSchema = z.object({
  NODE_ENV: z.enum(["development", "staging", "production"]),
  DATABASE_URL: z.string().url(),
  API_KEY: z.string().min(32),
  PORT: z.coerce.number().min(1).max(65535).default(3000),
});

const env = envSchema.parse(process.env);
```

With Joi:

```javascript
const Joi = require("joi");
const envSchema = Joi.object({
  NODE_ENV: Joi.string().valid("development", "staging", "production").required(),
  DATABASE_URL: Joi.string().uri().required(),
  API_KEY: Joi.string().min(32).required(),
  PORT: Joi.number().port().default(3000),
}).options({ stripUnknown: true });

const { value: env, error } = envSchema.validate(process.env);
if (error) throw new Error(`Invalid env: ${error.message}`);
```

Treat `.env.example` as the contract: list every variable with a placeholder and short description. Never commit `.env`. Use dotenv hierarchy: `.env` (base) ΓåÆ `.env.local` (local overrides, gitignored) ΓåÆ `.env.<environment>` for staging/production. Load in that order so higher-priority files override lower ones.

## Common Pitfalls

- **Missing required vars discovered at runtime**: A request hits a code path that needs `REDIS_URL`; the app never validated it at startup. Fix: validate all required vars in one place before the app accepts traffic.
- **Inconsistent naming across services**: One service uses `DB_HOST`, another `database_host`, another `DATABASE_HOST`. Fix: adopt a single convention (e.g., SCREAMING_SNAKE_CASE, prefixed by domain) and enforce via lint or schema.
- **Committing `.env` files**: Secrets end up in git history. Fix: add `.env`, `.env.local`, `.env.*.local` to `.gitignore`; use `.env.example` with placeholders only.
- **Default values masking misconfig**: Defaulting `DATABASE_URL` to `localhost` hides the fact production never received the real URL. Fix: no defaults for required production vars; fail fast if absent.

## Output Format

```markdown
## Variable Inventory
- <VAR_NAME>: <purpose> (<secret|non-secret>) [required|optional]

## Environment Mapping
- Local: <source>
- Staging: <source>
- Production: <source>

## Validation Rules
- [ ] Required variables are present
- [ ] Value format checks defined
- [ ] No secrets committed to repo

## Schema Snippet
- Validator: <zod|joi|other>
- Required vars: <list>
- Format checks: <URL, enum, port, etc.>

## .env.example Contract
- <VAR_NAME>=<placeholder> # <description>

## Common Pitfalls Addressed
- [ ] Startup validation blocks traffic until env valid
- [ ] Naming convention documented and consistent
- [ ] .env* in .gitignore, .env.example committed
- [ ] No silent defaults for required production vars

## Operational Notes
- Rotation: <how to rotate>
- Rollback: <fallback behavior>
```

## Constraints

- Never expose secret values in outputs.
- Prefer explicit validation failures over silent defaults for required variables.
- Keep environment naming and variable conventions consistent across services.