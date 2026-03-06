---
id: adaptyv
name: "Adaptyv"
description: "Plan Adaptyv protein experiment workflows, choose assay strategy, and structure submission-ready requirements. Use when users need experiment planning, sequence readiness checks, or result interpretation frameworks for protein validation."
category: Business
requires: []
examples:
  - "Help me choose the right Adaptyv assay for this protein objective."
  - "Turn these protein notes into a submission-ready experiment plan."
---

# Adaptyv

Use this skill as a guidance-only framework for planning protein validation work with Adaptyv-style lab workflows.

## Use this skill when

- The user needs help designing protein validation experiments.
- The user needs an assay selection recommendation with trade-offs.
- The user wants sequence readiness checks before lab submission.
- The user needs a structured interpretation framework for returned results.

## Do not use this skill when

- The request is unrelated to protein testing or experiment planning.
- The user expects direct API execution or real lab submission from this skill.

## Guardrails

- Treat this as guidance-only; do not claim to execute API calls.
- Do not fabricate experiment IDs, results, or backend actions.
- If execution is requested, provide a step-by-step plan the user can run in their own environment.

## Available Experiment Types

Use these options to map user goals to experiment design:

- **Binding assays** - Test protein-target interactions using biolayer interferometry
- **Expression testing** - Measure protein expression levels
- **Thermostability** - Characterize protein thermal stability
- **Enzyme activity** - Assess enzymatic function

## Protein Sequence Optimization

Before planning submission, check sequence readiness for expression and stability risk.

Common issues to assess:
- Unpaired cysteines that create unwanted disulfides
- Excessive hydrophobic regions causing aggregation
- Poor solubility predictions

Recommended analysis families:
- NetSolP / SoluProt - Initial solubility filtering
- SolubleMPNN - Sequence redesign for improved solubility
- ESM - Sequence likelihood scoring
- ipTM - Interface stability assessment
- pSAE - Hydrophobic exposure quantification

## Workflow

1. **Define objective**
   - Clarify target behavior: binding, expression, stability, or activity.
   - Capture constraints: turnaround, budget sensitivity, required confidence.

2. **Select assay strategy**
   - Propose one primary assay and one fallback.
   - Explain trade-offs: signal quality, interpretability, likely failure modes.

3. **Run sequence readiness review**
   - Flag high-risk motifs and solubility concerns.
   - Provide mitigation suggestions before submission.

4. **Prepare submission packet checklist**
   - Sequence format and metadata completeness
   - Experiment type and success criteria
   - Control conditions and acceptance thresholds

5. **Plan interpretation**
   - Define what counts as pass, fail, or inconclusive.
   - List follow-up actions for each outcome.

## Output format

```markdown
## Experiment Objective
- Primary goal:
- Key constraint:

## Assay Recommendation
- Primary assay:
- Why this assay:
- Backup assay:
- Trade-offs:

## Sequence Readiness Review
- Risks:
- Mitigations:

## Submission Checklist
- [ ] Sequence and metadata complete
- [ ] Assay and controls defined
- [ ] Success criteria defined

## Interpretation Plan
- Pass criteria:
- Fail criteria:
- Inconclusive criteria:
- Next actions by scenario:
```
