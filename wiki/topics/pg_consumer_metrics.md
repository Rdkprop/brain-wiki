---
title: PG Consumer Metrics 2026
type: topic
domain: work
created: 2026-06-01
updated: 2026-06-01
related_pages:
  - propertyguru
  - analytics_agent
  - lead_tracking_sot
confidence: high
sources:
  - raw/articles/Consumer Metrics - 2026 - Data Analytics & Activation.md
tags: [metrics, propertyguru, analytics, work]
---

# PG Consumer Metrics 2026

Official metric definitions from the Data Analytics & Activation (DAA) team. Applies across PGSG, PGMY, PGTH, IPPMY, PGCG unless noted. Priority levels: **p0** = must-have, **p1** = important, **p2** = nice-to-have.

---

## Audience Metrics

| Metric | Definition | Priority |
|--------|-----------|---------|
| **Visitors** | Distinct users with ≥1 page load in a given day. Includes logged-in + anonymous. | p2 |
| **MAU** | Distinct users with ≥1 page load in a given month, regardless of login status. | p0 |
| **MEU** (Monthly Engaged User) | Distinct users who viewed ≥3 LDPs in a given month. Filters out passive visits. Note: discussion to lower threshold to 2 LDPV to discount bots. | p0 |
| **Traffic by Page / Page Views by Page** | Distinct users who loaded each page type in a given day. Pages: HP, SRP (Buy/Rent), LDP, PLDP, NHL, APP, Guides, PDP, Developer Profile, Homeowner, Homeseller, My Property, Find Agents. | p1 |

---

## Search Metrics

| Metric | Definition | Priority |
|--------|-----------|---------|
| **Searches** | Total search queries submitted on the platform per day. | p0 |
| **Searchers** | Distinct users who performed ≥1 search per day. | p0 |
| **Searches by page type** | Breakdown by: Homepage, SRP, PDP, NHL, My Property. | p0 |
| **Searches by search type** | By search type category (freetext, filter, location, etc.). | p0 |
| **Zero Result Rate** | Share of searches returning no results. Signals inventory gaps, search quality issues, or unrealistic user expectations. | p0 (part of RI/Search) |

---

## Visibility Metrics

| Metric | Definition | Priority |
|--------|-----------|---------|
| **Real Impressions (RI)** | Count of listing cards where **100% of the info section** was visible in the user's viewport. Fired once per page load; no minimum view time. Event: `view_listing_card_v2`. | p0 |
| **RI by page type** | Breakdown: Homepage (Latest Projects, Handpicked For You), SRP (feed scroll), LDP (More Recommendations), PDP/Condo Directory, PLDP, NHL, Developer Profile. | p1 |
| **RI by listing type** | Agent listings vs project listings. | p1 |
| **RI per Search** | Avg real impressions per search query on SRP. Reflects search efficiency. Zero-result rate is a sub-metric here. | p0 |

---

## Listing Engagement Metrics

| Metric | Definition | Priority |
|--------|-----------|---------|
| **LDPV** | Total LDP loads per day, including repeat visits to the same listing. | p0 |
| **LDPV per Search** | Avg LDP views per search query, **scoped to SRP referrer only** (not all LDP views / all searches — that's misleading). | p0 |
| **LDPV / RI (CTR%)** | Two variants: (1) LDPV from SRP / SRP RI — loose (not session-bound); (2) Listing Clicks / SRP RI — strict in-session CTR. The strict metric should always be > loose. If not, investigate. | p0 |
| **LDPV by referrer type** | Breakdown: Platform search, Directory pages, Project/listing pages, Homepage, Search engine, Content/guidance, Agent directory, Others. | p1 |

---

## Lead Metrics

| Metric | Definition | Priority |
|--------|-----------|---------|
| **Total Unique Leads (TUL)** | Unique consumer enquiries, deduplicated by lead identity. **The canonical lead count** — use this everywhere. Source: LDM explore. Remove bots = Yes. | p0 |
| **Lead Starts** | Front-end lead events from Segment (`lead_v2`). Source: Consumer Master Explore → App Lead / Web Lead. | p0 |
| **Leads / RI (CVR%)** | Conversion from impression to lead. Use Total Unique Leads. Scope clearly: "SRP RI" vs all-surface RI. | p0 |
| **Leads / LDPV (CVR%)** | Conversion from page view to lead. Use Total Unique Leads. | p0 |
| **Leads per Active Listing** | Normalisation metric. North star for trend comparison. | p0 |
| **Leads per Active Agent** | Normalisation metric. North star for trend comparison. | p0 |
| **Leads by platform** | Web / mobile web / Android / iOS. | p0 |
| **Leads by user intent** | Buy / sell / rent / let / other. Proxy for demand-side vs supply-side pressure. | p0 |
| **Leads by source page** | SRP, LDP, APP, Others. | p0 |
| **Leads by lead type / action** | Call, WhatsApp, SMS, email, phone reveal, other. Includes E3.0 tracking. | p0 |

---

## Key Relationships & Conversion Funnel

```
Real Impressions → LDPV (CTR%) → Total Unique Leads (CVR%)
      ↑                ↑                   ↑
   Visibility      Discovery           Intent action
```

Normalisation metrics: **Leads per Active Listing**, **Leads per Active Agent** — use these when comparing across periods to control for supply changes.

---

## Notes & Caveats

- **MEU threshold**: Active discussion to use 2 LDPV (not 3) to better exclude bots.
- **LDPV/RI discrepancy**: Listing clicks on SRP / SRP RI should always be > LDPV from SRP / SRP RI — if it's inverted, investigate.
- **E3.0 Lead Starts / TUL** tracks conversion from WA button tap to sent message in PGWABA. Only applicable to `enquirer_type = consumer, guest`.
- **Active Listing / Active Agent** added as new 2026 metrics for normalisation.

See [[lead_tracking_sot]] for which system to use for each lead metric. See [[analytics_agent]] for how to query these in Looker.
