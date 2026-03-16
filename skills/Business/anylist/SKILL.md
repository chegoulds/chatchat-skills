---
id: anylist
name: Anylist
description: Plan and organize grocery or shopping lists with clear categories, quantities, and checklist-ready output. Use when users need shopping preparation, meal-prep list building, or household list cleanup.
category: Lifestyle
requires: []
examples:
  - "Create a grocery list for a week of high-protein meals."
  - "Organize this messy shopping list by category and priority."
---

# AnyList

Use this skill as a guidance-only framework for building and organizing shopping lists.

## Use this skill when

- The user needs a grocery or shopping list from goals, recipes, or preferences.
- The user wants list cleanup, deduplication, or category grouping.
- The user needs a checklist-ready output for shopping.

## Do not use this skill when

- The request is unrelated to personal shopping or household list planning.
- The user expects direct CLI/app execution from this skill.

## Guardrails

- Treat this as planning guidance only; do not claim to run AnyList commands.
- Ask for missing constraints (budget, servings, dietary restrictions, store type).
- Keep list items concrete and measurable where possible (quantity + unit).

## List-building workflow

1. **Clarify intent**
   - Trip type: weekly grocery, party, household restock, recipe prep
   - Number of people and duration
   - Budget and dietary constraints

2. **Draft raw item pool**
   - Convert user goals/recipes into ingredients and staples
   - Include household essentials if requested

3. **Normalize and deduplicate**
   - Merge duplicates (e.g., "milk" + "whole milk")
   - Standardize quantities and units
   - Flag uncertain quantities for confirmation

4. **Group by category**
   - Produce, meat/seafood, dairy, pantry, frozen, bakery, beverages, household, personal care, other

5. **Prioritize and finalize**
   - Mark must-buy vs optional items
   - Add quick substitutions for unavailable items

## Suggested categories

- Produce
- Meat/Seafood
- Dairy
- Pantry
- Frozen
- Bakery
- Beverages
- Household
- Personal Care
- Other

## Output format

```markdown
## Shopping Plan
- Context:
- People/Duration:
- Budget:
- Dietary constraints:

## Must-Buy Items
- Item - quantity - category
- Item - quantity - category

## Optional Items
- Item - quantity - category
- Item - quantity - category

## Grouped Checklist
### Produce
- [ ] Item

### Pantry
- [ ] Item

### Dairy
- [ ] Item

## Substitutions / Notes
- If unavailable, replace X with Y.
- Priority notes:
```
