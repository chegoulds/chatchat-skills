---
id: aibtc-bitcoin-wallet
name: AIBTC Bitcoin Wallet
description: Guidance playbook for AIBTC Bitcoin Wallet: plan and execute manually without requiring direct backend/API execution.
category: Blockchain
requires: []
examples:
  - "Check my Bitcoin balance"
  - "Send BTC from my agent wallet"
author: aibtcdev
version: 1.14.2
---

# AIBTC Bitcoin Wallet

Support AIBTC Bitcoin Wallet as a planning and implementation guide. Prioritize concrete recommendations, trade-offs, and manual runbooks.

## Objective

- Core focus: wallet operations and transaction safety.
- Skill context keywords: aibtc, bitcoin, wallet.
- Use this skill when the user wants recommendations, architecture choices, and manual execution steps.

## Action Plan

1. Clarify the target outcome, constraints, and timeline for wallet operations and transaction safety.
2. Break the work into phases and suggest 2-3 feasible approaches.
3. Recommend a low-risk path first, then an optimized path if needed.
4. Provide a manual checklist with checkpoints and rollback notes.
5. Call out common failure modes and how to diagnose them quickly.

## Review Checklist

- No assumption of direct access to APIs, nodes, wallets, databases, or MCP tools.
- Recommendations include at least one conservative fallback option.
- Any security-sensitive step includes explicit risk and mitigation notes.

## Response Format

- Goal summary and assumptions
- Recommended approach with trade-offs
- Manual execution checklist
- Risk notes and verification steps

## Example Prompts

- "Check my Bitcoin balance"
- "Send BTC from my agent wallet"
