---
title: PropertyGuru
type: entity
domain: work
created: 2026-06-01
updated: 2026-06-01
related_pages:
  - pg_consumer_metrics
  - analytics_agent
  - lead_tracking_sot
  - ga4_vs_segment
confidence: high
sources:
  - raw/articles/Analytics Agent.md
  - raw/articles/Consumer Metrics - 2026 - Data Analytics & Activation.md
tags: [propertyguru, work, entity]
---

# PropertyGuru

Southeast Asia's leading property marketplace. Operates under the brands **PGSG** (Singapore), **PGMY** (Malaysia), **PGTH** (Thailand), **IPPMY** (Malaysia secondary brand), and **PGCG** (Commercial Gateway).

## Data & Analytics Context

The **Data Analytics & Activation (DAA)** team owns consumer analytics, data definitions, and reporting infrastructure. Key systems:

| System | Purpose |
|--------|---------|
| **Segment CDP** | Client-side event collection and identity stitching |
| **LDM (Lead Data Management)** | Server-side lead ingestion — source of truth for lead counts |
| **GA4** | Session tracking + marketing attribution (web) |
| **AppsFlyer** | Marketing attribution (app) |
| **BigQuery** | Data warehouse |
| **Looker** | BI / self-serve analytics |

## Key Wiki Pages

- [[pg_consumer_metrics]] — official metric definitions (MAU, MEU, LDPV, TUL, Real Impressions, etc.)
- [[analytics_agent]] — what the Consumer Analytics skill can answer and how to ask well
- [[lead_tracking_sot]] — lead lifecycle, E1/E2/E3 models, LDM vs Segment vs GA4 rules
- [[ga4_vs_segment]] — why user counts from Segment and GA4 never match
