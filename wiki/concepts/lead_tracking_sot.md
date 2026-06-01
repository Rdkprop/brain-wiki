---
title: Lead Tracking — Single Source of Truth
type: concept
domain: work
created: 2026-06-01
updated: 2026-06-01
related_pages:
  - pg_consumer_metrics
  - ga4_vs_segment
  - propertyguru
  - analytics_agent
confidence: high
sources:
  - raw/articles/Leads Playbook Establishing a Single Source of Truth for Lead Tracking and Attribution - Data Analytics & Activation.md
tags: [leads, propertyguru, analytics, work, sot]
---

# Lead Tracking — Single Source of Truth

Establishes which system to trust for which lead question at PropertyGuru. The core problem: lead data exists in Segment, LDM (server-side), GA4, and AppsFlyer — and they will never match. This page explains what each is for.

---

## The Rule

| Question | Use | Why |
|----------|-----|-----|
| How many leads did we get? | **LDM** | Server-side, bot-removed, deduplicated. This is TUL. |
| Why did leads drop? (funnel debug) | **Segment** | Event-level visibility into where drop happened |
| Which channel/campaign drove leads? | **GA4** (web) / **AppsFlyer** (app) | Attribution layer |
| RCA sequence | GA4 to identify → Segment to debug → LDM for business impact | |

**Never compare absolute numbers between LDM, GA4, and Segment. Compare trends and directional alignment only.**

---

## Lead Lifecycle

```
Generation → Ingestion → Identification → Enrichment → Distribution → Reporting
   |              |             |               |              |             |
User submits   Segment +    UMSTID /        Lead Quality   LDM /        BigQuery +
form / WA      LDM capture  phone /         Preferences    Agentnet     Looker
               event        anonymous       Insights
```

---

## Communication Models (E1.0 / E2.0 / E3.0)

| Aspect | E1.0 (Direct Agent) | E2.0 (Lead Gating) | E3.0 (No Gate) |
|--------|--------------------|--------------------|----------------|
| Flow | User → click → direct call/WA to agent | User → pass gate (login/phone/WABA) → lead created | User → can contact directly OR via WABA |
| Lead creation | ❌ Not guaranteed | ✅ Always created | ⚠️ Partial (only via WABA/backend) |
| User ID | ❌ Mostly anonymous | ✅ Strong (UMSID, phone, email) | ⚠️ Weak |
| Phone capture | ❌ No | ✅ Mandatory | ⚠️ Optional |
| Tracking reliability | ❌ Low (loss is inherent) | ✅ High | ⚠️ Medium |
| Failure visibility | ❌ Invisible | ✅ Trackable | ⚠️ Partial |

**Current market status (as of early 2026):**
- SG: E2.0 → E3.0 (Feb 2026)
- MY: E1.0 → E3.0 (Apr 2026)
- TH: E1.0 → evolving

---

## Market Coverage Gaps

| Area | Gap |
|------|-----|
| Agent Profile Pages | Missing `lead_v2` in MY, TH |
| Find Agent Page | Missing in TH (LDM + events) |
| App Attribution | Cannot send Segment → GA4 (use AppsFlyer instead) |
| Developer Leads (IPP) | WIP / no clarity on SoT |

---

## Data Sources Quick Reference

**LDM (Primary SoT) — BigQuery tables:**

| Market | BQ Table |
|--------|---------|
| PGSG | `propertyguru-datalake-v0.sg_lead_server.lead` |
| PGMY | `propertyguru-datalake-v0.my_lead_server.lead` |
| IPPMY | `propertyguru-datalake-v0.th_lead_server.lead` |
| PGTH | `propertyguru-datalake-v0.ipp_lead_server_v2.lead` |

**Segment (Event layer) — BigQuery tables:**

| Market | Web | App |
|--------|-----|-----|
| PGSG | `sg_segment_consumer.lead_v2` | `sg_segment_consumer_android/ios_v2.lead_v2` |
| PGMY | `my_segment_consumer.lead_v2` | `my_segment_consumer_android/ios_v2.lead_v2` |
| IPPMY | `ipp_segment_consumer_web.lead_v2` | `ipp_consumer_ios/android.vw_lead` |
| PGTH | `th_segment_consumer.lead_v2` | `th_segment_consumer_android/ios_v2.lead_v2` |

---

## Future State

Channel attribution in LDM (for web) is under development. Once complete, LDM will also handle marketing attribution — at which point marketing teams can fully consolidate onto LDM as SoT.

See [[ga4_vs_segment]] for the deeper identity/session tracking comparison. See [[pg_consumer_metrics]] for the TUL metric definition.
