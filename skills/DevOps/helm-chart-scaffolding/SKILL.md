---
id: helm-chart-scaffolding
name: Helm Chart Scaffolding
description: Scaffold production-ready Helm charts with sane defaults, environment overlays, and validation checks. Use when packaging Kubernetes workloads for repeatable deployments.
category: DevOps
requires: []
examples:
  - Scaffold a Helm chart for a stateless API with configurable image tags, probes, and autoscaling.
  - Add values overlays for dev/staging/prod and validate templates before release.
---

# Helm Chart Scaffolding

Scaffold Helm charts that are easy to deploy, override, and validate across environments.

## When to Use

- You are packaging a Kubernetes application into a reusable chart.
- You need clean `values` structure for multi-environment deployments.
- You want to reduce deployment drift and template errors.

## Workflow

1. **Chart.yaml**: Set `apiVersion: v2`, `name`, `version` (semver), `appVersion`, and `kubeVersion` when targeting a minimum cluster version. Use `dependencies` for subcharts; run `helm dependency update` before packaging.

2. **`_helpers.tpl` patterns**: Define reusable named templates for labels and selectors to ensure consistency:

```yaml
{{- define "mychart.labels" -}}
app.kubernetes.io/name: {{ include "mychart.name" . }}
app.kubernetes.io/instance: {{ .Release.Name }}
{{- end -}}

{{- define "mychart.selectorLabels" -}}
app.kubernetes.io/name: {{ include "mychart.name" . }}
app.kubernetes.io/instance: {{ .Release.Name }}
{{- end -}}
```

Reference in resources: `labels: {{- include "mychart.labels" . | nindent 4 }}` and `selector: matchLabels: {{- include "mychart.selectorLabels" . | nindent 6 }}`.

3. **Baseline templates**: Create `Deployment`/`StatefulSet`, `Service`, optional `Ingress`, and `ConfigMap`/`Secret` references. Use `include` for shared logic; avoid duplicating label blocks.

4. **values.yaml design**: Provide safe defaults. Use `image.tag` with a default; allow `image.tag` override per environment. Structure nested keys for clarity (e.g. `resources.requests`, `autoscaling.enabled`).

5. **values.schema.json**: Add JSON schema to validate values at `helm install`/`helm upgrade`:

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "properties": {
    "replicaCount": { "type": "integer", "minimum": 1 },
    "image": {
      "type": "object",
      "required": ["repository"],
      "properties": {
        "repository": { "type": "string" },
        "tag": { "type": "string", "default": "latest" }
      }
    }
  }
}
```

6. **Environment overlays**: Use `values-dev.yaml`, `values-staging.yaml`, `values-prod.yaml` for overrides. Apply with `-f values.yaml -f values-prod.yaml`.

7. **Probes and resources**: Always include `livenessProbe` and `readinessProbe`. Set `resources.requests` and `resources.limits`. Add `PodDisruptionBudget` for production.

## Common Pitfalls

- **Hardcoded image tags**: Use `{{ .Values.image.tag | default .Chart.AppVersion }}` so tags are overridable. Never hardcode `:latest` in templates.
- **Missing resource limits**: Omit `resources.limits` at your peril; set both requests and limits to avoid noisy-neighbor and OOM issues.
- **No PodDisruptionBudget**: Production workloads should have a PDB to control voluntary disruption during node drains.
- **Inconsistent label selectors**: If Deployment labels change between upgrades, selectors may not match existing Pods. Use stable `selectorLabels` from helpers; never change selector keys after first release.
- **Over-templating values**: Avoid deep conditionals and complex logic in templates. Prefer simple `{{ .Values.foo }}`; push complexity into values files.

## Testing

- **helm unittest**: Add `tests/` directory with `*_test.yaml` files. Run `helm unittest .` to validate template logic and expected output.
- **ct (chart-testing)**: Use `ct lint-and-install` to lint against best practices and optionally install into a cluster. Configure `ct.yaml` with chart paths and namespace.

## Recommended Validation

- `helm lint <chart>`
- `helm template <release> <chart> -f values.yaml`
- `helm template <release> <chart> -f values-prod.yaml`
- `helm unittest .` (if tests exist)
- Optional dry-run install to a test namespace.

## Output Format

Return:

- Chart structure summary (files and purpose)
- Values model (defaults vs environment overrides) with key paths documented
- Example `_helpers.tpl` snippets for labels/selectors
- Render/validation commands and expected outcomes
- Testing commands (`helm unittest`, `ct lint-and-install` if applicable)
- Known risks or TODOs before production rollout (e.g. missing PDB, unvalidated values)

## Constraints

- Avoid embedding environment-specific constants in templates.
- Keep secrets external or referenced via `existingSecret`; never hardcode in values.
- Prefer explicit values over hidden template logic.
- Keep template names and selector labels stable to avoid upgrade drift.
- Always include `values.schema.json` for production charts.