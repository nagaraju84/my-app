---
title: "Product Brief: POS-to-Franchise-Billing Customer Sync Service"
status: draft
created: 2026-09-26
updated: 2026-09-26
---

# Product Brief: POS-to-Franchise-Billing Customer Sync Service

## Executive Summary

This is an internal API service for a laundry/dry-cleaning business [ASSUMPTION: a single location or small chain, franchise-affiliated] that keeps customer and order data consistent between **washdryclean-crm** (the shop's customer/order-tracking system) and the franchise's billing system [ASSUMPTION: billing system name still unconfirmed]. Today these two systems don't talk to each other automatically [ASSUMPTION: via manual entry, exports, or no sync at all], so customer records and order/billing data can drift apart — causing billing errors, mismatched customer balances, or manual reconciliation work. The service becomes the automated pathway that keeps the franchise billing system accurately reflecting what happens at the POS.

## The Problem

Customer and order data is captured in washdryclean-crm when a customer drops off or picks up items, but the franchise's billing system — which the franchise likely requires for royalty reporting, centralized invoicing, or customer account/loyalty tracking [ASSUMPTION] — doesn't automatically receive that data. [ASSUMPTION: Today this is bridged manually, or not bridged at all, meaning billing data is stale, incomplete, or requires manual double-entry by shop staff.]

The cost: billing/royalty discrepancies with the franchisor, customer complaints over incorrect balances or missed charges, and staff time spent reconciling two systems that should agree. [ASSUMPTION: There may also be a compliance angle — franchise agreements often require accurate, timely reporting to the franchisor, which is at risk without a reliable sync.]

## The Solution

An API service that connects washdryclean-crm to the franchise-specific billing system:

- Syncs customer records and order/transaction data from washdryclean-crm to the franchise billing system [ASSUMPTION: one-way, washdryclean-crm as source of truth for what happened at the counter].
- Runs on [ASSUMPTION: per-transaction/real-time sync, since billing accuracy depends on timely data] — a scheduled batch alternative (e.g., end-of-day) is the fallback if the franchise billing system's API only supports periodic uploads.
- Exposes basic status/error visibility so shop staff or the owner can tell when a sync has failed, rather than discovering it when a bill is wrong.

This is backend/integration infrastructure — no new customer-facing surface. Washdryclean-crm remains what staff use day-to-day; this service works invisibly behind it.

## What Makes This Different

There's likely no real alternative today beyond manual entry or doing without — this isn't competing with another integration, it's replacing a gap. [ASSUMPTION: If the franchise already provides or mandates a specific integration/sync tool, that changes the scope significantly and should replace this assumption.] The advantage here is narrow and practical: it removes a specific, recurring manual burden for this one shop (or small chain), not a broad platform play.

## Who This Serves

**Primary: shop owner/operator and staff** who currently deal with the gap between POS and franchise billing — fewer billing errors, less manual reconciliation, more confidence that royalty/billing reports to the franchisor are accurate. [ASSUMPTION: this is a small operation — likely one or a handful of locations, not a large franchisee group with dedicated IT staff.]

**Secondary: the franchisor**, indirectly — accurate, timely data improves the relationship and reduces disputes over royalty/billing figures.

## Success Criteria

- Customer/order data in the franchise billing system matches the POS reliably, with mismatches trending to zero.
- Manual reconciliation or double-entry work for billing is eliminated or sharply reduced.
- Billing/royalty reports submitted to the franchisor are accurate without manual correction. [ASSUMPTION — confirm if franchisor reporting is actually in scope, or if "billing system" refers only to customer-facing billing.]
- Sync failures are visible and addressed quickly rather than discovered via a customer complaint or a franchisor audit.

## Scope

**In for v1:**
- One-way sync of customer and order/transaction data from washdryclean-crm to the franchise billing system.
- Failure visibility (logging/alerting) so a broken sync doesn't go unnoticed.
- Handling whatever data format/API the franchise-specific billing system requires. [ASSUMPTION: this may be the hardest and least flexible part of the project — the franchise system's API/integration constraints, not this service's design, will likely dictate a lot of the technical approach.]

**Explicitly out for v1:**
- Data flowing back from billing to washdryclean-crm (e.g., payment status affecting its records) — revisit if needed.
- Supporting multiple different CRM/POS or billing systems — this is scoped to the one pairing in use today.
- Any new customer-facing feature (loyalty, notifications, etc.).

## Open Questions

Everything below is unconfirmed and should be resolved before this becomes a PRD:

- What is the actual name/product of the franchise billing system, and does it expose an API, file-based import, or something else? This materially affects feasibility and design.
- Does washdryclean-crm expose a usable API/webhook for reading customer and order data, or does this need to query its database/exports directly?
- Does the franchise billing system need data back from washdryclean-crm at all (e.g., payment status), or is this purely one-way?
- Real-time per-transaction sync vs. a scheduled batch — does the franchise billing system's integration even support real-time, or is batch the only option?
- Is this brief for your own planning before building, or is it going to someone else (a franchise partner, an owner who isn't you) for buy-in?
- Is franchisor royalty/reporting actually part of what "billing system" means here, or is this closer to just customer invoicing?

## Vision

If this works well for one shop, the same sync pattern likely generalizes to any additional locations under the same franchise umbrella [ASSUMPTION], removing the same manual burden wherever it's replicated — without necessarily becoming a broader product beyond this franchise relationship.
