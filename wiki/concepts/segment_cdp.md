---
title: Segment CDP
type: concept
domain: work
created: 2026-06-08
updated: 2026-06-08
related_pages:
  - ga4_vs_segment
  - lead_tracking_sot
  - lcm_campaign_data
  - propertyguru
confidence: high
sources:
  - raw/notes/Segment Tracking.md
tags: [segment, cdp, tracking, analytics, propertyguru, work, instrumentation]
---

# Segment CDP

Segment is a **Customer Data Platform (CDP)**. It sits between your website/app and all downstream analytics and marketing tools. Instead of implementing Facebook Pixel, GA4, Braze, etc. separately, you instrument once with Segment and it fans the data out to all destinations.

```
Your Website/App
│
▼
Segment ──► Braze (CRM / push)
       ──► Google Analytics (web analytics)
       ──► Facebook Pixel (paid ads)
       ──► Amplitude / Mixpanel (product analytics)
       └──► Data warehouse (BigQuery, Snowflake)
```

Every call to `api.segment.io/v1/t` visible in Omnibug is one event being captured before fanout.

---

## The Two Call Types

| Type | Method | When it fires | Example |
|------|--------|--------------|---------|
| `track` | `analytics.track()` | Something happened | clicked a button, submitted a form |
| `page` | `analytics.page()` | A new page loaded | navigated to listing detail |

**Rule:** `page` = arrival. `track` = action.

---

## Anatomy of a Track Event

Every `track` call has the same skeleton:

```json
{
  "type": "track",
  "event": "EventName",          // The "what" — high level
  "properties": {
    "action": "specific_action", // The "what" — granular
    "pageType": "...",
    "pageRefererType": "...",
    "eventData": {},             // Action-specific payload
    "listingData": {},           // Object-specific payload (optional)
    "agentData": {}              // Entity-specific payload (optional)
  },
  "context": {                   // Auto-collected by SDK
    "page": { "url": "...", "referrer": "...", "title": "..." },
    "userAgent": "...",
    "locale": "...",
    "timezone": "..."
  },
  "userId": "...",               // Logged-in ID
  "anonymousId": "...",          // Browser-assigned UUID
  "timestamp": "...",
  "messageId": "...",            // Unique ID for this event
  "integrations": {}             // Destination on/off switches
}
```

---

## The Four Questions Every Event Answers

| Question | Fields |
|----------|--------|
| **Who?** | `userId`, `anonymousId`, `memberType`, `umstId` |
| **Where?** | `pageType`, `pageRefererType`, `context.page.url`, `context.page.referrer` |
| **What?** | `event`, `action`, `eventData` |
| **About what?** | `listingData`, `agentData`, `searchId`, or equivalent object data |

If you can locate each of these four in an unfamiliar event payload, you understand the event.

---

## Identity: Who Is This Person?

### `userId`
- The logged-in user's ID in the company's own database
- Only exists post-login; consistent across devices once authenticated
- Used to stitch anonymous sessions to real accounts

### `anonymousId`
- A UUID Segment auto-generates and stores in a browser cookie
- Exists from the very first visit, before any login
- Lost if user clears cookies

### Identity Stitching
When a user logs in, Segment calls `analytics.identify()`, linking the `anonymousId` to the `userId`. All prior anonymous events retroactively belong to that user in downstream tools.

> A user who browses 5 listings anonymously, then logs in and submits a lead — the company can see the full pre-login journey.

### PG-Specific ID Fields

| Field | What it is |
|-------|-----------|
| `umstId` | PG's internal user ID — joins to their own DB |
| `gaClientId` | Google Analytics client ID — joins Segment data to GA4 |
| `gaSessionId` | GA session identifier |
| `gaSessionNumber` | How many sessions this user has had total |

---

## Navigation: Where Did They Come From?

- **`context.page.referrer`** — raw URL of the previous page, set by the browser
- **`pageRefererType`** — human-readable classification (e.g. `Listing Search`, `Homepage`)
- **`pageType`** — classification of the **current** page

> Rule: `pageType` = where you are. `pageRefererType` = where you came from.

Raw URLs change (query params, slugs). Classified types are stable for analytics grouping.

---

## Event Naming Patterns

**Pattern 1 — Unique event name per action** (clean, easy to query):
```
Lead_v2 / ListingClick / ClickContactAgent / SearchFilterEngagement
```

**Pattern 2 — Generic event + `action` discriminator** (one name covers a family):
```
event: "agent_profile_page", action: "click_profile_past_transactions"
event: "agent_profile_page", action: "click_operats_in_hdb_view_all"
```

> Watch out: Pattern 2 events require filtering by both `event` AND `action` in dashboards.

---

## The searchId Pattern (Session Stitching)

When a user submits a search, a unique `searchId` is generated and carried on every downstream event — card impressions, clicks, listing views, lead submissions.

```
Search_v2 ──► searchId: "b4d140..."
│
▼
View_Listing_Card ──► searchId: "b4d140..."
│
▼
ListingClick ──► searchId: "b4d140..."
│
▼
Lead_v2 ──► searchId: "b4d140..."
```

**This lets you answer:**
- Which search queries produce the most leads?
- Which filters correlate with higher conversion?
- What was the rank position of the listing that converted?
- Did a zero-result search kill this funnel?

---

## Key Tracking Patterns

### Scroll Tracking
PG fires a `Scroll` event each time a content section enters the viewport (`listingSection: "Overview"`, `"Property Details"`, etc.). Enables content engagement heatmaps and scroll depth as a lead quality signal.

### Soft Conversions
`Shortlist_v2` / Save / Favourite events carry the same `listingData` depth as a lead. Classic use: "what gets shortlisted but never leads?" — an intent-without-conversion segment.

### CTA Location Tracking (`loc`)
When the same action is available in multiple UI places, `loc` records which instance was used (e.g. `"Sticky Card"` vs `"Rightside Card"`). Answers "which placement converts better?" without needing separate event names.

### Intent vs Conversion Gap
`ClickContactAgent` (intent) fires before `Lead_v2` (conversion), letting teams measure form-open-but-not-submitted drop-off.

### A/B Experiments
`experiment: "feature_name=Control|other_feature=Test"` travels on every event. Multiple experiments pipe-separated. Enables segmenting any metric by variant.

### Progressive Data Enrichment
Object fields aren't always identical across funnel stages. E.g. `listingData.greenScore` appears on `ListingDetailPage_v2` but not on `View_Listing_Card_v2`. Don't flag a null field as broken before checking whether it appears later in the funnel.

### Timestamp Ordering
Always sort events by `timestamp`, not by capture/arrival order. Network requests can resolve out of order under retries or variable latency.

---

## Destination Routing

**Bundled (client-side):** Event delivered directly in the browser via the destination's own JS SDK. Visible in Omnibug. Common: Facebook Pixel, GA4, Bing Ads.

**Unbundled (server-side):** Segment's servers forward the event after the browser call. Not visible in Omnibug. Used for sensitive tools or when enrichment is needed first.

**Per-event routing control:**
```json
"integrations": { "Braze Web Mode (Actions)": false }
```
Reasons to block a destination: event fires too frequently (e.g. Scroll), tool handles it server-side, or data privacy constraints.

---

## Spec vs Reality Gap

The tracking spec is what the team *planned*. What fires in production is what's *actually* implemented. Common gaps:

| Gap type | Example | Impact |
|----------|---------|--------|
| Missing properties | `agentData` absent from `Lead_v2` | Can't analyse agent quality vs leads |
| Undocumented events | `agent_profile_page` not in spec | Shadow tracking — unreliable |
| Typos in event names | `click_operats_in_hdb` | Breaks queries |
| Empty `eventData` | `eventData: {}` on click events | Can't tell what was clicked |
| Dual events on same action | `lead_v2` + `lead` (LDM) | Risk of double-counting |

Audit: trigger every spec'd event manually with Omnibug open. Compare `properties` to spec.

---

## Quick Checklist for an Unknown Event

- [ ] What is `event`? What is `action`? (What happened?)
- [ ] What is `pageType` / `subPageType`? (Where, and how specifically?)
- [ ] What is `pageRefererType`? (Where did they come from?)
- [ ] Is there a `searchId`? (Can I join this to upstream events?)
- [ ] What's in `eventData`? (What are the specifics?)
- [ ] Is there a `loc` / `placement` field? (Which UI instance?)
- [ ] What object data is attached? (`listingData`, `agentData`) — enriched as expected?
- [ ] Which destinations got it? (Is Braze blocked? Why?)
- [ ] Is `isSymbiosis` true? (Internal traffic — should I filter out?)
- [ ] What experiment variant is active?
- [ ] Am I sorting by `timestamp` (correct) or capture order (unreliable)?

---

## Tools

| Tool | Purpose |
|------|---------|
| **Omnibug** (browser extension) | Intercepts and displays all analytics calls in real time |
| **Segment Debugger** | Live event stream in Segment's own UI |
| **Charles Proxy / mitmproxy** | Deep network inspection including HTTPS |
| **browser DevTools → Network** | Raw XHR/fetch calls to `api.segment.io` |

See [[ga4_vs_segment]] for how Segment's identity model compares to GA4's. See [[lead_tracking_sot]] for how Segment fits into the lead SoT hierarchy.
