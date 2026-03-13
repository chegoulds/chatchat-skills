---
id: zapper-api
name: Zapper API
description: Guidance playbook for Zapper API: plan and execute manually without requiring direct backend/API execution.
category: Blockchain
requires: []
examples:
  - "Get my DeFi portfolio summary across all chains"
  - "Show my unclaimed rewards and claimables"
---

# Zapper API

Offer advisory support for Zapper API without executing systems directly. Help the user choose a clear path and execute manually.

## Primary Goal

- Core focus: market integration and execution strategy.
- Skill context keywords: zapper.
- Use this skill when the user wants recommendations, architecture choices, and manual execution steps.

## Recommended Process

1. Clarify the target outcome, constraints, and timeline for market integration and execution strategy.
2. Break the work into phases and suggest 2-3 feasible approaches.
3. Recommend a low-risk path first, then an optimized path if needed.
4. Provide a manual checklist with checkpoints and rollback notes.
5. Call out common failure modes and how to diagnose them quickly.

## Validation Checks

- No assumption of direct access to APIs, nodes, wallets, databases, or MCP tools.
- Recommendations include at least one conservative fallback option.
- Any security-sensitive step includes explicit risk and mitigation notes.

## What to Return

- Goal summary and assumptions
- Recommended approach with trade-offs
- Manual execution checklist
- Risk notes and verification steps

## Example Prompts

- "Get my DeFi portfolio summary across all chains"
- "Show my unclaimed rewards and claimables"
