---
title: "Comparison: GA4 vs. Segment Identity & Session Tracking - Data Analytics & Activation"
source: https://propertyguru.atlassian.net/wiki/spaces/DAA/pages/35887579234/Comparison+GA4+vs.+Segment+Identity+Session+Tracking
author:
published:
created: 2026-06-01
description:
tags:
  - propertyguru
domain: Work
aliases:
  - PG
---
## Comparison: GA4 vs. Segment Identity & Session Tracking

**Segment =** Identity-first (people when stitched).Designed to understand who the user is  
**GA4 =** reporting-first (traffic by default, people if configured). GA4 is designed to understand how users interact with your site or app.

---

## 1\. Segment: How many people visited us?

Segment is user-centric and focuses on tracking an individual across touch points by stitching identity over time.  
Segment is an event collection and identity stitching layer—not a reporting tool—so how “users” are counted depends on how identity (anonymous vs logged-in) is defined downstream

### Identity Logic

- **User ID**: Assigned when a user logs in. This is the most reliable identifier for tracking a person across devices and sessions.
- **Anonymous ID**: Assigned when a user is not logged in. Stored in browser cookies/localStorage to track behavior on a single device/browser.
- **Identity Stitching (Unification)**: When an `identify` call is made, Segment links the current `anonymous_id` to a `user_id`. Over time, multiple anonymous IDs (from different devices/sessions) can be stitched to the same user, forming a unified profile.

### Key Characteristics

- **No native "Sessions"**: Segment captures event-level data and does not define sessions by default (sessions must be derived downstream if needed, we haven’t done it yet).
- **User Counting is model-dependent**:  
	Segment itself does not enforce a single “user count” definition. Metrics like Daily/Monthly Users depend on how you define them:
	- Based on `anonymous_id` → device-level users
		- Based on `user_id` → logged-in users
		- Based on stitched identity → unified users
- **Identity Persistence**:  
	The `anonymous_id` is stored in browser cookies/localStorage and persists until cleared or expired. It is domain-specific, so different websites generate different anonymous IDs. When a user logs in and an identify call is made, Segment stitches multiple anonymous IDs across devices/domains to a single User ID, enabling a unified cross-platform view.

## 2\. GA4: How much traffic did we get?

GA4 is an event-based analytics platform that is **device-centric by default**, designed to measure traffic, engagement, and sessions, while also supporting user-level tracking when configured.

### Identity Logic

- **User Pseudo ID (**`user_pseudo_id`**)**:  
	Default identifier for users
	- Web → stored in browser cookies
		- App → Firebase App Instance ID  
		Tracks users at a **device/browser level**
- **User ID (**`user_id`**)** *(optional)*:  
	Assigned when a user logs in  
	Enables **cross-device user tracking**
- **Google Signals** *(optional)*:  
	Uses Google’s data to enable **probabilistic cross-device identification**

### Unification (User Counting)

GA4 **pre-defines how users are counted** based on identity settings:

- Without `user_id` → users = devices (fragmented view)
- With `user_id` → users = unified across devices
- With Google Signals → partially stitched users

Unlike Segment, GA4 **automatically deduplicates users within its reporting layer**

### Key Characteristics

- **Session-based measurement**:  
	GA4 automatically tracks sessions and engagement metrics
- **Event-driven model**:  
	Everything (page views, clicks, etc.) is an event
- **Predefined metrics**:  
	GA4 provides built-in metrics like Users, Sessions, Engagement Rate

### Session Tracking Logic

- GA4 generates a `ga_session_id` for each session
- This ID is **not globally unique**

**In BigQuery:**

Unique session = `user_pseudo_id + ga_session_id`

**Identity Persistence**

- `user_pseudo_id` is stored in cookies (web) or app instance storage
- Persists until cookies/storage are cleared or expire
- Different browsers/devices generate different IDs

### Key Limitation

- Without `user_id`, GA4 **overcounts users across devices**
- Cross-device tracking depends on proper implementation or Google Signals

## 3\. Why the numbers will never match

It is expected that GA4 and Segment will show different totals for "Users."

1. **Identity Fragmentation (Biggest driver)**
- **GA4** (by default): counts users at a **device/browser level** using `user_pseudo_id`
- **Segment**: stitches multiple `anonymous_id` s to a single `user_id` once login happens

Result:

- Same person on laptop + mobile =
	- **GA4 → 2 users**
		- **Segment → 1 user (if stitched)**

GA4 *can* bridge users, but only if `user_id` or Google Signals is implemented properly—which is rarely perfect.

### 2) Different Units: Sessions vs Users

Your point is correct, just refine wording:

- **GA4** is **session-centric in reporting**
- **Segment** is **user/event-centric**

So:

- 1 user (Segment) can generate **multiple sessions (GA4)**
- Comparing:
	- GA4 *Sessions* ❌ vs Segment *Users is not valid*

3.**Cookie / Storage Reset Behavior**

- **GA4**:
	- New cookie = new `user_pseudo_id` = **new user**
- **Segment**:
	- Also creates a new `anonymous_id` initially
		- But can **merge identities later** via `identify(user_id)`

Both tools reset identity on cookie loss, but Segment can retroactively unify users after login, while GA4 cannot fully reconcile historical fragmentation unless user\_id is already in place.

## 3\. Practical Reporting Guidance

<table><tbody><tr><th rowspan="1" colspan="1"><div><p>If you want to know...</p><figure></figure></div></th><th rowspan="1" colspan="1"><div><p>Use this tool</p><figure></figure></div></th><th rowspan="1" colspan="1"><div><p>Why?</p><figure></figure></div></th></tr></tbody></table>

<table><tbody><tr><th rowspan="1" colspan="1"><div><p>If you want to know...</p><figure></figure></div></th><th rowspan="1" colspan="1"><div><p>Use this tool</p><figure></figure></div></th><th rowspan="1" colspan="1"><div><p>Why?</p><figure></figure></div></th></tr><tr><td rowspan="1" colspan="1"><p><strong>How many unique people used our product?</strong></p></td><td rowspan="1" colspan="1"><p><strong>Segment</strong></p></td><td rowspan="1" colspan="1"><p>It unifies logged-in and anonymous identities.</p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>What is our MAU / DAU?</strong></p></td><td rowspan="1" colspan="1"><p><strong>Segment</strong></p></td><td rowspan="1" colspan="1"><p>Best for accurate person-level counts.</p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>Which marketing channel drove the most traffic?</strong></p></td><td rowspan="1" colspan="1"><p><strong>GA4</strong></p></td><td rowspan="1" colspan="1"><p>Built specifically for acquisition and attribution.</p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>How many sessions did we have today?</strong></p></td><td rowspan="1" colspan="1"><p><strong>GA4</strong></p></td><td rowspan="1" colspan="1"><p>Native session logic is built-in.</p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>Where are users dropping off in a funnel?</strong></p></td><td rowspan="1" colspan="1"><p><strong>Segment</strong></p></td><td rowspan="1" colspan="1"><p>Better for tracking a single user's path through a flow.</p></td></tr></tbody></table>

## 4\. More Understanding into segment vs GA4

<table><tbody><tr><th rowspan="1" colspan="1"><div><p>Aspect</p><figure></figure></div></th><th rowspan="1" colspan="1"><div><p><strong>Segment</strong></p><figure></figure></div></th><th rowspan="1" colspan="1"><div><p><strong>GA4</strong></p><figure></figure></div></th></tr></tbody></table>

<table><tbody><tr><th rowspan="1" colspan="1"><div><p>Aspect</p><figure></figure></div></th><th rowspan="1" colspan="1"><div><p><strong>Segment</strong></p><figure></figure></div></th><th rowspan="1" colspan="1"><div><p><strong>GA4</strong></p><figure></figure></div></th></tr><tr><td rowspan="1" colspan="1"><p><strong>Core Philosophy</strong></p></td><td rowspan="1" colspan="1"><p>Identity-first (user-centric)</p></td><td rowspan="1" colspan="1"><p>Reporting-first (traffic & sessions)</p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>Primary Question</strong></p></td><td rowspan="1" colspan="1"><p>“Who are our users?”</p></td><td rowspan="1" colspan="1"><p>“How much traffic & engagement do we have?”</p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>Data Model</strong></p></td><td rowspan="1" colspan="1"><p>Event collection layer (raw, flexible)</p></td><td rowspan="1" colspan="1"><p>Event-based analytics with predefined metrics</p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>Identity Tracking</strong></p></td><td rowspan="1" colspan="1"><p>Strong identity stitching via <code>anonymous_id</code> + <code>user_id</code></p></td><td rowspan="1" colspan="1"><p>Default device-based (<code>user_pseudo_id</code>), optional <code>user_id</code></p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>Cross-device Tracking</strong></p></td><td rowspan="1" colspan="1"><p>✅ Strong (if <code>identify</code> is implemented)</p></td><td rowspan="1" colspan="1"><p>⚠️ Limited unless <code>user_id</code> / Google Signals used</p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>Anonymous Tracking</strong></p></td><td rowspan="1" colspan="1"><p>Yes (persistent per browser/device)</p></td><td rowspan="1" colspan="1"><p>Yes (via cookies/app instance ID)</p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>User Unification</strong></p></td><td rowspan="1" colspan="1"><p>Flexible (you define in downstream systems)</p></td><td rowspan="1" colspan="1"><p>Built-in (depends on GA4 identity settings)</p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>Sessions</strong></p></td><td rowspan="1" colspan="1"><p>❌ No native sessions</p></td><td rowspan="1" colspan="1"><p>✅ Native session tracking</p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>User Counting</strong></p></td><td rowspan="1" colspan="1"><p>Depends on your logic (anonymous vs user_id vs stitched)</p></td><td rowspan="1" colspan="1"><p>Predefined (Users, Active Users, etc.)</p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>Data Ownership</strong></p></td><td rowspan="1" colspan="1"><p>✅ Full control (warehouse-first)</p></td><td rowspan="1" colspan="1"><p>❌ Limited (Google-controlled reporting layer)</p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>Raw Data Access</strong></p></td><td rowspan="1" colspan="1"><p>✅ Full raw event stream</p></td><td rowspan="1" colspan="1"><p>⚠️ BigQuery export (sampling/limits may apply)</p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>Ease of Use</strong></p></td><td rowspan="1" colspan="1"><p>⚠️ Requires setup + downstream tools</p></td><td rowspan="1" colspan="1"><p>✅ Plug-and-play reporting</p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>Customization</strong></p></td><td rowspan="1" colspan="1"><p>✅ Highly flexible</p></td><td rowspan="1" colspan="1"><p>⚠️ Limited to GA4 framework</p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>Debugging / Tracking Control</strong></p></td><td rowspan="1" colspan="1"><p>✅ High visibility (event pipelines)</p></td><td rowspan="1" colspan="1"><p>⚠️ Less transparent</p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>Marketing Attribution</strong></p></td><td rowspan="1" colspan="1"><p>❌ Not native</p></td><td rowspan="1" colspan="1"><p>✅ Strong (channels, campaigns, ads)</p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>Real-time Reporting</strong></p></td><td rowspan="1" colspan="1"><p>⚠️ Needs setup</p></td><td rowspan="1" colspan="1"><p>✅ Built-in</p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>Tool Integrations</strong></p></td><td rowspan="1" colspan="1"><p>✅ Sends data to many tools (Amplitude, Mixpanel, etc.)</p></td><td rowspan="1" colspan="1"><p>⚠️ Mostly within Google ecosystem</p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>Use as Source of Truth</strong></p></td><td rowspan="1" colspan="1"><p>✅ Yes (recommended)</p></td><td rowspan="1" colspan="1"><p>❌ Not ideal as source of truth</p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>Implementation Complexity</strong></p></td><td rowspan="1" colspan="1"><p>High (needs planning)</p></td><td rowspan="1" colspan="1"><p>Low to medium</p></td></tr></tbody></table>

Related content