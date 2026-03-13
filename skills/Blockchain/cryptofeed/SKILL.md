---
id: cryptofeed
name: Cryptofeed
description: Guidance playbook for Cryptofeed: plan and execute manually without requiring direct backend/API execution.
category: Blockchain
requires: []
examples:
  - "Set up real-time ticker feed from Coinbase and Kraken"
  - "Stream order book data for BTC-USD from multiple exchanges"
---

# Cryptofeed

Offer advisory support for Cryptofeed without executing systems directly. Help the user choose a clear path and execute manually.

## Primary Goal

- Core focus: market integration and execution strategy.
- Skill context keywords: cryptofeed.
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

- "Set up real-time ticker feed from Coinbase and Kraken"
- "Stream order book data for BTC-USD from multiple exchanges"
