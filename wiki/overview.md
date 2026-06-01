---
title: Brain Wiki Overview
type: reference
domain: personal
created: 2026-04-09
updated: 2026-06-01
related_pages:
  - index
confidence: high
sources: []
tags: [wiki, overview]
---

# Brain Wiki Overview

Your personal knowledge base for work and personal learning. It compounds as you add sources.

## How It Works

1. **You curate** — find articles, save notes, clip pages, drop PDFs
2. **I synthesize** — read sources, extract insights, build pages, maintain connections
3. **You explore** — browse in Obsidian, follow wikilinks, use graph view
4. **Knowledge compounds** — each new source strengthens what's already there

## Vault Structure

```
brain-wiki/
├── CLAUDE.md          # Zone 0 — schema and operating rules
├── raw/               # Zone 1 — your sources (immutable)
│   ├── articles/      # Web clips, blog posts
│   ├── research/      # Papers, reports
│   ├── books/         # Chapter notes, highlights
│   ├── notes/         # Fleeting notes, transcripts
│   └── assets/        # Images
├── wiki/              # Zone 2 — agent-maintained knowledge (here)
│   ├── overview.md    # This file
│   ├── index.md       # Catalog of all pages
│   ├── log.md         # Operation history
│   ├── entities/      # People, teams, orgs, projects
│   ├── topics/        # Deep dives
│   ├── concepts/      # Standalone ideas
│   └── syntheses/     # Your analysis and comparisons
└── dev/               # Zone 3 — collaborative work/project notes
    ├── adr/           # Architecture Decision Records
    ├── projects/      # Project notes, debriefs
    ├── meetings/      # Meeting notes
    └── reading/       # Technical reading notes
```

## The Three Zones

**raw/** — Your sources. Drop anything here. I read, never touch.

**wiki/** — Synthesized knowledge. I create pages, maintain links, track provenance. Use `domain: work` vs `domain: personal` in frontmatter to separate your PropertyGuru work context from personal learning — Dataview can filter on this.

**dev/** — Work in progress. ADRs, project debriefs, meeting notes, half-processed technical reading. You draft, I co-pilot: suggest wikilinks, find related context, flag when something is ready to promote into wiki/.

## Four Workflows

**Ingest** — drop a source in raw/, tell me to process → pages created, connections made

**Query** — ask a question → I synthesize from wiki pages, file the answer if worth keeping

**Dev co-pilot** — share a dev/ draft → I suggest links, find related context, propose edits

**Lint** — periodic health check → find orphans, contradictions, gaps

See [[WORKFLOW]] for step-by-step instructions.

## Key Files

- [[WORKFLOW]] — how to use this daily
- [[index]] — catalog of all wiki pages
- [[log]] — append-only history of operations

## Why It Works

The tedious part of a knowledge base is maintenance: updating cross-references, keeping summaries current, noting contradictions. Humans abandon wikis because the maintenance burden grows faster than the value does.

I handle the bookkeeping. You handle curation and thinking.
