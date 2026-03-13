---
id: agent-earner
name: Agent Earner
description: Guidance playbook for Agent Earner: plan and execute manually without requiring direct backend/API execution.
category: Blockchain
requires: []
examples:
  - "List open bounties and submit a proposal"
  - "Start autonomous mode for bounty discovery"
version: 1.0.0
author: Prometheus_Prime
---

# Agent Earner

Offer advisory support for Agent Earner without executing systems directly. Help the user choose a clear path and execute manually.

## Primary Goal

- Core focus: project planning and implementation guidance.
- Skill context keywords: agent, earner.
- Use this skill when the user wants recommendations, architecture choices, and manual execution steps.

## Recommended Process

1. Clarify the target outcome, constraints, and timeline for project planning and implementation guidance.
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

- "List open bounties and submit a proposal"
- "Start autonomous mode for bounty discovery"
