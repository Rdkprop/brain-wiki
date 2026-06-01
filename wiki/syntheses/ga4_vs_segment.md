---
title: GA4 vs Segment — Identity & Session Tracking
type: synthesis
domain: work
created: 2026-06-01
updated: 2026-06-01
related_pages:
  - lead_tracking_sot
  - propertyguru
  - pg_consumer_metrics
confidence: high
sources:
  - raw/articles/Comparison GA4 vs. Segment Identity & Session Tracking - Data Analytics & Activation.md
tags: [ga4, segment, analytics, identity, propertyguru, work]
---

# GA4 vs Segment — Identity & Session Tracking

**Core distinction:**
- **Segment** = identity-first → *"Who are our users?"*
- **GA4** = reporting-first → *"How much traffic and engagement do we have?"*

---

## Why the Numbers Will Never Match

Three reasons user counts differ:

### 1. Identity Fragmentation (biggest driver)
- GA4 (default): counts users at device/browser level via `user_pseudo_id`
- Segment: stitches multiple `anonymous_id`s to one `user_id` after login

**Same person on laptop + mobile =**
- GA4 → 2 users
- Segment → 1 user (if `identify()` was called on both)

GA4 can bridge users only if `user_id` or Google Signals is properly implemented — which is rarely perfect.

### 2. Different Units — Sessions vs Users
- GA4 is **session-centric** in reporting
- Segment is **user/event-centric**

One Segment user can generate multiple GA4 sessions. Comparing GA4 Sessions vs Segment Users is not valid.

### 3. Cookie / Storage Reset Behaviour
Both tools reset identity on cookie loss. But Segment can **retroactively unify users** after login via `identify(user_id)`. GA4 cannot fully reconcile historical fragmentation unless `user_id` was already in place.

---

## Identity Models Side by Side

| Aspect | Segment | GA4 |
|--------|---------|-----|
| Core philosophy | Identity-first (user-centric) | Reporting-first (traffic & sessions) |
| Default identifier | `anonymous_id` (per browser/device) | `user_pseudo_id` (per device/browser) |
| Logged-in identifier | `user_id` | `user_id` (optional) |
| Cross-device tracking | ✅ Strong (if `identify` implemented) | ⚠️ Limited (needs `user_id` or Google Signals) |
| Sessions | ❌ No native sessions | ✅ Native (`ga_session_id`) |
| User unification | Flexible — you define downstream | Built-in (depends on GA4 identity settings) |
| Raw data access | ✅ Full event stream | ⚠️ BigQuery export (sampling may apply) |
| Marketing attribution | ❌ Not native | ✅ Strong (channels, campaigns, ads) |
| Source of truth | ✅ Recommended | ❌ Not ideal as SoT |

**BigQuery note:** GA4 unique session = `user_pseudo_id + ga_session_id` (neither field alone is globally unique).

---

## Practical Guidance — When to Use Which

| Question | Tool |
|----------|------|
| How many unique people used our product? | **Segment** |
| What is our MAU / DAU? | **Segment** |
| Which marketing channel drove the most traffic? | **GA4** |
| How many sessions did we have? | **GA4** |
| Where are users dropping off in a funnel? | **Segment** |
| App attribution | **AppsFlyer** (Segment → GA4 forwarding not available for app) |

---

## PG-Specific Note

At PropertyGuru, **Segment is the source of truth for user counts (MAU, DAU, MEU)**. GA4 is used for web marketing attribution only. For lead counts, neither — use [[lead_tracking_sot]] (LDM is the SoT for leads).
