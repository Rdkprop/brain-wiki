---
title: Analytics Agent
source: https://allpropertymedia-my.sharepoint.com/:w:/r/personal/rohan_propertyguru_tech/_layouts/15/Doc.aspx?sourcedoc=%7BE3EAE78B-1FA6-4457-87E2-443D4F934FB5%7D&file=We%20now%20have%20a%20well-defined%20Consumer%20Analytics%20skill%20that%20can.docx&action=default&mobileredirect=true&DefaultItemOpen=1&wdOrigin=APPHOME-WEB.DIRECT%2CAPPHOME-WEB.FILEBROWSER.RECENT&wdPreviousSession=72be7af2-bfba-4f8b-81dc-8557e89b87af&wdPreviousSessionSrc=AppHomeWeb&ct=1780296330292
author: Rohan
published:
created: 2026-06-01
description:
tags:
  - propertyguru
aliases:
  - PG
domain: Work
---
Analytics Agent Prompt Set 

Overview 

We now have a well-defined Analytics skill that can answer a broad set of stakeholder questions across: 

- User behavior 
    

- Listing engagement 
    

- Lead generation 
    

- Search behavior 
    

- Marketing performance 
    

Supported markets: Singapore (SG), Malaysia (MY), Thailand (TH) (with some explore-specific exceptions). 

What this capability is for 

Consumer-side analytics only, including: 

- Visitor behavior and audience growth 
    

- Listing discovery and engagement 
    

- Lead funnels 
    

- Search behavior 
    

- App/Web actions 
    

- Selected finance/campaign flows (where supported by the governed datasets) 
    

Strongest for: 

- Trend analysis 
    

- Breakdowns by market / platform / intent 
    

- Listing-level performance when pre-aggregated explores exist 
    

Additional coverage: 

- Works across SG, MY, TH depending on the dataset 
    

- Also supports IPP MY and PGCG in specific lead contexts 
    

1) Core question categories it can answer 

2. Users and audience growth 

- How many unique users did we have in SG in Q1? 
    

- What is the DAU/WAU/MAU trend by market? 
    

- How did app vs web users change month over month? 
    

- How many logged-in vs anonymous users do we have? 
    

- Which campaigns/channels drove user growth? 
    

2. Listing Detail Page Views (LDPV) 

- How many listing detail views did we get by day/week/month? 
    

- Which property types or intents got the most LDPV? 
    

- What is LDPV split by platform (web, mobile web, iOS, Android, app)? 
    

- Which referrer types drive LDPV? 
    

- Which listings/projects are top by LDPV in a given period? 
    

3. Impressions and visibility (Real Impressions) 

- How many real impressions did listings receive? 
    

- What is impression split by page type (SRP, LDP widget, homepage)? 
    

- How do impressions trend by market, intent, platform? 
    

- Which listings got the most impressions? 
    

- What is the impression-to-view relationship by category? 
    

4. Leads and enquiries 

- What are total leads and unique leads by market and period? 
    

- How are leads distributed by channel (WhatsApp, call, email, phone reveal)? 
    

- How are leads split by lead source page and platform? 
    

- Which listings or agents generated the most leads? 
    

- How do lead trends differ across SG, MY, TH and IPP? 
    

- For web experiments: which variant drove higher lead volume and conversion? 
    

5. Search behavior and demand signals 

- How many distinct searches happened by market and platform? 
    

- Which locations, property types, and sub-types are most searched? 
    

- What are user intent splits (sale vs rent)? 
    

- Which trigger pages initiate the most searches? 
    

- What share of searches return zero results? 
    

- How did search demand shift by district/state/city over time? 
    

6. Engagement and action events 

- Listing clicks / click-through patterns / contact-agent clicks 
    

- Scroll depth and gallery engagement 
    

- Shortlist/save search/share/report/hide/restore behavior 
    

- Login behavior and app screen/page interactions 
    

- Recommendation exposure events 
    

7. Finance and mortgage journey (where supported) 

- Mortgage navigation and mortgage calculator usage 
    

- Finance detail entry behavior 
    

- Price insights and price widget interactions 
    

8. Marketing and lifecycle 

- App installations 
    

- Braze lifecycle step performance 
    

- Turbo Pro attribution and WhatsApp first-touch views 
    

9. Listing aggregate performance (strong use case) 

- Listing-level or agent listing performance 
    

- Funnel-style listing metrics: impressions → views → leads → enquirers 
    

- Platform splits and channel-level lead composition 
    

- Ad product-level outcomes 
    

- SG and MY listing aggregate analysis (including IPP via entity filtering) 
    

10. SG market transaction questions (specialized) 

- Completed transaction trends (resale, sub-sale, rentals, etc.) 
    

- Transaction volume by property type, district, town 
    

- Agent representation and participation counts 
    

- Onboarded vs non-onboarded agent lens for SG transaction data 
    

2) Example business questions by function 

Product 

- Is search quality improving (zero-result rate, avg result count)? 
    

- Are recommendation surfaces driving incremental listing views? 
    

- Which app screens correlate with higher listing engagement? 
    

Consumer Marketing 

- Which channels/campaigns drove user growth last month? 
    

- Did app installs convert into meaningful listing engagement? 
    

- Did campaign cohorts produce higher-quality lead actions? 
    

Business / Commercial 

- Which property segments/intents are growing fastest? 
    

- Which projects/listings are top by impressions, views, and leads? 
    

- How does SG compare with MY/TH for funnel efficiency? 
    

Sales / Agent-facing teams 

- Which listing portfolios generate the most unique leads? 
    

- Which lead channels dominate by market or project type? 
    

- Which geographies have strong visibility but weak lead conversion? 
    

Leadership 

- Is top-of-funnel demand rising? 
    

- Where are we seeing demand-to-conversion gaps? 
    

- Which market/platform needs intervention next quarter? 
    

3) What this skill does especially well 

- Fast pre-aggregated trends for LDPV, impressions, leads, and search 
    

- Cross-market comparisons with correct market/entity handling 
    

- Detailed listing-level and agent-level lead analysis via enriched joins 
    

- Practical routing between: 
    

- aggregate for speed 
    

- event-level for detail 
    

- Explains results in business language, not internal technical fields 
    

4) Important boundaries & non-goals 

Use this skill for: 

- Consumer behavior and marketplace demand/conversion 
    

- Listing visibility and engagement 
    

- Lead/enquiry analytics 
    

- Search and on-site behavior 
    

- Selected marketing/finance event analytics 
    

Do not use this skill for: 

- Agent subscriptions / inventory management 
    

- Credits billing / or revenue accounting 
    

- Non-consumer operational reporting that belongs to other domain models 
    

5) Key data caveats everyone should know 

- Different explores apply different market/date filter logic; incorrect filtering can produce wrong answers. 
    

- Some app explores default to MY unless market is explicitly set. 
    

- MAU explores are pre-aggregated and may not use the normal date-selector pattern. 
    

- Search aggregate may include all platforms; combining with web/app search explores can double count. 
    

- Listing aggregate schema differs between SG and MY (field names/available metrics are not identical). 
    

- Inactive listings may show zero in narrow windows even if history exists (two-step “active window discovery” may be required). 
    

- Certain lead questions require careful explore routing: 
    

- All-market trend/channel → lead aggregate explore 
    

- Listing/agent detail → lead management explore 
    

- Experiment split → web lead explore 
    

6) How to ask questions for best results 

Include these five items in your request: 

1. Market: SG, MY, TH, all, or specific brand context (e.g., IPP MY) 
    

2. Date range: explicit period 
    

3. Granularity: day, week, month 
    

4. Platform: web, app, or both 
    

5. Audience scope: all users or logged-in only 
    

Examples of high-quality asks 

- SG, Jan–Mar 2026, monthly: top 10 residential projects by LDPV (sale and rent) 
    

- MY, last 90 days, weekly: lead channel trend + unique lead split by platform 
    

- TH, Q1 2026, monthly: search demand by province and user intent 
    

7) Suggested rollout message to teams 

- Use the Consumer Analytics skill for answerable, auditable consumer behavior insights. 
    

- Ask with market + date + granularity + platform + user scope. 
    

- For listing-level performance, explicitly say listing-level (to route to aggregate listing explores). 
    

- For experiments, explicitly say experiment analysis (to route to the correct lead explore). 
    

Optional Appendix (future) 

If helpful, we can add either: 

1. A shorter “one-screen” executive version, or  
    

2. A “question bank” appendix with 50+ ready-to-use prompts by team (Product, Marketing, Commercial, Leadership). 
    

------- 

Appendix: Consumer Analytics Skill — Question Bank (50+ Prompts) 

How to use: Replace the bracketed fields with your target values (e.g., SG, last 30 days, monthly, sale/rent, web/app, etc.). 

A) Users & Audience Growth (10+) 

1. How many unique users did we have in [SG] in [Q1 2026]? 
    

2. What is the DAU/WAU/MAU trend by market from [start] to [end]? 
    

3. How did app vs web users change month-over-month for [MY]? 
    

4. What is the split of logged-in vs anonymous users in [SG] for [last 90 days]? 
    

5. Which platform drove the most user growth in [TH] during [period]? 
    

6. Which campaigns/channels drove user growth in [SG] last month? 
    

7. What are user trends by intent (sale vs rent) in [SG]? 
    

8. How many new users vs returning users did we have in [MY]? 
    

9. What is the top entry funnel page for new users in [SG] (if supported)? 
    

10. Compare user growth between [SG vs MY] for [same date range]. 
    

B) LDPV (Listing Detail Page Views) (10+) 

11. How many LDP views did we get in [SG] by day/week/month in [period]? 
    

12. Which property types generated the most LDPV in [MY]? 
    

13. Which intents (sale vs rent) drove the most LDPV in [TH]? 
    

14. What is the LDPV split by platform (web/mobile web/iOS/Android/app) in [SG]? 
    

15. Which referrer types drive the most LDPV in [SG]? 
    

16. Top 10 listings/projects by LDPV in [last 30 days] for [SG]. 
    

17. What is the LDPV trend by market and intent for [Q1]? 
    

18. Show LDPV by page section or widget context (if supported). 
    

19. For [Sale] listings only, which platform has the highest LDPV in [MY]? 
    

20. LDPV comparison: web vs app performance in [SG] during [period]. 
    

C) Real Impressions & Visibility (8+) 

21. How many real impressions did listings receive in [SG] by month? 
    

22. What is the impression split by page type (SRP/LDP widget/homepage) in [MY]? 
    

23. How do real impressions trend by market, intent, platform for [period]? 
    

24. Which listings/projects generated the most real impressions in [last 90 days]? 
    

25. What is the impression-to-view ratio by property category in [SG]? 
    

26. Which intent has the highest impressions but lowest view conversion in [TH]? 
    

27. Compare impression trends between [SG vs TH] for [same period]. 
    

28. What are the top districts/areas driving impressions in [SG]? (where district is available) 
    

D) Leads & Enquiries (10+) 

29. Total leads and unique leads in [SG] for [period]. 
    

30. Leads by channel (WhatsApp/call/email/phone reveal) in [MY] during [period]. 
    

31. Leads split by lead source page and platform in [SG]. 
    

32. Which listings/projects generated the most leads in [last 30 days] for [TH]? 
    

33. Which agents generated the most leads in [SG] during [period]? 
    

34. Lead trend: WoW comparison for [MY]. 
    

35. Lead trend: MoM comparison for [SG] (and show breakdown by intent if supported). 
    

36. Leads comparison across SG vs MY vs TH for [period]. 
    

37. For [web] experiments, which variant drove higher lead volume in [market]? 
    

38. Which listings have high LDPV but low lead conversion in [SG]? (relationship analysis) 
    

E) Search Behavior & Demand Signals (8+) 

39. How many distinct searches happened in [SG] during [period]? 
    

40. Searches by location/district in [SG] over [time range]. 
    

41. Searches by property type in [MY] for [month]. 
    

42. Intent split of searches (sale vs rent) in [TH]. 
    

43. Which trigger pages initiate the most searches in [SG]? 
    

44. What share of searches return zero results in [MY]? 
    

45. Search demand trend by district in [SG] from [start] to [end]. 
    

46. Search demand comparison: web vs app in [SG] for [period]. 
    

F) Engagement & Action Events (6+) 

47. What are listing click volumes and click-through patterns in [SG] during [period]? 
    

48. Shortlist/save/share behavior trend in [MY] by week. 
    

49. What are login rates and login funnel drops in [TH]? (if supported in the explore) 
    

50. Scroll depth / gallery engagement trend in [SG] during [period]. 
    

51. Recommendation exposure vs listing engagement correlation in [MY]. 
    

52. App screen/page interactions: where do users spend the most time before leads? (if supported) 
    

G) Listing Aggregate Performance (10+) 

53. Listing funnel performance (impressions → views → leads → enquirers) for [SG] in [period]. 
    

54. Top listings by views + leads in [MY] (show channel composition if supported). 
    

55. Platform split: funnel efficiency in [SG] (web vs app). 
    

56. How do ad products impact listing outcomes in [SG] (views/leads)? (if supported by the ad product tables) 
    

57. Agent portfolio performance: top agents by unique leads in [SG]. 
    

58. Listing performance comparison: [intent sale vs rent] in [TH]. 
    

59. Channel composition of leads for [SG] (WhatsApp vs call vs email). 
    

60. High-visibility but low-conversion listings by district in [SG]. 
    

H) Marketing & Lifecycle (5+) 

61. App installs trend in [SG] for [period]. 
    

62. Braze lifecycle step performance: completion and downstream listing engagement in [MY]. 
    

63. Turbo Pro attribution: WhatsApp first-touch views in [SG]. 
    

64. Campaign cohort: do campaigns produce higher-quality lead actions in [TH]? 
    

65. App installs → listing engagement conversion rate in [SG]. 
    

I) Specialized: SG Transactions (4+) 

66. Completed transaction trends (resale/sub-sale/rentals) in [SG] for [period]. 
    

67. Transaction volume by property type and district in [SG]. 
    

68. Agent participation counts for transactions in [SG]. 
    

69. Onboarded vs non-onboarded agent lens for SG transactions in [period]. 
    

Quick “Best Practice” Prompt Templates  

Use these if stakeholders want consistency: 

1. [Market], [start–end], [daily/weekly/monthly] — top [N] [projects/listings] by [metric] for [sale/rent], split by [platform/intent/property type]. 
    

2. [Market], [period] — show LDPV + Real Impressions + Lead counts and compute conversion (views→leads) by [platform/intent/district]. 
    

3. [Market], [period] — compare web vs app for [LDPV / Leads / Search], and show the biggest drivers behind the change. 
    

4. Web experiment, [Market], [variant A vs B] — which variant drove higher lead volume and lead conversion?