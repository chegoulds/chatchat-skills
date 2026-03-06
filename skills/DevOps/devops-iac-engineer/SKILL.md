---
id: devops-iac-engineer
name: DevOps IaC Engineer
description: Design and review Infrastructure as Code changes for reliability, security, drift control, and safe environment promotion.
category: DevOps
requires: []
examples:
  - Review this IaC plan for risk before applying to production.
  - Create an IaC rollout strategy across dev, staging, and production.
---

# DevOps IaC Engineer

Plan and validate Infrastructure as Code workflows with policy and safety guardrails.

## When to Use

- You are creating or modifying Terraform/Pulumi/CloudFormation-style infrastructure definitions.
- You need to prevent drift, unsafe changes, or privilege misconfiguration.
- You need a controlled rollout and verification sequence.

## Workflow

1. **Define intended infrastructure change and blast radius.** Identify resources to create, modify, or destroy. Use module composition patterns (call modules from root, avoid monolithic root modules) to limit scope. For brownfield resources, use `terraform import` or equivalent to bring existing infrastructure under IaC control before changing it.

2. **Review state dependencies and potential drift impacts.** Ensure backend state locking is configured (e.g., DynamoDB for Terraform, S3 with versioning). Run `terraform plan -out=tfplan` (or equivalent) and persist the plan artifact for audit and apply. Never apply without a saved plan output; always review the plan diff before apply.

3. **Add policy/security checks before apply.** Run tfsec, checkov, or OPA/Conftest against the plan or configuration. Integrate these into CI so policy failures block apply. Address findings for IAM policies (avoid overly broad `*` permissions), network exposure, and secrets handling.

4. **Stage changes by environment with explicit approval gates.** Promote dev ΓåÆ staging ΓåÆ prod only after validation. Use workspace or environment-specific variables. Require manual approval for production applies.

5. **Validate post-apply state and document rollback path.** Run `terraform state list` and compare with expected resources. Check for state drift between runs; investigate and reconcile before next change. Document rollback steps (e.g., revert plan, re-apply previous state).

## Common Pitfalls

- **Applying without plan output.** Always use `terraform plan -out=tfplan` and `terraform apply tfplan` so the exact plan is applied, not a re-generated one that may differ.
- **Ignoring state drift between runs.** Drift causes unexpected destroys or conflicts. Run periodic `terraform plan` in read-only mode and fix drift before applying new changes.
- **Overly broad IAM policies in IaC.** Avoid `"*"` on actions or resources. Use least-privilege; scope policies to specific ARNs and actions.
- **Monolithic root modules.** Large root modules make changes risky and slow. Use composable modules; keep root thin and delegate to modules.
- **Missing backend locking.** Without state locking, concurrent applies can corrupt state. Use S3+DynamoDB (Terraform), or equivalent locking for your tool.

## Risk Classification

Classify each change before apply:

- **Data-destructive:** Drops, truncates, or replaces resources that hold data (DBs, volumes). Requires backup verification and explicit approval.
- **Permission-escalating:** Adds IAM roles, policies, or broadens existing permissions. High security risk; require security review.
- **Network-opening:** New ingress rules, public IPs, or exposed ports. Increases attack surface; validate necessity.
- **Stateful resource replacement:** Recreates resources with `ForceNew` (e.g., instance type change). May cause downtime or data loss; plan maintenance windows.

## Concrete Commands

**Terraform:**
```bash
terraform init -backend-config=backend.hcl
terraform plan -out=tfplan -var-file=env.tfvars
tfsec .  # or checkov -d .
terraform apply tfplan
terraform state list
```

**Pulumi:**
```bash
pulumi stack select dev
pulumi preview --diff
pulumi up --yes  # or omit --yes for interactive
pulumi stack export
```

**CloudFormation:**
```bash
aws cloudformation validate-template --template-body file://template.yaml
aws cloudformation create-change-set --change-set-name pre-apply --template-body file://template.yaml
aws cloudformation describe-change-set --change-set-name pre-apply
aws cloudformation execute-change-set --change-set-name pre-apply
```

## Output Format

```markdown
## Change Scope
- Resources affected: <list with create/modify/destroy>
- Environment path: <dev -> staging -> prod>
- Module/stack: <root or module path>

## Risk Classification
- [ ] Data-destructive | [ ] Permission-escalating | [ ] Network-opening | [ ] Stateful replacement
- Risk level: low | medium | high

## Risk Review
- Security concerns: <list>
- Availability concerns: <list>
- Drift status: <known drift or none>

## Apply Plan
- [ ] Plan saved (plan -out or equivalent)
- [ ] Policy checks pass (tfsec/checkov/OPA)
- [ ] Approval gate satisfied
- [ ] Backend locking confirmed

## Post-Apply Verification
- [ ] State is consistent
- [ ] Health checks pass
- [ ] Drift check completed
- [ ] Rollback path documented
```

## Constraints

- Prefer small, reversible infrastructure changes.
- Avoid production-first apply paths.
- Treat state and permission changes as high risk by default.
- Never apply without a persisted plan artifact.
- Always run policy checks (tfsec, checkov, or OPA) before apply.
- Use backend state locking for all environments.