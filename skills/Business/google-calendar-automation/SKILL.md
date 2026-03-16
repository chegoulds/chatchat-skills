---
id: google-calendar-automation
name: Google Calendar Automation
description: Guidance-only workflow for planning calendar scheduling, availability checks, and attendee coordination.
category: Business
requires: []
examples:
  - "Help me design a meeting scheduling workflow."
  - "Use google-calendar-automation to improve recurring meeting operations."
---

# Google Calendar Scheduling Operations

Use this guidance-only skill to design reliable scheduling processes. It does not execute integrations or API calls directly.

## When to use

- You need repeatable meeting scheduling workflows.
- You want standards for availability checks and attendee handling.
- You need escalation rules for conflicts and overbooking.

## Core workflows

### 1) Event planning standards

- Define event naming conventions and ownership.
- Standardize required fields: title, timezone, attendees, purpose.
- Require clear success/cancel behavior for booking flows.

### 2) Availability and slot selection

- Check availability windows before proposing slots.
- Use local timezone context for date-bound requests.
- Keep fallback slot options for faster coordination.

### 3) Attendee operations

- Confirm attendee identity before scheduling.
- Define who can add/remove attendees and when.
- Track organizer and approver roles for external meetings.

### 4) Recurring meeting governance

- Define recurrence cadence and review frequency.
- Archive or sunset stale recurring meetings.
- Add periodic agenda refresh checks.

## Common pitfalls

- Timezone mismatch causing wrong-day bookings.
- Missing context in event titles/descriptions.
- Silent attendee replacement without notification.
- No conflict/escalation policy for high-priority meetings.

## Output format

When asked for help, provide:

- A scheduling SOP (create/search/update/cancel paths).
- A conflict-resolution checklist.
- A concise communication template for attendees.
