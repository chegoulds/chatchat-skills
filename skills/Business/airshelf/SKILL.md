---
id: airshelf
name: Airshelf
description: Search, compare, and buy products from verified merchants. Returns structured product data with Decision Packs (pros, cons, best_for, allergens, verified pricing) instead of raw web scraping. No CAPTCHAs, no auth required. ~980 products across 10 merchants.
category: Business
requires: []
examples:
  - "Help me compare product options for office chairs under a fixed budget."
  - "Create a shortlist and recommendation for safe baby sunscreen options."
---

# AirShelf

Use this skill as a guidance-only framework for product discovery, comparison, and recommendation decisions.

## Use this skill when

- The user needs help narrowing product options.
- The user needs side-by-side trade-off analysis.
- The user wants a recommendation aligned to budget, safety, and use case.

## Do not use this skill when

- The user expects direct live search, checkout, or API execution from this skill.
- The task is unrelated to product decision support.

## Guardrails

- Treat this as advisory guidance only; do not claim to execute live merchant or checkout actions.
- Do not fabricate exact real-time prices, stock status, or shipping promises.
- For health/food/skincare recommendations, include safety and allergen caution notes.

## Decision workflow

1. **Clarify buyer intent**
   - What problem is being solved?
   - Who is the end user?
   - What constraints exist (budget, timeline, region, compliance)?

2. **Define evaluation criteria**
   - Price fit
   - Feature fit
   - Quality/reliability confidence
   - Safety/allergen risk (if relevant)
   - Return/refund and support confidence

3. **Build shortlist**
   - Select 3 to 5 options.
   - Include one best-overall option, one best-value option, and one low-risk option.

4. **Compare trade-offs**
   - Explain why each option is good for specific needs.
   - Highlight key downsides and risks.
   - Show where paying more is justified (or not).

5. **Recommend and finalize**
   - Provide a primary recommendation and fallback.
   - List missing details needed before purchase.
   - Include a final risk check.

## Evaluation checklist

- [ ] User goal and context captured
- [ ] Budget boundaries defined
- [ ] Must-have criteria listed
- [ ] Safety/allergen checks considered (if applicable)
- [ ] Shortlist trade-offs explained
- [ ] Final recommendation + fallback provided

## Output format

```markdown
## Buying Goal
- Problem to solve:
- Buyer/user profile:
- Budget:
- Must-have requirements:

## Shortlist
| Option | Price Fit | Feature Fit | Quality Confidence | Safety/Risk Notes | Best For |
|--------|-----------|-------------|--------------------|-------------------|----------|
| A | | | | | |
| B | | | | | |
| C | | | | | |

## Recommendation
1. **Best Overall**: [Option]
   - Why:
   - Main trade-off:
2. **Best Value**: [Option]
   - Why:
   - Main trade-off:
3. **Lowest Risk**: [Option]
   - Why:
   - Main trade-off:

## Final Decision Notes
- Missing info before purchase:
- Suggested next step:
```

