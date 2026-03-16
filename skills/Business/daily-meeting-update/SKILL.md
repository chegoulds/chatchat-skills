---
id: daily-meeting-update
name: Daily Meeting Update
description: Run a structured standup interview and produce a clear daily update covering yesterday, today, blockers, and discussion topics.
category: Business
requires: []
examples:
  - "Help me prepare my daily standup update."
  - "Turn my notes into a clean yesterday/today/blockers summary."
---

# Daily Meeting Update

Use this skill as a guidance-only workflow to collect status information and generate a concise meeting update.

## Use this skill when

- The user needs a daily standup or check-in update.
- The user has rough notes and wants a polished summary.
- The user wants a manager-friendly or team-friendly status format.

## Do not use this skill when

- The request is unrelated to progress reporting or team updates.
- The user expects direct integration calls to external systems from this skill.

## Guardrails

- Do not claim to fetch data from Jira, GitHub, Slack, or other tools unless explicitly available through supported backend tools.
- If context is missing, ask short clarifying questions instead of guessing.
- Keep updates factual, concise, and action-oriented.

## Interview workflow

1. **Clarify audience and style**
   - Ask who the update is for (team, manager, client).
   - Ask preferred tone (brief, detailed, executive).

2. **Collect core inputs**
   - Yesterday: completed work and outcomes
   - Today: planned work and priorities
   - Blockers: risks, dependencies, asks
   - Discussion topics: items needing team input

3. **Improve signal quality**
   - Convert vague statements into concrete outcomes.
   - Add impact where possible (why it matters).
   - Highlight owners and next actions for blockers.

4. **Generate final update**
   - Keep bullets short and scannable.
   - Include only relevant detail for the audience.
   - End with explicit asks if blockers exist.

## Output format

```markdown
## Daily Update

### Yesterday
- Completed:
- Outcome/impact:

### Today
- Priority 1:
- Priority 2:

### Blockers
- Blocker:
  - Owner:
  - Next step:

### Discussion Topics
- Topic 1:
- Topic 2:

### Help Needed
- Ask:
```

## Optional variants

- **Short version (30-second standup):** one bullet each for yesterday, today, blockers.
- **Manager version:** add risk level and ETA confidence.
- **Async version:** include explicit decisions needed before next update.
