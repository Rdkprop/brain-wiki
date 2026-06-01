---
title: "Leads Playbook: Establishing a Single Source of Truth for Lead Tracking and Attribution - Data Analytics & Activation"
source: https://propertyguru.atlassian.net/wiki/spaces/DAA/pages/35901833284/Leads+Playbook+Establishing+a+Single+Source+of+Truth+for+Lead+Tracking+and+Attribution
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
## Leads Playbook: Establishing a Single Source of Truth for Lead Tracking and Attribution

This document is formatted for direct upload to **Confluence**. It preserves **all original data, links, and technical details** while organizing them into a professional heading structure with clear tables and callouts.

---

## Lead Data Single Source of Truth (SoT)

This document establishes the single source of truth (SoT) for lead data. Currently, lead data is generated and processed across multiple systems—client-side tracking (Segment), server-side pipelines (LDM), and attribution tools (GA4)—which can lead to:

- Inconsistent definitions of leads across teams
- Mismatch in reporting between tools (e.g., GA4 vs internal systems)
- Lack of clarity on which system should be trusted for what

This document resolves these gaps by defining a unified lead flow and authoritative data sources.

### Scope of Coverage:

- Client-side lead tracking (via Segment CDP)
- Server-side lead ingestion and processing
- Channel attribution flow (Segment → GA4)
- Lead enrichment (Lead Insights / Quality)
- Data availability for agents (LDM / Agent systems)
- Reporting and Analytics (Big Query as warehouse and Looker as BI tool)

---

## 1\. Lead Lifecycle: High-Level Stages

<table><tbody><tr><th rowspan="1" colspan="1"><div><p><strong>Stage</strong></p><figure></figure></div></th><th rowspan="1" colspan="1"><div><p><strong>What it Means</strong></p><figure></figure></div></th><th rowspan="1" colspan="1"><div><p><strong>What Happens (Your System)</strong></p><figure></figure></div></th><th rowspan="1" colspan="1"><div><p><strong>Primary Systems</strong></p><figure></figure></div></th></tr></tbody></table>

<table><tbody><tr><th rowspan="1" colspan="1"><div><p><strong>Stage</strong></p><figure></figure></div></th><th rowspan="1" colspan="1"><div><p><strong>What it Means</strong></p><figure></figure></div></th><th rowspan="1" colspan="1"><div><p><strong>What Happens (Your System)</strong></p><figure></figure></div></th><th rowspan="1" colspan="1"><div><p><strong>Primary Systems</strong></p><figure></figure></div></th></tr><tr><td rowspan="1" colspan="1"><p><strong>Generation</strong></p></td><td rowspan="1" colspan="1"><p>Lead is created</p></td><td rowspan="1" colspan="1"><p>User submits form / sends WhatsApp message (PGWABA/direct agent)</p></td><td rowspan="1" colspan="1"><p>Website/App, PGWABA</p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>Ingestion</strong></p></td><td rowspan="1" colspan="1"><p>Lead is captured & stored</p></td><td rowspan="1" colspan="1"><p>Lead data captured via client-side (Segment) and server-side systems</p></td><td rowspan="1" colspan="1"><p>Segment CDP, LDM (Lead Data Management)</p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>Identification</strong></p></td><td rowspan="1" colspan="1"><p>Identify the user</p></td><td rowspan="1" colspan="1"><p>Match using UMSTID, phone number, or treat as anonymous</p></td><td rowspan="1" colspan="1"><p>LDM / Identity logic</p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>Enrichment</strong></p></td><td rowspan="1" colspan="1"><p>Add insights to lead</p></td><td rowspan="1" colspan="1"><p>Add lead quality, preferences, search behavior, past activity</p></td><td rowspan="1" colspan="1"><p>Lead insights</p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>Distribution</strong></p></td><td rowspan="1" colspan="1"><p>Assign/send lead</p></td><td rowspan="1" colspan="1"><p>Lead is routed to agent and shown in LDM / Agent Net</p></td><td rowspan="1" colspan="1"><p>LDM, Agent systems</p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>Reporting</strong></p></td><td rowspan="1" colspan="1"><p>Analyze & report</p></td><td rowspan="1" colspan="1"><p>Attribution via GA4; lead counts & performance via Big Query</p></td><td rowspan="1" colspan="1"><p>GA4, Big Query, Looker</p></td></tr></tbody></table>

---

## 2\. Lead Data Layers

### 2.1 Client-Side Leads (Attribution Layer)

- Leads generated via frontend interactions are tracked using Segment CDP.
- These events are forwarded to GA4 for:
	- Channel attribution
		- Campaign performance tracking
- **Note:** These systems are not the source of truth for lead counts, but are used for marketing attribution and analytics.
- **Source of Truth:** GA4 should be treated as the source of truth for attribution, not for absolute lead numbers.

### 2.2 Server-Side Leads (Source of Truth)

- Leads are captured and processed via backend systems.
- Stored and processed in internal systems and Big Query.
- These leads act as the authoritative source of truth for:
	- Lead counts
		- Lead identity (UMSTID, email, etc.)

### 2.3 Lead Enrichment (Agent-Facing Layer)

- Server-side leads are enriched with:
	- Lead Quality
		- Preferences
		- Insights (search behavior, history, etc.)
- This enrichment is powered via Lead Insights info (Activation team owns this).
- Final enriched leads are exposed to agents via LDM / Agentnet.
- This ensures agents receive actionable, context-rich leads, not just raw contact data.

---

## 3\. Understanding LDM & Communication Models

### 3.1 Lead Gating Definitions

- **Lead gating** = forcing user to share details (like phone/email/login) before connecting to agent.
- **In E2.0** → user must pass a gate (login / phone capture / WABA flow).
- **In E3.0** → no gate → user can directly contact agent (E3.0 is E2.0 without lead gating for WhatsApp enquiry channel).

### 3.2 Communication Models (E1.0 vs E2.0 vs E3.0)

*In E1.0 & E2.0, login of user is mandatory to generate a lead.*

#### Model A: WhatsApp Business Account (WABA – PG Owned)

- User messages go through PG’s WhatsApp Business Account.
- PG acts as an intermediary layer between user and agent.
- **What we get:** User mobile number (mandatory), message-level data (sent, delivered, replied), timestamps, engagement signals, and visibility into conversation flow.
- **Outcome:** Strong tracking + high data reliability.
	- *Example:* SG utilizes E2.0 for leads generation. For E2.0 consumer mobile is mandatory. So, it has high no. of leads with consumer mobile.

#### Model B: Direct Agent Flow (E1.0)

- User directly contacts agent’s mobile (via call/WhatsApp deep link).
- No backend involvement after click.
- **What we get:** Only **click/intent-level tracking**.
- **What we DON’T get:** Whether user sent message, whether agent responded, conversation details, or user phone number (if not captured earlier).
- **Outcome:** Limited tracking + high data loss. communication happens outside pg. systems and WhatsApp is peer to peer. We have no visibility into agent inbox.
- *Note:* In case of failure for E2.0 and E3.0, failures are visible and controllable. For E1.0, tracking loss is inherent even if systems work.

---

## 4\. Lead Systems Comparison Matrix

<table><tbody><tr><th rowspan="1" colspan="1"><div><p><strong>Aspect</strong></p><figure></figure></div></th><th rowspan="1" colspan="1"><div><p><strong>E1.0 (Direct Agent Flow)</strong></p><figure></figure></div></th><th rowspan="1" colspan="1"><div><p><strong>E2.0 (Lead Gating + Controlled Flow)</strong></p><figure></figure></div></th><th rowspan="1" colspan="1"><div><p><strong>E3.0 (No Gating – Hybrid)</strong></p><figure></figure></div></th></tr></tbody></table>

<table><tbody><tr><th rowspan="1" colspan="1"><div><p><strong>Aspect</strong></p><figure></figure></div></th><th rowspan="1" colspan="1"><div><p><strong>E1.0 (Direct Agent Flow)</strong></p><figure></figure></div></th><th rowspan="1" colspan="1"><div><p><strong>E2.0 (Lead Gating + Controlled Flow)</strong></p><figure></figure></div></th><th rowspan="1" colspan="1"><div><p><strong>E3.0 (No Gating – Hybrid)</strong></p><figure></figure></div></th></tr><tr><td rowspan="1" colspan="1"><p><strong>Basic Flow</strong></p></td><td rowspan="1" colspan="1"><p>User → Click → Direct call/WhatsApp to agent</p></td><td rowspan="1" colspan="1"><p>User → Pass gate (login/phone/WABA) → Lead created → Connect to agent</p></td><td rowspan="1" colspan="1"><p>User → Click → Can directly contact OR go via WABA (no forced gate)</p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>Lead Creation</strong></p></td><td rowspan="1" colspan="1"><p>❌ Not guaranteed (depends on user action)</p></td><td rowspan="1" colspan="1"><p>✅ Always created before connecting</p></td><td rowspan="1" colspan="1"><p>⚠️ Partial (only if captured via WABA/backend)</p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>User Identification</strong></p></td><td rowspan="1" colspan="1"><p>❌ Mostly anonymous</p></td><td rowspan="1" colspan="1"><p>✅ Strong (UMSID, phone, email)</p></td><td rowspan="1" colspan="1"><p>⚠️ Weak (mix of known + anonymous)</p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>Phone Number Capture</strong></p></td><td rowspan="1" colspan="1"><p>❌ No</p></td><td rowspan="1" colspan="1"><p>✅ Mandatory</p></td><td rowspan="1" colspan="1"><p>⚠️ Optional / inconsistent</p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>Data Flow Control</strong></p></td><td rowspan="1" colspan="1"><p>❌ No control (external to system)</p></td><td rowspan="1" colspan="1"><p>✅ Fully controlled (server-side)</p></td><td rowspan="1" colspan="1"><p>⚠️ Partial control</p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>Tracking Level</strong></p></td><td rowspan="1" colspan="1"><p>Intent only (clicks)</p></td><td rowspan="1" colspan="1"><p>Full funnel (lead → contact → engagement)</p></td><td rowspan="1" colspan="1"><p>Mixed (intent + some confirmed leads)</p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>Visibility of Conversation</strong></p></td><td rowspan="1" colspan="1"><p>❌ None</p></td><td rowspan="1" colspan="1"><p>✅ Full (via WABA)</p></td><td rowspan="1" colspan="1"><p>⚠️ Only for WABA flows</p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>Matching</strong></p></td><td rowspan="1" colspan="1"><p>❌ Not applicable / very weak</p></td><td rowspan="1" colspan="1"><p>✅ Strong</p></td><td rowspan="1" colspan="1"><p>⚠️ Weak (mostly falls to Step 3–4)</p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>Lead Types Distribution</strong></p></td><td rowspan="1" colspan="1"><p>Mostly unknown / not structured</p></td><td rowspan="1" colspan="1"><p>More Type 1,2,3 (identified leads)</p></td><td rowspan="1" colspan="1"><p>More Type 4,5 (anonymous leads)</p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>Deduplication</strong></p></td><td rowspan="1" colspan="1"><p>❌ Poor</p></td><td rowspan="1" colspan="1"><p>✅ Strong (UMSID/phone based)</p></td><td rowspan="1" colspan="1"><p>⚠️ Weak (anonymous heavy)</p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>Attribution Accuracy</strong></p></td><td rowspan="1" colspan="1"><p>❌ Very low</p></td><td rowspan="1" colspan="1"><p>✅ High</p></td><td rowspan="1" colspan="1"><p>⚠️ Medium to low</p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>Dependency</strong></p></td><td rowspan="1" colspan="1"><p>User + agent behavior</p></td><td rowspan="1" colspan="1"><p>Backend systems</p></td><td rowspan="1" colspan="1"><p>Hybrid (user + partial backend)</p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>Failure Visibility</strong></p></td><td rowspan="1" colspan="1"><p>❌ Invisible</p></td><td rowspan="1" colspan="1"><p>✅ Trackable</p></td><td rowspan="1" colspan="1"><p>⚠️ Partial</p></td></tr></tbody></table>

---

## 5\. Market Overview & Coverage

### 5.1 Consumer Leads by Market

<table><tbody><tr><th rowspan="1" colspan="1"><div><p><strong>Metric</strong></p><figure></figure></div></th><th rowspan="1" colspan="1"><div><p><strong>SG</strong></p><figure></figure></div></th><th rowspan="1" colspan="1"><div><p><strong>MY (PGMY + IPPMY)</strong></p><figure></figure></div></th><th rowspan="1" colspan="1"><div><p><strong>TH</strong></p><figure></figure></div></th></tr></tbody></table>

<table><tbody><tr><th rowspan="1" colspan="1"><div><p><strong>Metric</strong></p><figure></figure></div></th><th rowspan="1" colspan="1"><div><p><strong>SG</strong></p><figure></figure></div></th><th rowspan="1" colspan="1"><div><p><strong>MY (PGMY + IPPMY)</strong></p><figure></figure></div></th><th rowspan="1" colspan="1"><div><p><strong>TH</strong></p><figure></figure></div></th></tr><tr><td rowspan="1" colspan="1"><p><strong>Lead Event Coverage</strong></p></td><td rowspan="1" colspan="1"><p>lead_v2 (Segment ASOT)</p></td><td rowspan="1" colspan="1"><p>lead_v2 on SRP, LDP, PLDP</p></td><td rowspan="1" colspan="1"><p>lead_v2</p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>Pages Covered</strong></p></td><td rowspan="1" colspan="1"><p>SRP, LDP, Agent Profile, PLDP, homeowner dashboard landing</p></td><td rowspan="1" colspan="1"><p>SRP, LDP, PLDP</p></td><td rowspan="1" colspan="1"><p>SRP, LDP</p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>Missing Coverage</strong></p></td><td rowspan="1" colspan="1"><p>My activities (lead event is triggered)</p></td><td rowspan="1" colspan="1"><p>Agent Profile Page, My activities (only lead, not lead_v2)</p></td><td rowspan="1" colspan="1"><p>Agent Profile, Find Agent, My Activities (no lead event triggered)</p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>Source of Truth (Product/Agents/Sales)</strong></p></td><td rowspan="1" colspan="1"><p>LDM (Web + App)</p></td><td rowspan="1" colspan="1"><p>LDM (Web + App)</p></td><td rowspan="1" colspan="1"><p>LDM (web+app) (Data availability from mid-Jan 2026)</p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>Marketing Attribution (Web)</strong></p></td><td rowspan="1" colspan="1"><p>GA4 (via Segment forwarding)</p></td><td rowspan="1" colspan="1"><p>GA4 (via Segment)</p></td><td rowspan="1" colspan="1"><p>GA4 (via Segment)</p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>Marketing Attribution (App)</strong></p></td><td rowspan="1" colspan="1"><p>AppsFlyer</p></td><td rowspan="1" colspan="1"><p>AppsFlyer (Only for PGMY). No deeplinks architecture for app on IPPMY</p></td><td rowspan="1" colspan="1"><p>AppsFlyer</p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>Lead System</strong></p></td><td rowspan="1" colspan="1"><p>E2.0 → E3.0 (Feb 2026)</p></td><td rowspan="1" colspan="1"><p>E1.0 → E3.0 (Apr 2026)</p></td><td rowspan="1" colspan="1"><p>E1.0 → evolving</p></td></tr></tbody></table>

### 5.2 Key Gaps across Markets

<table><tbody><tr><th rowspan="1" colspan="1"><div><p><strong>Area</strong></p><figure></figure></div></th><th rowspan="1" colspan="1"><div><p><strong>Gap</strong></p><figure></figure></div></th></tr></tbody></table>

<table><tbody><tr><th rowspan="1" colspan="1"><div><p><strong>Area</strong></p><figure></figure></div></th><th rowspan="1" colspan="1"><div><p><strong>Gap</strong></p><figure></figure></div></th></tr><tr><td rowspan="1" colspan="1"><p>Agent Profile Pages</p></td><td rowspan="1" colspan="1"><p>Missing lead_v2 (MY, TH)</p></td></tr><tr><td rowspan="1" colspan="1"><p>Find Agent Page</p></td><td rowspan="1" colspan="1"><p>Missing in TH (LDM + events)</p></td></tr><tr><td rowspan="1" colspan="1"><p>App Attribution</p></td><td rowspan="1" colspan="1"><p>Cannot send Segment → GA4</p></td></tr></tbody></table>

### 5.3 Developer Leads

<table><tbody><tr><th rowspan="1" colspan="1"><div><p><strong>Market</strong></p><figure></figure></div></th><th rowspan="1" colspan="1"><div><p><strong>Tracking Method</strong></p><figure></figure></div></th><th rowspan="1" colspan="1"><div><p><strong>Source of Truth</strong></p><figure></figure></div></th></tr></tbody></table>

<table><tbody><tr><th rowspan="1" colspan="1"><div><p><strong>Market</strong></p><figure></figure></div></th><th rowspan="1" colspan="1"><div><p><strong>Tracking Method</strong></p><figure></figure></div></th><th rowspan="1" colspan="1"><div><p><strong>Source of Truth</strong></p><figure></figure></div></th></tr><tr><td rowspan="1" colspan="1"><p>SG</p></td><td rowspan="1" colspan="1"><p>Backend API (not LDM)</p></td><td rowspan="1" colspan="1"><p>Backend systems</p></td></tr><tr><td rowspan="1" colspan="1"><p>MY</p></td><td rowspan="1" colspan="1"><p>Backend API (not LDM)</p></td><td rowspan="1" colspan="1"><p>Backend systems</p></td></tr><tr><td rowspan="1" colspan="1"><p>TH</p></td><td rowspan="1" colspan="1"><p>Backend API (not LDM)</p></td><td rowspan="1" colspan="1"><p>Backend systems</p></td></tr><tr><td rowspan="1" colspan="1"><p>IPP</p></td><td rowspan="1" colspan="1"><p>❓ WIP / No clarity</p></td><td rowspan="1" colspan="1"><p>TBD</p></td></tr></tbody></table>

---

## 6\. Technical Data Sources (BigQuery & Looker)

### 6.1 LDM Sources (Primary SoT)

<table><tbody><tr><th rowspan="1" colspan="1"><div><p>Marketplace</p><figure></figure></div></th><th rowspan="1" colspan="1"><div><p>BQ table (web and app)</p><figure></figure></div></th><th rowspan="1" colspan="1"><div><p>LDM explore (web and app)</p><figure></figure></div></th></tr></tbody></table>

<table><tbody><tr><th rowspan="1" colspan="1"><div><p>Marketplace</p><figure></figure></div></th><th rowspan="1" colspan="1"><div><p>BQ table (web and app)</p><figure></figure></div></th><th rowspan="1" colspan="1"><div><p>LDM explore (web and app)</p><figure></figure></div></th></tr><tr><td rowspan="1" colspan="1"><p><strong>PGSG</strong></p></td><td rowspan="1" colspan="1"><p><code>propertyguru-datalake-v0.sg_lead_server.lead</code></p></td><td rowspan="1" colspan="1"><p><a href="https://propertyguru.cloud.looker.com/explore/agent/lead_extend?qid=tkqGM1N20IZlFWivLaZFlC">Looker Explore</a></p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>PGMY</strong></p></td><td rowspan="1" colspan="1"><p><code>propertyguru-datalake-v0.my_lead_server.lead</code></p></td><td rowspan="1" colspan="1"><p><a href="https://propertyguru.cloud.looker.com/explore/agent/lead_extend?qid=tkqGM1N20IZlFWivLaZFlC">Looker Explore</a></p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>IPPMY</strong></p></td><td rowspan="1" colspan="1"><p><code>propertyguru-datalake-v0.th_lead_server.lead</code></p></td><td rowspan="1" colspan="1"><p><a href="https://propertyguru.cloud.looker.com/explore/agent/lead_extend?qid=tkqGM1N20IZlFWivLaZFlC">Looker Explore</a></p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>PGTH</strong></p></td><td rowspan="1" colspan="1"><p><code>propertyguru-datalake-v0.ipp_lead_server_v2.lead</code></p></td><td rowspan="1" colspan="1"><p><a href="https://propertyguru.cloud.looker.com/explore/agent/lead_extend?qid=tkqGM1N20IZlFWivLaZFlC">Looker Explore</a></p></td></tr></tbody></table>

### 6.2 Segment Sources

<table><tbody><tr><th rowspan="1" colspan="1"><div><p>Marketplace</p><figure></figure></div></th><th rowspan="1" colspan="1"><div><p>Web BQ / Looker</p><figure></figure></div></th><th rowspan="1" colspan="1"><div><p>App BQ / Looker</p><figure></figure></div></th></tr></tbody></table>

<table><tbody><tr><th rowspan="1" colspan="1"><div><p>Marketplace</p><figure></figure></div></th><th rowspan="1" colspan="1"><div><p>Web BQ / Looker</p><figure></figure></div></th><th rowspan="1" colspan="1"><div><p>App BQ / Looker</p><figure></figure></div></th></tr><tr><td rowspan="1" colspan="1"><p><strong>PGSG</strong></p></td><td rowspan="1" colspan="1"><p><code>sg_segment_consumer.lead_v2</code> / <a href="https://propertyguru.cloud.looker.com/explore/consumer_master_explore/segment_lead">Explore</a></p></td><td rowspan="1" colspan="1"><p><code>sg_segment_consumer_android/ios_v2.lead_v2</code> / <a href="https://propertyguru.cloud.looker.com/explore/consumer_master_explore/app_lead">Explore</a></p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>PGMY</strong></p></td><td rowspan="1" colspan="1"><p><code>my_segment_consumer.lead_v2</code> / <a href="https://propertyguru.cloud.looker.com/explore/consumer_master_explore/segment_lead">Explore</a></p></td><td rowspan="1" colspan="1"><p><code>my_segment_consumer_android/ios_v2.lead_v2</code> / <a href="https://propertyguru.cloud.looker.com/explore/consumer_master_explore/app_lead">Explore</a></p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>IPPMY</strong></p></td><td rowspan="1" colspan="1"><p><code>ipp_segment_consumer_web.lead_v2</code> / <a href="https://propertyguru.cloud.looker.com/explore/consumer_master_explore/segment_lead">Explore</a></p></td><td rowspan="1" colspan="1"><p><code>ipp_consumer_ios/android.vw_lead</code> / <a href="https://propertyguru.cloud.looker.com/explore/consumer_master_explore/app_lead">Explore</a></p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>PGTH</strong></p></td><td rowspan="1" colspan="1"><p><code>th_segment_consumer.lead_v2</code> / <a href="https://propertyguru.cloud.looker.com/explore/consumer_master_explore/segment_lead">Explore</a></p></td><td rowspan="1" colspan="1"><p><code>th_segment_consumer_android/ios_v2.lead_v2</code> / <a href="https://propertyguru.cloud.looker.com/explore/consumer_master_explore/app_lead">Explore</a></p></td></tr></tbody></table>

### 6.3 GA4 Sources

<table><tbody><tr><th rowspan="1" colspan="1"><div><p>Marketplace</p><figure></figure></div></th><th rowspan="1" colspan="1"><div><p>Web BQ / Looker</p><figure></figure></div></th><th rowspan="1" colspan="1"><div><p>App BQ / Looker</p><figure></figure></div></th></tr></tbody></table>

<table><tbody><tr><th rowspan="1" colspan="1"><div><p>Marketplace</p><figure></figure></div></th><th rowspan="1" colspan="1"><div><p>Web BQ / Looker</p><figure></figure></div></th><th rowspan="1" colspan="1"><div><p>App BQ / Looker</p><figure></figure></div></th></tr><tr><td rowspan="1" colspan="1"><p><strong>PGSG</strong></p></td><td rowspan="1" colspan="1"><p><code>datamart_v0.sg_fact_leads_views</code> / <a href="https://propertyguru.cloud.looker.com/projects/pg/files/views/lead_and_view/fact_lead.view.lkml?line=7&tab=file">View</a></p></td><td rowspan="1" colspan="1"><p><code>datamart_v0.sg_fact_leads_views_mobile</code> / <a href="https://propertyguru.cloud.looker.com/explore/lead/fact_lead_app?qid=GswrrqWNSkNTbznLciiczL&origin_space=207&toggle=vis">Explore</a></p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>PGMY</strong></p></td><td rowspan="1" colspan="1"><p><code>datamart_v0.my_fact_leads_views</code> / <a href="https://propertyguru.cloud.looker.com/projects/pg/files/views/lead_and_view/fact_lead.view.lkml?line=7&tab=file">View</a></p></td><td rowspan="1" colspan="1"><p><code>datamart_v0.my_fact_leads_views_mobile</code> / <a href="https://propertyguru.cloud.looker.com/explore/lead/fact_lead_app?qid=GswrrqWNSkNTbznLciiczL&origin_space=207&toggle=vis">Explore</a></p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>IPPMY</strong></p></td><td rowspan="1" colspan="1"><p><code>data-services-asia-staging.raw_ga.lead_web</code> / <a href="https://propertyguru.cloud.looker.com/explore/ipp_consumer/ipp_lead_web?qid=q4fUrNQcRsUQXQJbSas8rE&origin_space=207&toggle=vis">Explore</a></p></td><td rowspan="1" colspan="1"><p><code>data-services-asia-staging.raw_ga.lead_app</code> / <a href="https://propertyguru.cloud.looker.com/explore/ipp_consumer/ipp_lead_app?qid=xUKI3mFjDjQR0xt2untt2O&origin_space=1405&toggle=vis">Explore</a></p></td></tr><tr><td rowspan="1" colspan="1"><p><strong>PGTH</strong></p></td><td rowspan="1" colspan="1"><p><code>datamart_v0.th_fact_leads_views</code> / <a href="https://propertyguru.cloud.looker.com/projects/pg/files/views/lead_and_view/fact_lead.view.lkml?line=7&tab=file">View</a></p></td><td rowspan="1" colspan="1"><p><code>datamart_v0.th_fact_leads_views_mobile</code> / <a href="https://propertyguru.cloud.looker.com/explore/lead/fact_lead_app?qid=GswrrqWNSkNTbznLciiczL&origin_space=207&toggle=vis">Explore</a></p></td></tr></tbody></table>

---

## 7\. Usage Guidelines & Critical Rules

### 7.1 LDM (Lead Data Management) – Primary SoT

**Should be used by:** Product teams, Agent teams, Sales / Business teams.  
**Why LDM?**

- Backend-driven (server-side) → not impacted by ad blockers.
- Consistent coverage across all pages.
- Represents actual lead creation (post-validation).
- Most reliable for: Reporting, Performance tracking, Business metrics.
- **Conclusion: LDM = Single Source of Truth for leads across Web + App.**

### 7.2 Segment (Event Tracking Layer)

**When to use Segment (lead\_v2)?**

- To analyze: Lead drops / funnel issues, Top-of-funnel behavior, Event-level debugging.  
	**Key Characteristics:** Client-side + event-driven. May be impacted by ad blockers, network issues, or event firing gaps.
- **Conclusion: Use Segment for debugging and funnel analysis — NOT for final lead counts.**

### 7.3 GA4 (Marketing Attribution – Web)

**When to use GA4?** Channel attribution, Campaign performance, Marketing reporting.  
**Context:** Leads are forwarded from Segment and stitched using GA session ID and channel grouping logic.  
**How to use with Segment:** If there is a trend issue in GA4, check Segment events to validate flow and drop points.

### 7.4 AppsFlyer (Marketing Attribution – App)

- Source of truth for app attribution (since Segment → GA4 forwarding is not available for app).
- Used by marketing for campaign tracking and attribution on app.

### 7.5 IMPORTANT GUIDELINES (CRITICAL)

- **Do NOT compare absolute numbers between: LDM vs GA4 OR LDM vs Segment.** These systems serve different purposes and will never match exactly.
- **What SHOULD be done:** Compare trends, not absolute values. Ensure directional alignment (up/down trends) and no major unexplained divergence.
- **For RCA (Root Cause Analysis):**
	- Use **GA4** for identifying issue (channel level).
		- Use **Segment** for debugging event flow.
		- Use **LDM** for actual business impact.

---

## 8\. Future State (Work in Progress)

- Channel attribution in LDM (for web) is currently under development. Once completed and validated, LDM will also support marketing attribution use cases. At that point, Marketing teams can also rely on LDM as source of truth.
- For further understanding of LDM tables data points, refer to: [DAA Wiki Page](https://propertyguru.atlassian.net/wiki/spaces/DAA/pages/35822960798 "https://propertyguru.atlassian.net/wiki/spaces/DAA/pages/35822960798")