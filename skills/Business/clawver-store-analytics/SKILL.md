---
id: clawver-store-analytics
name: Clawver Store Analytics
description: Guidance-only framework for monitoring store performance using revenue, conversion, product, and customer-behavior analytics.
category: Business
requires: []
examples:
  - "Help me with clawver store analytics."
  - "Use clawver-store-analytics for this task."
version: "1.1.0"
---

# Clawver Store Analytics

This is a guidance-only skill for evaluating store performance and turning metrics into business actions. It does not execute API calls.

## When to use

- You need a recurring performance review process for an online store.
- You want to diagnose conversion, pricing, and retention issues.
- You need decision-ready reporting for weekly or monthly planning.

## Core metric framework

Track these groups together to avoid misleading conclusions:

### Revenue and efficiency

- `totalRevenue`: gross revenue after refunds, before platform fees.
- `netRevenue`: revenue after platform fees.
- `averageOrderValue`: average transaction size.
- `platformFees`: cost of distribution platform.

### Demand and conversion

- `storeViews`: top-of-funnel demand signal.
- `productViews`: listing-level demand signal.
- `conversionRate`: orders divided by relevant views.
- `totalOrders`: conversion output volume.

### Product quality signals

- `units`: sales velocity by SKU/product.
- `averageRating`: quality perception.
- `reviewsCount`: confidence level of rating data.

## Time-window analysis

Use a fixed cadence:

- Weekly review for operational tuning.
- Monthly review for strategy shifts.
- Quarterly review for assortment and pricing policy.

Compare shorter windows to baseline windows and flag gaps, not just raw totals.

## Diagnostic playbooks

### Low conversion

If conversion is low, inspect in this order:

1. Listing clarity (title, imagery, description)
2. Pricing and shipping competitiveness
3. Social proof (ratings/reviews)
4. Checkout friction and trust signals

### High traffic, low sales

- Revisit value proposition on top-viewed listings.
- Test price bands and offer structures.
- Improve first three images and primary product copy.
- Add stronger proof content (reviews, usage examples, guarantees).

### Revenue decline

- Separate volume decline from margin decline.
- Check whether top products are underperforming or traffic quality changed.
- Quantify refund impact and repeat return reasons.
- Trigger a recovery plan with owner and due date.

## Reporting template

Use this structure for stakeholder updates:

1. Executive summary (3-5 bullets)
2. Metric snapshot (revenue, orders, conversion, AOV, net margin)
3. Top/worst product movers
4. Root-cause hypotheses
5. Action plan for next cycle
6. Risks and escalation items

## Output format

When asked for help, provide:

- A period-over-period analysis summary.
- 3 prioritized actions with expected business impact.
- A compact KPI dashboard spec the team can track weekly.
