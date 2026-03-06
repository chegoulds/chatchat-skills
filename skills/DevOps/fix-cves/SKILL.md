---
id: fix-cves
name: Fix CVEs
description: Triage and remediate CVEs in application and infrastructure dependencies with risk-based prioritization and verification. Use when security scans report vulnerabilities that must be fixed without destabilizing releases.
category: DevOps
requires: []
examples:
  - Prioritize and patch critical CVEs in container base images and runtime libraries.
  - Prepare a remediation plan for transitive dependency vulnerabilities with upgrade constraints.
---

# Fix CVEs

Reduce vulnerability risk with prioritized remediation, compatibility checks, and release-safe verification.

## When to Use

- Security scans identify CVEs in dependencies, containers, or build artifacts.
- You need to decide patch order based on exploitability and runtime exposure.
- You must ship fixes quickly while controlling regression risk.

## CVE Triage Techniques

**Exploit probability:** Use EPSS (Exploit Prediction Scoring System) to weight exploit likelihood. EPSS &gt; 0.1 indicates active exploitation is likely; prioritize these even if CVSS is lower.

**CISA KEV catalog:** Check whether the CVE appears in CISAΓÇÖs Known Exploited Vulnerabilities catalog. KEV-listed items require urgent remediation and often have mandated timelines.

**CVSS refinement:** Use CVSS temporal (exploit code maturity, remediation level) and environmental (confidentiality/integrity/availability impact in your context) scores. A high base score with ΓÇ£unprovenΓÇ¥ exploit code may rank below a medium CVE with ΓÇ£functionalΓÇ¥ exploit code.

**Runtime reachability:** Determine if the vulnerable code path is reachable in your deployment. Use call-graph or dependency-tree analysis; unreachable transitive deps can be deprioritized but not ignored.

## Remediation Patterns

**Pinning:** Lock exact versions in lockfiles (package-lock.json, Pipfile.lock, go.sum) to prevent drift. Prefer pinning after patching, not before.

**Patching:** Apply upstream patches when available. Prefer minor/patch upgrades over major when compatibility risk is high.

**Forking:** For unmaintained or slow-to-patch dependencies, maintain a minimal fork with the fix and plan migration to a maintained alternative.

**Virtual patching:** Use WAF rules, network segmentation, or runtime controls to mitigate when code changes are not feasible. Document as temporary; track a target date for proper remediation.

## Tool-Specific Guidance

**Trivy:** Use `trivy fs` for filesystem scans, `trivy image` for containers. Enable `--scanners vuln` and `--severity HIGH,CRITICAL`. Export to SARIF for CI integration.

**Grype:** Similar to Trivy; good for Syft-generated SBOMs. Use `grype sbom:./sbom.json` for SBOM-first workflows.

**Snyk:** Integrates with Dependabot-style PRs; use `snyk test` and `snyk monitor` for continuous tracking. Leverage SnykΓÇÖs fix suggestions and compatibility checks.

**Dependabot / Renovate:** Enable automated PRs for dependency updates. Configure grouping, scheduling, and merge rules to avoid PR fatigue. Use security-only mode if full upgrade noise is high.

## Common Pitfalls

- **CVSS-only prioritization:** High CVSS does not imply exploitability. Combine with EPSS and KEV status.
- **Fixing transitive deps without testing:** Transitive upgrades can introduce breaking changes. Run full test suites and integration checks.
- **Vendor advisory lag:** Scanners may report before official advisories. Cross-check NVD, vendor security pages, and GitHub advisories.
- **Scanner false positives masking real issues:** Validate findings; some scanners misattribute CVEs to wrong versions or report fixed-in versions incorrectly. Confirm affected version ranges.

## Workflow

1. Collect CVE findings with package/component, version, severity, and affected environment.
2. Prioritize using EPSS, KEV, CVSS temporal/environmental, and runtime reachability.
3. Choose remediation path: direct upgrade, transitive override, base image bump, fork, or virtual patch.
4. Run compatibility and regression tests on patched versions.
5. Re-scan artifacts to confirm CVE closure and capture residual risk.
6. Document deferred findings with mitigation and target fix date.
7. Publish change notes for security, platform, and service owners.

## Output Format

```markdown
## CVE Remediation Report

### Summary
- Total findings: <count>
- Critical: <count>, High: <count>
- KEV-listed: <count>, EPSS &gt; 0.1: <count>

### Prioritized Findings
| CVE | Package | Version | EPSS | KEV | Remediation | Status |
|-----|---------|---------|------|-----|-------------|--------|
| ... | ... | ... | ... | ... | ... | ... |

### Remediation Actions
- [ ] <CVE>: <action> ΓÇö <validation steps>
- [ ] ...

### Deferred (with mitigation)
- <CVE>: <reason> ΓÇö <mitigation> ΓÇö Target: <date>

### Post-Remediation Scan
- Tool: <name>
- Resolved: <count>
- Residual: <count> (documented)
```

## CVE Checklist

- [ ] Critical/high vulnerabilities triaged with EPSS, KEV, and reachability.
- [ ] Patch path chosen and validated against runtime compatibility.
- [ ] Post-fix scan confirms resolved findings.
- [ ] Deferred items include mitigation and owner/date.

## Constraints

- Do not prioritize by CVSS alone; include exploitability and exposure.
- Avoid blanket upgrades without dependency impact testing.
- Do not close findings without post-remediation scan evidence.
- Keep exception windows short and owner-approved.