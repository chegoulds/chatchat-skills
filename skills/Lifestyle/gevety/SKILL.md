---
category: Lifestyle
id: gevety
name: Gevety
version: 1.5.0
description: Access your Gevety health data - biomarkers, healthspan scores, biological age, supplements, activities, daily actions, 90-day health protocol, and upcoming tests.
homepage: https://gevety.com
user-invocable: true
command: gevety
metadata:
  clawdbot:
    primaryEnv: GEVETY_API_TOKEN
    requires:
      env:
        - GEVETY_API_TOKEN
---

# Gevety Health Assistant

Use the Gevety API to fetch biomarker and health data, then summarize trends.

## Setup

- Requires `GEVETY_API_TOKEN` in the environment.

## Workflow

1. Confirm the token is available.
2. Fetch: biomarkers, healthspan scores, biological age, supplements, activities, daily actions, 90-day protocol, and upcoming tests.
3. Summarize trends and highlight notable changes.

## Output

- Snapshot table of key metrics
- Trends (improving, stable, declining)
- Actionable suggestions and next steps

## Safety

Provide general wellness guidance only; recommend a clinician for diagnosis or treatment decisions.
