---
title: LCM Campaign Data
type: topic
domain: work
created: 2026-06-08
updated: 2026-06-08
related_pages:
  - daa_team
  - active_home_seeker
  - analytics_agent
  - propertyguru
  - segment_cdp
confidence: medium
sources:
  - raw/daily/Mon June 8th.md
tags: [propertyguru, work, lcm, campaign, braze, segmentation, lifecycle]
---

# LCM Campaign Data

Lifecycle & Campaign Management (LCM) data infrastructure at PropertyGuru. As of June 2026, campaign-related data is **scattered across multiple sources**, making analysis and targeting difficult. Mounica owns this workstream.

---

## Two Distinct Problem Areas

### 1 — Campaign Analysis Side
- Pre/post campaign performance
- Metrics and evaluation framework
- Goal: unified view of campaign effectiveness

### 2 — Targeting Side
- Who campaigns are being run on
- How data flows **to and from Braze**
- How internal segmentation (e.g., [[active_home_seeker|Home Seekers]], rental seekers) can be **pushed into Braze** for audience targeting

---

## Dataset Being Built

A new dataset is being created to bridge the targeting gap:

| Field | Description |
|-------|-------------|
| `date` | Event date |
| `anonymous_id` | User identifier |
| `user_sub_segmentation` | home seeker, rental seeker, etc. |

This enables CRM/lifecycle campaigns to target based on behavioural segmentation rather than just demographic or explicit signals.

---

## Braze Integration Plan

**Key dependencies:**
- **Shariq** — how to export segmentation data to Braze
- **Inho** — how the exported data should be used in campaigns

**Current state:** integration not yet live. Discussion in progress.

---

## Data Visibility Work

Mounica is improving data visibility in **BigQuery / Looker Explore** to make campaign analysis more accessible. Objective: reduce friction for analysts pulling campaign data.

---

## Home Seeker Drop (June 2026)

An anomaly was flagged in the June 8 sync:
- **Home Seekers count dropped** in recent data
- **Lead volume did not drop correspondingly**

This mismatch raised concern about:
- **Instrumentation issues** in the home seeker pipeline
- **Logic / pipeline errors** in the segmentation definition

**Actions assigned to Mounica (by Thursday):**
1. End-to-end sanity check: instrumentation + pipeline logic
2. Investigate root cause of drop
3. Explore lead quality trends (since lead volume is stable, quality may have shifted)
4. Define analytical framework for home seeker trends + lead quality

See [[active_home_seeker]] for the full segment definition and KPIs.

---

## Pipeline Fix (June 2026)

A data issue in Looker was identified and fixed. Root cause: pipeline refresh timing didn't align with mid-month reporting needs. Suggested improvement: move to more frequent refreshes (e.g., weekly cadence).

---

## Strategic Gap

Consumer engagement work overall lacks a clear North Star. Ongoing efforts in LCM/lifecycle risk producing analysis without tied outcomes. Rohan to set up alignment with Vasu on:
- What success looks like for consumer engagement
- Expected outcomes from current workstreams

See [[daa_team]] for broader team context and bandwidth constraints.
