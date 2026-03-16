---
id: anndata
name: AnnData
description: Practical guidance for using AnnData in scalable single-cell workflows.
category: Data & AI
requires: [".h5ad"]
examples:
  - "Show me best practices for organizing AnnData objects."
  - "How do I optimize AnnData workflows for large single-cell datasets?"
---

# AnnData

Help the agent provide practical guidance for AnnData modeling, storage, and analysis workflows.

## When to Use

- The user needs help structuring or manipulating AnnData objects.
- The user asks about memory usage, I/O formats, or interoperability.
- The user wants implementation details for robust single-cell pipelines.

## Instructions

1. Clarify data volume, schema, and downstream analysis goals.
2. Recommend AnnData layout conventions for `X`, `obs`, `var`, and layers.
3. Suggest efficient loading, slicing, and persistence patterns.
4. Highlight common pitfalls (shape mismatch, metadata drift, memory blowups).
5. End with a reproducible validation checklist.

## Output

- Recommended AnnData approach and rationale
- Step-by-step implementation checklist
- Validation and troubleshooting checks
