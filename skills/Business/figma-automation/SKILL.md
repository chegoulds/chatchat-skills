---
id: figma-automation
name: Figma Automation
description: Guidance-only framework for planning Figma design operations, exports, token extraction, and collaboration workflows.
category: Development
requires: []
examples:
  - "Help me design a Figma file export workflow for engineering handoff."
  - "Use figma-automation to plan design token extraction and governance."
---

# Figma Automation

This is a guidance-only skill for designing repeatable Figma workflows. It does not execute integrations or API calls directly.

## When to use

- Planning design-to-dev handoff flows.
- Defining component, token, and export conventions.
- Auditing comments/version workflows for teams.

## Workflow patterns

### 1) File and component discovery

- Define how teams identify canonical files and libraries.
- Standardize node naming and component set conventions.
- Document ID mapping format expectations across tools.

### 2) Export and asset delivery

- Specify export formats per asset type (PNG/SVG/PDF/JPG).
- Define scale, naming, and folder conventions for handoff.
- Add fallback rules when render outputs are incomplete.

### 3) Design token governance

- Map color, typography, and spacing tokens to engineering targets.
- Keep token source-of-truth versioned and reviewable.
- Validate conversion output against design system standards.

### 4) Collaboration operations

- Define comment etiquette and resolution SLAs.
- Create version checkpoints before major design changes.
- Track approval state for release-critical screens.

## Common pitfalls to prevent

- Inconsistent node naming causing broken automation mappings.
- Overly broad exports generating oversized or unused assets.
- Token changes without downstream impact review.
- Missing review ownership for comments and version approvals.

## Output format

When asked for help, provide:

- A step-by-step workflow tailored to the user's team.
- Required inputs/metadata for each stage.
- A risk checklist and rollout plan for operational adoption.
