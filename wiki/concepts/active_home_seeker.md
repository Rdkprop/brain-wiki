---
title: Active Home Seeker
type: concept
domain: work
created: 2026-06-01
updated: 2026-06-01
related_pages:
  - pg_consumer_metrics
  - analytics_agent
  - propertyguru
confidence: high
sources:
  - raw/notes/Active Home seeker.md
tags: [propertyguru, work, segmentation, home-seeker, kpi]
---

# Active Home Seeker

A defined user segment at PropertyGuru representing high-intent property searchers. The **primary source of leads** on the platform despite being a minority of total visitors.

---

## Definition

A user who, in the **past 30 days**, has:
- **≥ 3 Searches**, AND
- **≥ 4 Listing Detail Page Views (LDPVs)**

**Why this over a pure LDPV threshold:**
- Combines intent (search) + engagement (views)
- Less prone to contamination than a "50+ LDPVs" rule
- Better aligns with real consumer behaviour and lead generation patterns
- 50+ LDPVs is treated as an *outcome / average*, not a qualification rule

**Relationship to [[pg_consumer_metrics]]:**
- MEU (Monthly Engaged User) = ≥3 LDPVs in a month (broader, no search requirement)
- Home Seeker = ≥3 searches AND ≥4 LDPVs (narrower, higher intent)
- Home Seekers are a subset of MEU

---

## Scale (Monthly)

| Month | Home Seekers |
|-------|-------------|
| Oct 2025 | ~240K |
| Nov 2025 | ~221K |
| Dec 2025 | ~209K |
| Jan 2026 | ~258K |

---

## Engagement Characteristics

| Metric | Median | Average | Note |
|--------|--------|---------|------|
| LDPVs per Home Seeker | ~18 | ~50 | Right-skewed by heavy users |
| Leads per Home Seeker | 1 | ~0.8 | Average < median due to conversion sparsity |

Distribution is **right-skewed**: a small cohort generates hundreds to thousands of LDPVs — likely bots, scrapers, or agents. Risk: inflated baseline leading to unrealistic targets.

**Target-setting rule:** use **median for engagement depth**, **average for leads**.

**Agreed directional targets:**
- Median LDPVs / Home Seeker: 18 → 22 (+20%)
- Avg Leads / Home Seeker: 0.8 → 1.0

---

## Core KPIs

| KPI | Definition | Note |
|-----|-----------|------|
| **Home Seekers (count)** | Monthly unique users meeting the ≥3 search + ≥4 LDPV threshold | Absolute + MoM trend |
| **% Home Seekers of Total Visitors** | Home Seekers ÷ Total Monthly Unique Visitors | First-order indicator of demand depth |
| **Median LDPVs / Home Seeker** | Engagement depth KPI | Preferred over avg due to skew |
| **Avg Leads / Home Seeker** | Conversion KPI | Tracks monetizable value per high-intent user |

---

## Supporting / Diagnostic KPIs

| KPI | Definition | Why |
|-----|-----------|-----|
| **LDPV Distribution** | % of users in buckets: 1–10, 11–20, 21–50, 51–100, 100+ | Spots abnormal spikes |
| **Leads Contribution Share** | % of total leads from Home Seekers | Expected ~90%+ |
| **Extreme Activity Monitor** | % of Home Seekers with >500 or >1000 LDPVs | Bot / scraper quality guardrail |
| **Email Coverage Rate (high-LDPV)** | % of extreme-LDPV users with valid emails vs baseline | Identifies anomalies in high-activity users |

---

## Phase 2 (Planned)

Mid-Intent vs High-Intent Home Seeker segmentation based on higher search + view thresholds — useful for targeted experiments and funnel optimisation.
