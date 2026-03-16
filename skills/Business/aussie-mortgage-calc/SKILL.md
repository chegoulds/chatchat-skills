---
id: aussie-mortgage-calc
name: Aussie Mortgage Calc
description: Estimate Australian home-buying affordability using mortgage repayments, deposit scenarios, LVR, and fee planning checklists. Use when users need home loan budgeting and purchase planning support.
category: Lifestyle
requires: []
examples:
  - "Estimate repayments and upfront costs for buying a $850,000 property in NSW."
  - "Compare 10% vs 20% deposit scenarios for an Australian first-home buyer."
---

# Australian Mortgage Calculator

Use this skill as a guidance-only framework for Australian home-loan affordability planning.

## Use this skill when

- The user needs repayment and affordability estimates.
- The user wants deposit and LVR scenario comparisons.
- The user needs a structured checklist of likely upfront costs.

## Do not use this skill when

- The request is unrelated to personal mortgage planning.
- The user expects lender-grade approval decisions from this skill.

## Guardrails

- Treat outputs as estimates, not financial advice.
- Always tell users to verify current rates, duties, grants, and lender policy for their state.
- Do not present outdated government incentive figures as guaranteed.

## Core calculations

### 1) LVR (Loan-to-Value Ratio)

```text
LVR = (Loan Amount / Property Value) × 100
```

### 2) Principal and Interest repayment (monthly)

```text
M = P × [r(1+r)^n] / [(1+r)^n - 1]

Where:
P = loan principal
r = monthly interest rate (annual rate / 12)
n = total months (loan term × 12)
```

### 3) Interest-only repayment (monthly)

```text
Monthly IO = Principal × (Annual Rate / 12)
```

## Planning workflow

1. **Collect buyer inputs**
   - Property price target
   - Deposit amount
   - Loan term and expected interest rate
   - State/territory
   - Existing debts and household context

2. **Run scenario comparison**
   - Baseline (current deposit)
   - Lower deposit scenario (higher LVR)
   - Higher deposit scenario (lower LVR)
   - Optional stress scenario (interest rate +1% or +2%)

3. **Estimate upfront cost buckets**
   - Stamp duty (state dependent; verify latest schedule)
   - LMI likelihood if LVR exceeds common thresholds
   - Legal/conveyancing, inspections, and setup fees

4. **Assess repayment comfort**
   - Compare repayment estimate to user’s budget buffer.
   - Highlight risk if repayments exceed comfort threshold.

5. **Summarize actions**
   - What to verify with broker/lender
   - Which scenario appears most sustainable
   - Key risks and mitigations

## Output format

```markdown
## Home Loan Inputs
- Property value:
- Deposit:
- State:
- Interest rate assumption:
- Loan term:

## Scenario Comparison
| Scenario | Loan Amount | LVR | Monthly P&I | Monthly IO | Notes |
|---------|-------------|-----|-------------|------------|------|
| Base | | | | | |
| Lower deposit | | | | | |
| Higher deposit | | | | | |

## Upfront Cost Checklist (Estimates)
- Stamp duty (verify current state rates):
- LMI likelihood/cost range:
- Legal/conveyancing:
- Inspections and miscellaneous:

## Affordability and Risk Notes
- Repayment comfort assessment:
- Sensitivity to rate rises:
- Main risks:

## Next Steps
1.
2.
3.
```
