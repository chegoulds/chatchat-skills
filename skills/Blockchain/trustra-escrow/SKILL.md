---
id: trustra-escrow
name: Trustra Escrow
description: Guidance playbook for Trustra Escrow: plan and execute manually without requiring direct backend/API execution.
category: Blockchain
requires: []
examples:
  - "Create a USDC escrow with the seller's wallet for payment"
  - "Check the status of my escrow and mark it as delivered"
version: 1.0.0
---

# Trustra Escrow

Offer advisory support for Trustra Escrow without executing systems directly. Help the user choose a clear path and execute manually.

## Primary Goal

- Core focus: wallet operations and transaction safety.
- Skill context keywords: trustra, escrow.
- Use this skill when the user wants recommendations, architecture choices, and manual execution steps.

## Recommended Process

1. Restate the user objective for wallet operations and transaction safety in one sentence.
2. Identify prerequisites and dependency constraints up front.
3. Propose a phased plan (baseline, improve, harden).
4. Translate each phase into hands-on actions the user can execute.
5. Include acceptance checks and post-change monitoring guidance.

## Validation Checks

- No claims of having executed commands, queries, transactions, or deployments.
- Plan includes rollback or recovery guidance for mistakes.
- Final answer is concise, ordered, and easy to hand off to implementation.

## What to Return

- Context recap
- Implementation plan by phase
- Acceptance tests and success metrics
- Known pitfalls and mitigations

## Example Prompts

- "Create a USDC escrow with the seller's wallet for payment"
- "Check the status of my escrow and mark it as delivered"
