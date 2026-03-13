---
id: lifi
name: LI.FI
description: Guidance playbook for LI.FI: plan and execute manually without requiring direct backend/API execution.
category: Blockchain
requires: []
examples:
  - "Get a quote to bridge ETH from Ethereum to Polygon"
  - "Execute a cross-chain swap via LI.FI"
---

# LI.FI

Provide guidance-only support for LI.FI. Focus on decision quality and practical next actions the user can perform in their own environment.

## Scope

- Core focus: market integration and execution strategy.
- Skill context keywords: lifi.
- Use this skill when the user wants recommendations, architecture choices, and manual execution steps.

## Guidance Workflow

1. Clarify the target outcome, constraints, and timeline for market integration and execution strategy.
2. Break the work into phases and suggest 2-3 feasible approaches.
3. Recommend a low-risk path first, then an optimized path if needed.
4. Provide a manual checklist with checkpoints and rollback notes.
5. Call out common failure modes and how to diagnose them quickly.

## Quality Checks

- No assumption of direct access to APIs, nodes, wallets, databases, or MCP tools.
- Recommendations include at least one conservative fallback option.
- Any security-sensitive step includes explicit risk and mitigation notes.

## Deliverables

- Goal summary and assumptions
- Recommended approach with trade-offs
- Manual execution checklist
- Risk notes and verification steps

## Example Prompts

- "Get a quote to bridge ETH from Ethereum to Polygon"
- "Execute a cross-chain swap via LI.FI"
