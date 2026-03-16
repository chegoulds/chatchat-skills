---
id: backtesting-frameworks
name: Backtesting Frameworks
description: Guidance-only framework for evaluating trading strategy backtests with realistic assumptions, bias controls, and decision-ready reporting.
category: Business
requires: []
examples:
  - "Help me assess whether this backtest is reliable enough for deployment."
  - "Use backtesting-frameworks to design a robust validation plan for this strategy."
---

# Backtesting Frameworks

Use this guidance-only skill to evaluate strategy-testing quality and decision readiness. It does not execute code or place trades.

## When to use

- You need to judge whether a backtest result is credible.
- You want a structured process for avoiding common quant research errors.
- You need an approval-style summary before moving from research to pilot.

## Core evaluation workflow

### 1) Define test objective and scope

- Clarify strategy type (trend, mean reversion, stat arb, options, market making).
- Define target market, timeframe, and trading horizon.
- Set decision criteria before testing (minimum Sharpe, max drawdown, turnover limits).

### 2) Validate data quality and representativeness

- Verify data source coverage, gaps, and survivorship handling.
- Confirm corporate actions, symbol mapping, and delisting treatment.
- Check timestamp alignment across prices, signals, and execution assumptions.

### 3) Remove methodological bias

- Guard against look-ahead bias in feature engineering and labels.
- Separate in-sample from out-of-sample periods with no leakage.
- Use walk-forward or rolling windows for non-stationary markets.

### 4) Model trading frictions realistically

- Include commissions, fees, borrow costs, and realistic slippage.
- Apply position sizing and liquidity constraints by instrument.
- Stress-test fill assumptions for volatile and low-liquidity regimes.

### 5) Stress and sensitivity testing

- Test across market regimes (high vol, low vol, crisis periods, trend reversals).
- Perturb key parameters to detect fragile optimization.
- Assess concentration risk by symbol, sector, and factor exposure.

### 6) Produce decision-oriented outputs

- Report performance distributions, not only point estimates.
- Show failure modes and specific conditions where strategy degrades.
- Recommend next action: reject, revise, paper trade, or limited rollout.

## Minimum KPI set

- CAGR, volatility, Sharpe/Sortino
- Max drawdown and recovery time
- Hit rate and payoff ratio
- Turnover and capacity estimate
- Exposure by market/factor bucket

## Red flags

- Exceptional in-sample performance with weak out-of-sample stability.
- Large performance drop after realistic cost assumptions.
- Heavy dependence on a narrow period, market, or parameter setting.
- Missing audit trail for assumptions and experiment versions.

## Output format

When asked for help, provide:

- Backtest credibility scorecard (data, method, costs, robustness).
- Top 5 weaknesses and concrete remediation steps.
- Go/no-go recommendation with clear gating thresholds.
