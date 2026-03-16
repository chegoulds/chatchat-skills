---
id: signnow
name: Signnow
description: Guidance-only playbook for e-signature operations with SignNow, including document lifecycle governance, signer workflows, and compliance controls.
category: Business
requires: []
examples:
  - "Help me define a SignNow document workflow with approvals and reminders."
  - "Use signnow to improve e-sign compliance and turnaround times."
---

# SignNow Operations

Use this guidance-only skill to design e-signature process governance. It does not execute API requests, scripts, or external commands.

## When to use

- You need standardized contract and approval flows.
- You want lower signature turnaround time.
- You need auditable controls for signed document handling.

## Core workflows

### 1) Document lifecycle design

- Define document classes (NDA, MSA, SOW, HR forms) and owners.
- Set template governance, approval checkpoints, and version control.
- Establish retention and archival policies by document type.

### 2) Signer workflow governance

- Standardize signer roles, sequence, and fallback delegates.
- Use reminder and expiration policies by deal or request urgency.
- Document exception paths for rejected or expired requests.

### 3) Compliance and auditability

- Require complete metadata (requestor, purpose, counterparty, effective date).
- Keep evidence trails for signatures, approvals, and amendments.
- Run periodic access and permission reviews for sensitive templates.

### 4) Operational reporting

- Track send-to-sign time, rejection rate, and rework causes.
- Segment delays by document type and team.
- Use monthly reviews to improve templates and routing policies.

## Risk controls

- Avoid template drift without legal review checkpoints.
- Prevent signer sequence mistakes that invalidate workflows.
- Ensure signed-document access follows least-privilege practices.

## Output format

When asked for help, provide:

- A signature workflow policy.
- A document governance checklist.
- A KPI tracker for turnaround and compliance.
