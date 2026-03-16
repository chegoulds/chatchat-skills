---
id: analyzing-financial-statements
name: Analyzing Financial Statements
description: Analyze company financial statements using ratio frameworks, trend review, and risk interpretation. Use when users need profitability, liquidity, leverage, efficiency, or valuation analysis.
category: Business
requires: []
examples:
  - "Analyze this company's financial statements and highlight key risks."
  - "Compare these two companies using ratio-based financial analysis."
---

# Analyzing Financial Statements

Use this skill as a guidance-only framework for financial statement analysis and interpretation.

## Use this skill when

- The user wants ratio-based financial analysis for a company.
- The user needs period-over-period or peer comparison.
- The user needs a structured interpretation of financial strengths, weaknesses, and risks.

## Do not use this skill when

- The request is unrelated to financial performance analysis.
- The user expects direct data scraping, live market feeds, or script execution from this skill.

## Guardrails

- Treat this as analytical guidance; do not claim automated spreadsheet/script execution.
- State assumptions clearly when data is incomplete.
- Avoid definitive investment advice; provide risk-aware interpretation.

## Analysis workflow

1. **Define scope**
   - Confirm entity, period, and purpose (credit risk, investment screening, performance review).
   - Confirm whether the user wants single-company analysis or comparison.

2. **Check data completeness**
   - Income statement: revenue, gross profit, operating profit, net income.
   - Balance sheet: current assets/liabilities, debt, equity, cash.
   - Cash flow: operating cash flow, capex, financing context.
   - Flag missing data before drawing conclusions.

3. **Calculate core ratio groups**
   - **Profitability**: ROE, ROA, gross/operating/net margins
   - **Liquidity**: current ratio, quick ratio, cash ratio
   - **Leverage**: debt-to-equity, interest coverage
   - **Efficiency**: asset turnover, inventory turnover, receivables turnover
   - **Valuation** (if market data provided): P/E, P/B, EV/EBITDA

4. **Interpret in context**
   - Compare against prior periods and (if available) peers.
   - Distinguish one-time effects from recurring trends.
   - Note where industry structure changes ratio interpretation.

5. **Summarize risk and next actions**
   - Highlight top financial risks and their likely implications.
   - Recommend follow-up questions and further analysis.

## Output format

```markdown
## Analysis Scope
- Company:
- Period:
- Objective:
- Data completeness notes:

## Key Ratios
### Profitability
- ROE:
- ROA:
- Gross/Operating/Net Margin:

### Liquidity
- Current Ratio:
- Quick Ratio:
- Cash Ratio:

### Leverage
- Debt-to-Equity:
- Interest Coverage:

### Efficiency
- Asset Turnover:
- Inventory/Receivables Turnover:

### Valuation (if market data provided)
- P/E:
- P/B:
- EV/EBITDA:

## Interpretation
- Strengths:
- Weaknesses:
- Trend notes:
- Peer/benchmark notes:

## Risk Summary
- Risk 1:
- Risk 2:

## Recommended Next Steps
1.
2.
```
