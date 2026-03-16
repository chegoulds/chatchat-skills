---
id: clawver-print-on-demand
name: Clawver Print On Demand
description: Guidance-only playbook for planning and operating print-on-demand merchandise workflows, including variants, pricing, fulfillment, and shipping operations.
category: Business
requires: []
examples:
  - "Help me with clawver print on demand."
  - "Use clawver-print-on-demand for this task."
version: "1.3.0"
---

# Clawver Print-on-Demand

This is a guidance-only skill for planning and operating print-on-demand merchandise workflows. It does not execute API calls.

## When to use

- You are launching merch without holding inventory.
- You need variant strategy for size/color-based products.
- You want a repeatable fulfillment and shipping operations checklist.

## How print-on-demand works

1. Create product definitions mapped to supplier catalog variants.
2. Publish product listings with clear media and pricing.
3. Customer orders trigger supplier fulfillment.
4. You track status until delivery and manage support issues.

## Core planning rules

### Product and variant modeling

- Treat supplier product and variant IDs consistently and avoid mixed formats.
- Define one storefront variant per buyer-visible option (size/color bundle).
- Use buyer-friendly variant names and normalized size labels.
- Ensure each variant has a unique supplier variant mapping.

### Activation readiness

Before activating a product, verify:

- Variant list is complete and non-empty.
- Prices and margins are validated by variant.
- Listing images/mockups represent the actual print area.
- Stock/availability metadata is current for each variant.

### Design asset governance

- Keep source artwork and production exports versioned.
- Track placement intent (front/back/default) in metadata.
- Document which design files map to which variants.
- Keep fallback imagery so listings remain stable if mockups fail.

### Pricing and margin controls

- Compute margin per variant, not just at product level.
- Include platform fee, production base cost, and expected refund rate.
- Apply tiered pricing where larger sizes have higher costs.
- Recheck margins after supplier price changes.

## Fulfillment operations

### Status model

Track order and payment state separately:

- Order: confirmed, processing, shipped, delivered
- Payment: paid, partially refunded, refunded

### Monitoring checklist

- Confirm every paid order enters production.
- Flag orders stuck in processing beyond SLA.
- Capture and surface tracking URL and tracking number.
- Escalate exceptions (misprint, lost shipment, delayed delivery).

### Customer support playbook

- Send proactive shipping updates for delayed orders.
- Offer clear replacement/refund policy for quality issues.
- Capture root cause to reduce repeated failures.

## Risk controls

- Avoid publishing products with incomplete variant coverage.
- Do not allow checkout on out-of-stock variants.
- Verify mockup accuracy before campaigns.
- Audit webhook/event handling logic in the implementation layer.

## Output format

When asked for help, provide:

- A launch checklist (catalog, variants, pricing, media, activation).
- A fulfillment monitoring checklist (status, SLAs, escalation triggers).
- A support SOP snippet for delays, defects, and refunds.
