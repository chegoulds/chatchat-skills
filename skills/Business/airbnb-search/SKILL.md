---
id: airbnb-search
name: Airbnb Search
description: Search Airbnb listings with prices, ratings, and direct links. No API key required.
category: Business
requires: []
examples:
  - "Help me shortlist Airbnb stays in Bangalore for a 5-day work trip."
  - "Compare these Airbnb options and recommend the best one for a family trip."
---

# Airbnb Search

Use this skill as a guidance-only framework to evaluate short-term stay options and produce a clear recommendation.

## Use this skill when

- The user needs help comparing Airbnb-style accommodation options.
- The user has multiple listing options and wants a recommendation.
- The user wants a structured travel-stay decision summary.

## Do not use this skill when

- The user expects this skill to fetch live listings directly.
- The request is unrelated to travel accommodation planning.

## Guardrails

- Treat this as decision-support guidance only; do not claim live API/search execution.
- Ask for missing constraints (budget, dates, location, guest count, priorities).
- Avoid assumptions about availability and pricing unless explicitly provided.

## Evaluation workflow

1. **Collect trip constraints**
   - Destination, dates, guest count
   - Budget range (nightly or total)
   - Must-have amenities (wifi, kitchen, workspace, parking)
   - Preferred area and transport access

2. **Define ranking criteria**
   - Price fit
   - Location fit
   - Amenities fit
   - Rating/review confidence
   - Policy flexibility (cancellation, check-in rules)

3. **Score candidate stays**
   - Use a simple weighted score (for example: price 30%, location 30%, amenities 25%, reviews 15%).
   - Highlight trade-offs instead of forcing a single “perfect” choice.

4. **Create shortlist**
   - Provide top 3 options with rationale.
   - Flag risks (noise, commute distance, hidden fees, low review count).

5. **Recommend final choice**
   - Best overall option
   - Best value option
   - Lowest-risk option
   - Include what information is still needed before booking

## Output format

```markdown
## Trip Constraints
- Destination:
- Dates:
- Guests:
- Budget:
- Must-have amenities:

## Comparison Table
| Option | Price Fit | Location Fit | Amenities Fit | Review Confidence | Risk Notes |
|--------|-----------|--------------|---------------|-------------------|-----------|
| A | | | | | |
| B | | | | | |
| C | | | | | |

## Top Recommendations
1. **Best Overall**: [Option]
   - Why:
   - Trade-off:
2. **Best Value**: [Option]
   - Why:
   - Trade-off:
3. **Lowest Risk**: [Option]
   - Why:
   - Trade-off:

## Final Decision Guidance
- Recommended booking choice:
- Open questions to confirm before booking:
```
