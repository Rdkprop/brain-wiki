---
title: PG Analytics Agent (Consumer Analytics Skill)
type: concept
domain: work
created: 2026-06-01
updated: 2026-06-06
related_pages:
  - pg_consumer_metrics
  - propertyguru
  - lead_tracking_sot
  - looker_vs_analytics_agent
confidence: high
sources:
  - raw/articles/Analytics Agent.md
tags: [analytics, propertyguru, work, looker, agent]
---

# PG Analytics Agent (Consumer Analytics Skill)

The Consumer Analytics skill is a defined capability for answering stakeholder questions from PropertyGuru's Looker/BigQuery data. It covers **consumer-side analytics only** across SG, MY, TH (with some explore-specific exceptions).

---

## What It Covers

| Category | Examples |
|----------|---------|
| **Users & Audience** | DAU/WAU/MAU trends, app vs web split, logged-in vs anonymous |
| **LDPV** | Views by platform/intent/referrer, top listings by LDPV |
| **Real Impressions** | Impression trends, page-type split, impression→view relationship |
| **Leads & Enquiries** | Total/unique leads, by channel/platform/listing/agent, experiment splits |
| **Search Behavior** | Distinct searches, by location/property type/intent, zero-result rate |
| **Engagement Events** | Clicks, shortlists, scroll depth, recommendations, login behavior |
| **Finance/Mortgage** | Navigation, calculator usage, price widget interactions (where supported) |
| **Marketing/Lifecycle** | App installs, Braze lifecycle, Turbo Pro attribution, WhatsApp first-touch |
| **Listing Aggregate** | Impressions→views→leads funnel, ad product impact, agent portfolio performance |
| **SG Transactions** | Resale/sub-sale/rental trends, transaction volume, agent participation |

## What It Does NOT Cover

- Agent subscriptions / inventory management
- Credits billing / revenue accounting
- Non-consumer operational reporting from other domain models

---

## How to Ask Well — 5 Elements

Always include:

1. **Market** — SG, MY, TH, all, or brand context (e.g., IPP MY)
2. **Date range** — explicit period (e.g., Jan–Mar 2026, last 90 days)
3. **Granularity** — day / week / month
4. **Platform** — web, app, or both
5. **Audience scope** — all users or logged-in only

**Good examples:**
- *SG, Jan–Mar 2026, monthly: top 10 residential projects by LDPV (sale and rent)*
- *MY, last 90 days, weekly: lead channel trend + unique lead split by platform*
- *TH, Q1 2026, monthly: search demand by province and user intent*

For **listing-level** analysis → explicitly say "listing-level" (routes to aggregate listing explores).
For **experiments** → explicitly say "experiment analysis" (routes to web lead explore).

---

## Important Data Caveats

- Some app explores **default to MY** unless market is explicitly set.
- **MAU explores** are pre-aggregated — may not use the normal date-selector pattern.
- **Search aggregate** may include all platforms — combining with web/app search explores can double-count.
- **Listing aggregate schema differs between SG and MY** — field names and available metrics are not identical.
- **Inactive listings** may show zero in narrow windows — may need a two-step "active window discovery."
- **Lead explore routing matters:**
  - All-market trend/channel → lead aggregate explore
  - Listing/agent detail → lead management explore
  - Experiment split → web lead explore

---

## Question Bank by Function

**Product**
- Is search quality improving? (zero-result rate, avg result count)
- Are recommendation surfaces driving incremental listing views?

**Consumer Marketing**
- Which channels/campaigns drove user growth last month?
- Did app installs convert into meaningful listing engagement?

**Business / Commercial**
- Which property segments/intents are growing fastest?
- How does SG compare with MY/TH for funnel efficiency?

**Sales / Agent-facing**
- Which listing portfolios generate the most unique leads?
- Which geographies have strong visibility but weak lead conversion?

**Leadership**
- Is top-of-funnel demand rising?
- Where are demand-to-conversion gaps?

See [[pg_consumer_metrics]] for the official metric definitions used in these analyses.

---

## Role in the Exploration Stack

As of mid-2026, the agent's designated role is **assisted exploration** — not the primary exploration interface yet. Looker Explores remain the fallback for power users and correctness-critical analysis.

The planned transition (per [[looker_vs_analytics_agent]]):
- **Phase 1 (now):** Agent complements Explores; dashboards are SOT
- **Phase 2:** Agent becomes the default entry point; Explores become the escape hatch
- **Phase 3:** Explores deprecated only once agent meets stability, accuracy, and coverage thresholds

**Reliability caveat:** occasional instability and routing issues currently prevent full replacement of Explores. This is the primary blocker for Phase 3.
