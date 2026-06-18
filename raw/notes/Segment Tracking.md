

`# Segment Tracking — How Product Analytics Works`

`> **Type:** Evergreen Reference`

`> **Tags:** #analytics #segment #tracking #product #data`

`> **Related:** [[A/B Testing]], [[Funnel Analysis]], [[Data Destinations]]`

`---`

`## What is Segment?`

`Segment is a **Customer Data Platform (CDP)**. It sits between your website/app and all your analytics/marketing tools. Instead of implementing Facebook Pixel, Google Analytics, Braze, etc. separately, you instrument once with Segment and it fans the data out everywhere.`

` ``` `

`Your Website`

`│`

`▼`

`Segment ──► Braze (CRM / push)`

`│ ──► Google Analytics (web analytics)`

`│ ──► Facebook Pixel (paid ads)`

`│ ──► Amplitude / Mixpanel (product analytics)`

`└──── ──► Your data warehouse (Snowflake, BigQuery)`

` ``` `

``Everything you see in Omnibug going to `api.segment.io/v1/t` is one event being captured before it gets fanned out.``

`---`

`## The Two Call Types`

`| Type | Segment method | When it fires | Example |`

`|---|---|---|---|`

``| `track` | `analytics.track()` | Something happened | clicked a button, submitted a form |``

``| `page` | `analytics.page()` | A new page loaded | navigated to listing detail |``

``> **Rule of thumb:** `page` = arrival. `track` = action.``

`---`

`## Anatomy of a Track Event`

``Every `track` call has the same skeleton. Learn this once, read any company's events.``

` ```json `

`{`

`"type": "track",`

`"event": "EventName", // The "what" — high level`

`"properties": { // All the detail`

`"action": "specific_action", // The "what" — granular`

`"pageType": "...",`

`"pageRefererType": "...",`

`"eventData": { }, // Action-specific payload`

`"listingData": { }, // Object-specific payload (optional)`

`"agentData": { } // Entity-specific payload (optional)`

`},`

`"context": { // Auto-collected by SDK`

`"page": { "url": "...", "referrer": "...", "title": "..." },`

`"userAgent": "...",`

`"locale": "...",`

`"timezone": "..."`

`},`

`"userId": "...", // Logged-in ID`

`"anonymousId": "...", // Browser-assigned ID`

`"timestamp": "...",`

`"messageId": "...", // Unique ID for this event`

`"integrations": { }, // Destination on/off switches`

`"_metadata": { "bundled": [] } // Which destinations got it`

`}`

` ``` `

`---`

`## The Four Questions Every Event Answers`

`A useful mental model — every event simultaneously answers:`

`| Question | Fields |`

`|---|---|`

``| **Who?** | `userId`, `anonymousId`, `memberType`, `umstId` |``

``| **Where?** | `pageType`, `pageRefererType`, `context.page.url`, `context.page.referrer` |``

``| **What?** | `event`, `action`, `eventData` |``

``| **About what?** | `listingData`, `agentData`, `searchId`, or equivalent object data |``

`If you can locate each of these four in an unfamiliar event payload, you understand the event.`

`---`

`## Identity: Who Is This Person?`

`` ### `userId` ``

`- The logged-in user's ID in the company's own database`

`- Only exists post-login`

`- Consistent across devices once authenticated`

`- **Used to:** stitch anonymous sessions to real accounts, personalise comms`

`` ### `anonymousId` ``

`- A UUID Segment auto-generates and stores in a browser cookie`

`- Exists from the very first visit, before any login`

`- Lost if user clears cookies`

`- **Used to:** track behaviour before login, attribute conversion to anonymous sessions`

`### Identity Stitching`

``When a user logs in, Segment calls `analytics.identify()`, linking the `anonymousId` to the `userId`. All prior anonymous events retroactively belong to that user in downstream tools.``

`> **Why it matters:** A user who browses 5 listings anonymously, then logs in and submits a lead — the company can see the full pre-login journey.`

`### Other ID Fields`

`Companies often pass their own internal IDs alongside Segment's:`

`| Field | What it is |`

`|---|---|`

``| `umstId` | PG's internal user ID, used to join to their own DB |``

``| `gaClientId` | Google Analytics client ID — joins Segment data to GA4 |``

``| `gaSessionId` | GA session identifier |``

``| `gaSessionNumber` | How many sessions this user has had total |``

`---`

`## Navigation: Where Did They Come From?`

``### `referrer` (raw, in `context.page`)``

`The full URL of the previous page — set automatically by the browser. Tells you exactly which page the user was on before navigating here.`

``### `pageRefererType` (enriched, in `properties`)``

``A human-readable classification of the referrer page. Instead of a raw URL, the engineering team maps it to a label like `Homepage`, `Listing Search`, `Agent Directory`.``

`` > **Raw:** `https://example.com/search?district=D15&type=condo` ``

`` > **Enriched:** `Listing Search` ``

`` ### `pageType` ``

`The classification of the **current** page.`

``> **Rule:** `pageType` = where you are. `pageRefererType` = where you came from.``

`### Why Both?`

`Raw URLs change (query params, slugs). Classified types are stable for analytics grouping and funnel analysis.`

`---`

`## Event Naming Patterns`

`Companies use two main patterns. Learn both.`

`### Pattern 1: Unique event name per action`

`Each action has its own event name. Clean, easy to query.`

` ``` `

`Lead_v2`

`ListingClick`

`ClickContactAgent`

`SearchFilterEngagement`

` ``` `

``### Pattern 2: Generic event name + `action` discriminator``

``One event name covers a whole family of interactions. The `action` property tells them apart.``

` ``` `

`event: "agent_profile_page"`

`action: "click_profile_past_transactions"`

`event: "agent_profile_page"`

`action: "click_operats_in_hdb_view_all"`

` ``` `

``> **Watch out for:** Pattern 2 events are harder to filter in dashboards — you always need to filter by both `event` AND `action`.``

`---`

`## The Properties Payload in Depth`

`` ### `eventData` ``

`The action-specific payload. What was done, with what parameters.`

`| Context | Example fields |`

`|---|---|`

``| Search | `bed`, `minPrice`, `maxPrice`, `district`, `resultCount`, `searchType` |``

``| Lead | `leadType`, `loc` (which CTA), `isEnquiry2` |``

``| Scroll | `listingSection` (which section entered the viewport) |``

``| View | `rankInPage`, `cardPosition` |``

`` ### `metaData` ``

`Contextual wrapper around an action. Where/how it happened, not what was done.`

` ``` `

`metaData.Origin = "Agent Directory Homepage"`

` ``` `

`### Object data sub-objects`

`Companies often attach entire objects representing the thing the user interacted with:`

` ```json `

`"listingData": {`

`"listingId": 500127183,`

`"price": 4091000,`

`"bedroom": 3,`

`"district": "D15",`

`"adProduct": "TurboPro",`

`"isNewProject": true`

`},`

`"agentData": {`

`"agentId": 545239,`

`"ratingAverage": 5,`

`"reviewCount": 14,`

`"isVerified": false`

`}`

` ``` `

`Carrying this data on every event means analysts never need to join to another table to answer "what listing was this event about?"`

`---`

`## The searchId Pattern (Session Stitching)`

`One of the most powerful patterns in e-commerce and marketplace tracking.`

``When a user submits a search, a unique `searchId` is generated. That ID is then carried on **every downstream event** — card impressions, card clicks, listing views, contact clicks, lead submissions.``

` ``` `

`Search_v2 ──► searchId: "b4d140..."`

`│`

`▼`

`View_Listing_Card ──► searchId: "b4d140..."`

`│`

`▼`

`ListingClick ──► searchId: "b4d140..."`

`│`

`▼`

`Lead_v2 ──► searchId: "b4d140..."`

` ``` `

`**This lets you answer:**`

`- Which search queries produce the most leads?`

`- Which filters correlate with higher conversion?`

`- What was the rank position of the listing that converted?`

`- Did a zero-result search kill this funnel?`

``> **Generic equivalent:** In e-commerce, this is the `sessionId` or `searchQuery`. In SaaS, it might be a `flowId` or `correlationId`. Always look for an ID that threads through a funnel.``

`---`

`## Scroll Tracking`

``PG (and many companies) fire a `Scroll` event each time a new content section enters the viewport.``

` ``` `

`Scroll → listingSection: "Overview"`

`Scroll → listingSection: "Property Details"`

`Scroll → listingSection: "Amenities"`

`Scroll → listingSection: "Home Finance"`

` ``` `

`**What this enables:**`

`- Content engagement heatmaps — which sections do users actually read?`

`- Scroll depth as a lead quality signal — deep scrollers convert more`

`- Section-level A/B testing`

`---`

`## Soft Conversions (Shortlist / Save / Favourite)`

`Not every meaningful action is a hard conversion (lead, purchase, signup). Many products track **soft conversions** — lower-friction actions that still signal strong interest.`

` ``` `

`Shortlist_v2 / Save / Favourite / Wishlist_Add`

` ``` `

``These events are often enriched with the *same* depth of object data (`listingData`, `agentData`, etc.) as a hard conversion, because the company wants to analyse "what kind of listings get shortlisted but never lead?" — a classic intent-without-conversion segment.``

`> **Generic equivalent:** "Add to wishlist" in e-commerce, "Save job" in job boards, "Star repo" in dev tools. Always check if these are tracked with full object context — if so, they're a goldmine for understanding interest that doesn't convert.`

`---`

``## CTA Location Tracking (`loc`)``

``When a page has the *same action* available in multiple places, a `loc` (location) property records exactly which instance was used.``

` ``` `

`Lead_v2 → loc: "Sticky Card" (session 1)`

`Lead_v2 → loc: "Rightside Card" (session 2)`

` ``` `

`Same event, same listing, different UI placement. This is how teams answer: *"Does the sticky bottom bar convert better than the sidebar card?"* — without needing separate event names for every button on the page.`

``> **Generic equivalent:** Look for `loc`, `placement`, `source`, `position`, or `cta_id` properties whenever a product has multiple paths to the same action (e.g. "Buy Now" appearing in 3 different places on a product page).``

`---`

`## subPageType — Finer-Grained Page Classification`

``Some events carry both `pageType` and a more granular `subPageType`:``

` ``` `

`pageType: "Listing Detail"`

`subPageType: "Listing Detail" // same here, but can diverge`

` ``` `

``Used when one broad page type contains multiple meaningfully different views or states (e.g. a "Listing Detail" page might have sub-states like a gallery modal, a map view overlay, etc.). Not always populated — but worth checking for when `pageType` alone feels too coarse to explain a metric difference.``

`---`

`## Progressive Data Enrichment`

`The same object (e.g. a listing) doesn't always carry identical fields across every event in the funnel. Some fields only get attached once the user reaches a certain depth.`

`**Example:**`

` ``` `

`View_Listing_Card_v2 (on search results) → listingData has NO greenScore`

`ListingDetailPage_v2 (on the listing page) → listingData HAS greenScore: "5|Excellent"`

` ``` `

`**Why this happens:**`

`- Some data is expensive to compute and only fetched when the detail page loads`

`- Card-level views use a lightweight summary object; detail pages use the full object`

`- The field may simply not have existed yet at the time the card-level event was instrumented`

`> **Practical implication:** Don't assume a field is "missing" or "broken" just because it's null on an early-funnel event — check whether it appears later in the funnel before flagging a bug.`

`---`

`## Timestamp Ordering vs Arrival Ordering`

``When reconstructing a user journey from captured network requests, **never trust the order events arrived in your capture tool.** Always sort by the event's own `timestamp` field.``

`**Why this matters:**`

`Client-side, events fire in the correct chronological order. But the network requests carrying them can resolve out of order — a later event's request might complete before an earlier event's request, especially under retries, batching, or variable latency.`

` ``` `

`Captured order: ListingClick → View_Listing_Card_v2`

`Actual order (by timestamp): View_Listing_Card_v2 (09:20:40.804) happened AFTER`

`ListingClick (09:20:40.698) was queued`

` ``` `

``> **Rule:** When building a funnel or journey reconstruction, always sort your event set by `timestamp`, not by row order, `messageId` sequence, or network log order.``

`---`

`## Intent vs Conversion Gap`

`A deliberate pattern to measure funnel drop-off:`

` ``` `

`ClickContactAgent (intent — they showed interest)`

`│`

`▼`

`Lead_v2 (conversion — they actually submitted)`

` ``` `

``By firing `ClickContactAgent` separately before `Lead_v2`, the product team can measure:``

`- How many users opened the contact form but didn't submit?`

`- At which step do they drop off?`

``> **Generic pattern:** Look for `Click_CTA` → `Form_Submitted` pairs. The gap between them is your funnel leak.``

`---`

`## A/B Experiments`

` ``` `

`experiment: "feature_name=Control|other_feature=Test"`

` ``` `

`- Multiple experiments can be active simultaneously, pipe-separated`

``- `Control` = original version``

``- `Test` / `Variant` = the new version being tested``

`- The experiment string travels on **every event** so analysts can segment any metric by variant`

`**What this enables:**`

`- "Did users in the Enquiry_3 Test variant submit more leads?"`

`- "Did Control or Test users scroll further down the listing page?"`

`---`

`## Destination Routing`

`### Bundled (client-side)`

`Destinations that receive the event directly in the browser via their own JavaScript SDK. Fast, but visible in the browser (Omnibug can see them).`

`Common bundled destinations:`

`- **Facebook Pixel** — ad attribution`

`- **Google Analytics / GA4** — web analytics`

`- **Bing Ads** — Microsoft ad attribution`

`- **DoubleClick Floodlight** — Google ad conversion tracking`

`### Unbundled (server-side)`

`Destinations that Segment's servers forward the event to, after the browser call. Not visible in Omnibug. Often used for sensitive tools or when data enrichment is needed first.`

``### `integrations` — per-event routing control``

`Individual events can explicitly turn destinations on or off:`

` ```json `

`"integrations": {`

`"Braze Web Mode (Actions)": false`

`}`

` ``` `

`**Why would you block a destination on one event?**`

`- The event fires too frequently (e.g. Scroll) — no point sending to an ad platform`

`- The tool handles it server-side instead (avoids double-counting)`

`- Data privacy — some user data shouldn't reach certain vendors`

`---`

`## User Context Fields`

`| Field | What it means | Why it matters |`

`|---|---|---|`

``| `memberType` | Account type (Consumer, Agent, Developer) | Filter out non-consumer behaviour from analytics |``

``| `isSymbiosis` | Internal employee / test flag | Exclude from production metrics |``

``| `intentType` | Buy / Sell / Rent / Lease | Segment funnel analysis by intent |``

``| `platform` | `web` / `ios` / `android` | Cross-platform behaviour comparison |``

``| `country` | Country code (e.g. `SG`) | Multi-market companies track per-country |``

``| `locale` | Browser locale (e.g. `en-GB`) | Internationalisation and language testing |``

``| `experiment` | Active A/B tests and variants | Segment all metrics by experiment arm |``

`---`

`## Spec vs Reality Gap`

`The **omnibus / tracking spec** is what the team *planned* to track. What fires in production is what's *actually* implemented.`

`Common gaps to watch for:`

`| Gap type | Example | Impact |`

`|---|---|---|`

``| Missing properties | `agentData` fields absent from `Lead_v2` | Can't analyse which agent quality drives leads |``

``| Undocumented events | `agent_profile_page` not in spec | Shadow tracking — can't rely on it |``

``| Typos in event names | `click_operats_in_hdb` | Breaks queries, misleads analysts |``

``| Empty `eventData` | `eventData: {}` on click events | Can't tell what was clicked |``

``| Dual events on same action | `lead_v2` + `lead` (LDM) | Risk of double-counting in reports |``

``> **How to audit:** Trigger every spec'd event manually with Omnibug open. Compare `properties` to spec. Log discrepancies.``

`---`

`## Reading an Unknown Event — Quick Checklist`

`When you see an unfamiliar event in Omnibug or a data warehouse, ask:`

``- [ ] What is `event`? What is `action`? (What happened?)``

``- [ ] What is `pageType` / `subPageType`? (Where did it happen, and how specifically?)``

``- [ ] What is `pageRefererType`? (Where did they come from?)``

``- [ ] Is there a `searchId` or similar thread ID? (Can I join this to upstream events?)``

``- [ ] What's in `eventData`? (What are the specifics?)``

``- [ ] Is there a `loc` / `placement` field? (Which UI instance triggered this, if there are multiple?)``

``- [ ] What object data is attached? (`listingData`, `agentData`, etc.) — and is it as enriched as it would be later in the funnel?``

`- [ ] Which destinations got it? (Is Braze blocked? Why?)`

``- [ ] Is this user `isInternal` / `isSymbiosis`? (Should I filter them out?)``

`- [ ] What experiment variant is active? (Is this skewed by a test?)`

``- [ ] When reconstructing a sequence of events, am I sorting by `timestamp` (correct) or by capture/arrival order (unreliable)?``

`---`

`## Tools`

`| Tool | Purpose |`

`|---|---|`

`| **Omnibug** (browser extension) | Intercepts and displays all analytics calls in real time |`

`| **Segment Debugger** | Live event stream in Segment's own UI |`

`| **Charles Proxy / mitmproxy** | Deep network inspection including HTTPS |`

``| **browser DevTools → Network** | Raw XHR/fetch calls to `api.segment.io` |``

`---`

`*Derived from live instrumentation analysis of a marketplace product. Patterns apply broadly to any Segment-instrumented web product.`