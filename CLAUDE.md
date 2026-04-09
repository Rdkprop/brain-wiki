# CLAUDE.md — Brain Wiki Schema & LLM Workflow

This document guides the LLM (me) on how to maintain and evolve your personal knowledge base. Read this before every session.

---

## Core Principles

1. **The wiki is the product**, not chat history. Every valuable insight gets filed.
2. **Humans curate, LLMs compile.** You decide what's worth reading; I do the synthesis.
3. **Sources are immutable.** Read from `raw/`, write to `wiki/`.
4. **Connections are valuable.** Cross-references matter as much as individual pages.

---

## Page Structure & Conventions

### Frontmatter (YAML)

Every wiki page starts with metadata:

```yaml
---
title: Page Title
type: entity | concept | topic | synthesis | reference
domain: personal | research | work | hobby
created: 2026-04-09
updated: 2026-04-09
related_pages:
  - page_name_1
  - page_name_2
confidence: high | medium | low
sources:
  - source_name_or_file
tags: [tag1, tag2]
---
```

### File Naming

- Entities (people, projects): `john_doe.md`, `project_name.md`
- Concepts: `concept_name.md`
- Topics: `topic_deep_dive.md`
- Syntheses: `synthesis_comparison.md`
- Always lowercase, hyphens for spaces: `my_concept.md`

### Page Types

| Type | Purpose | Example |
|------|---------|---------|
| **entity** | Person, organization, project, place | Rohan Kajgaonkar, Data team |
| **concept** | Abstract idea or principle | LLM wiki pattern, compound learning |
| **topic** | Deep-dive into a subject | Data analysis methodology, prompt engineering |
| **synthesis** | Your unique connections/analysis | Comparison, analysis, personal insight |
| **reference** | Quick lookup, definitions | Technical terms, process checklists |

---

## Operations

### 1. Ingest

When you hand me a new source:

1. **Read the source** → Skim for key takeaways
2. **Discuss with you** → Ask clarifying questions if needed
3. **Create/update pages** → Extract entities, concepts, connections
4. **Update index.md** → Add new pages to the catalog
5. **Update log.md** → Record what was ingested
6. **Suggest gaps** → What's missing or needs follow-up?

**Example flow:**
```
User: "I read this article on PKM systems"
Me: "I've read it. Key takeaway: LLMs reduce the maintenance burden. 
     I'm updating: CLAUDE.md updated link + tag. Should I create a 
     page comparing this to your current system? Also, any sections 
     you want me to emphasize?"
```

### 2. Query

When you ask a question:

1. **Read index.md** → Find relevant pages
2. **Read those pages** → Drill into the detail
3. **Synthesize** → Connect dots, cite sources
4. **File the answer** → Create a new synthesis page if valuable
5. **Update cross-refs** → Link from related pages back to this

**Format answers as:**
- Direct answer first
- Evidence/reasoning from wiki
- Links to relevant pages: `[[page_name]]`
- New connections discovered
- Follow-up questions

### 3. Lint (Health Check)

Periodically, I'll ask to run a lint pass:

**Search for:**
- Contradictions between pages
- Stale claims (newer sources supersede old)
- Orphan pages (no inbound links)
- Missing cross-references
- Important concepts mentioned but lacking their own page
- Data gaps

**Example lint output:**
```
Issues found:
1. page_a claims X, page_b claims ¬X (contradictory)
2. learning_methods.md not linked from overview.md (orphan potential)
3. "Obsidian" mentioned 3 times but no dedicated page
```

---

## Wikilink Conventions

Use **double brackets** to create connections:

```markdown
This relates to [[concept_name]].
See also [[entity_page]] for more context.
In contrast with [[other_concept]].
```

**Rules:**
- Link when meaningful, not everywhere
- Link to the most specific relevant page
- If page doesn't exist yet, I can create it or note as TODO

---

## File Locations (Directory Guide)

```
wiki/
├── overview.md              # Master synthesis, all domains
├── entities/                # People, teams, organizations
│   ├── rohan_kajgaonkar.md
│   ├── your_team.md
│   └── ...
├── topics/                  # Deep dives
│   ├── data_analysis.md
│   ├── personal_knowledge_mgmt.md
│   └── ...
├── concepts/                # Standalone ideas
│   ├── compound_learning.md
│   ├── llm_wiki_pattern.md
│   └── ...
├── syntheses/               # Your analysis
│   ├── comparison_tools.md
│   └── ...
└── index.md                 # Catalog
```

---

## Log Format (log.md)

Keep log.md as an append-only record. Use consistent prefixes so it's grep-able:

```markdown
# Brain Wiki Log

## [2026-04-09] ingest | Karpathy's LLM Wiki Gist
- Pages created: llm_wiki_pattern, karpathy_insights
- Pages updated: overview, index
- Sections emphasized: wiki as compound artifact, maintenance cost near-zero

## [2026-04-09] query | How to structure a wiki?
- Answer filed at: [[starting_a_wiki]]
- New connections: [[karpathy_lessons]] → [[my_wiki_setup]]

## [2026-04-09] lint | Health check
- Orphans found: 3 (fixed: linked 2, archived 1)
- Contradictions: 0
- Suggestions: Create dedicated page for "PKM systems comparison"
```

---

## Index Format (index.md)

Maintain a structured catalog. Example:

```markdown
# Index

## Overview
- [[overview]] — Master synthesis of everything

## Entities
### People
- [[rohan_kajgaonkar]] — Your personal page | 2026-04-09 | 3 sources

### Teams/Orgs
- [[my_team]] — Team structure and dynamics | 2026-04-08 | 5 sources

## Topics
- [[data_analysis]] — Methodology and tools | 2026-04-01 | 8 sources
- [[llm_and_ai]] — LLM systems and applications | 2026-04-09 | 12 sources

## Concepts
- [[compound_learning]] — Knowledge that compounds over time | 2026-04-09 | 2 sources
- [[llm_wiki_pattern]] — Personal wiki maintained by LLM | 2026-04-09 | 1 source

## Recent (Last 5)
- 2026-04-09 — [[llm_wiki_pattern]]
- 2026-04-09 — [[my_wiki_setup]]
```

---

## Dos and Don'ts

### Do ✓
- File back valuable discoveries as new wiki pages
- Create connections between domains
- Ask clarifying questions before filing something disputed
- Update timestamps and metadata
- Link liberally (but meaningfully)
- Flag contradictions explicitly

### Don't ✗
- Modify `raw/` — that's your source of truth
- Create duplicate pages (merge instead)
- Leave pages orphaned (update index + cross-refs)
- Skip the log (it's your wiki's audit trail)
- Forget to update frontmatter when pages change
- Make up wikilinks to pages that don't exist

---

## Sample Workflows

### Workflow 1: Ingest an Article

```
User: "Read this thing and process it"
[user: drops file in raw/articles/]

Me:
1. Read the article
2. "Okay, got it. Key insights:
   - Point A
   - Point B (contradicts [[old_page]])
   - Point C is new
   
   Creating/updating: [[concept_from_article]], [[entity_name]]
   Questions for you: Should I emphasize X or Y?"

3. based on feedback, refine the pages
4. "Filed at [[new_page]]. Updated [[overview]] with new context."
5. Log entry: 
   ## [2026-04-09] ingest | Article Name
   - Pages created: 1
   - Pages updated: 3
   - Contradictions flagged: 1
```

### Workflow 2: Answer a Question

```
User: "How does LLM-driven knowledge management differ from RAG?"

Me:
1. Read [[llm_wiki_pattern]], [[rag_systems]], [[knowledge_mgmt_approaches]]
2. Answer with synthesis + citations
3. "This is worth keeping. Filing as [[llm_vs_rag_synthesized]]"
4. Update [[llm_wiki_pattern]] to link to new synthesis page
5. Log entry:
   ## [2026-04-09] query | LLM vs RAG
   - Answer filed at: [[llm_vs_rag_synthesized]]
```

### Workflow 3: Lint Pass

```
Me: "Running lint check on wiki..."

Findings:
- 2 orphans (pages with no inbound links)
- 1 contradiction
- 3 important concepts mentioned but missing dedicated pages
- [[overview]] last updated 5 days ago (needs refresh?)

Recommendations:
1. Create: [[recommendation_systems]]
2. Merge: page_a and page_b (duplicates)
3. Resolve: contradiction in [[topic_x]]

Shall I proceed?
```

---

## Integration with Obsidian

The wiki works seamlessly with Obsidian:

1. Open Obsidian, point to `brain-wiki/wiki/`
2. Enable graph view (shows all connections)
3. Use backlinks to see what links to a page
4. Use search to find pages
5. Use quick switcher (CMD+O) to jump between pages

The LLM writes to `wiki/`, Obsidian picks it up automatically.

---

## Getting the Most From This

1. **Curate actively** — Better sources = better wiki
2. **Let me know your interests** — I'll guide what to emphasize
3. **Review my work** — Check pages I create, give feedback
4. **Follow links** — The graph view shows connections you might not expect
5. **Periodic lint** — Every 10-15 ingests, run a health check

---

## Version

- Created: 2026-04-09
- Last updated: 2026-04-09

*This schema is a living document. We'll iterate based on what works for your workflow.*
