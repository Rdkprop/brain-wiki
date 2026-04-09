---
title: Brain Wiki Overview
type: reference
domain: personal
created: 2026-04-09
updated: 2026-04-09
related_pages:
  - WORKFLOW
  - CLAUDE
confidence: high
sources: []
tags: [wiki, knowledge-base, overview]
---

# Brain Wiki Overview

This is your personal knowledge base. It grows and compounds as you learn.

## How It Works

1. **You curate sources** → Find articles, papers, notes, screenshots
2. **I process them** → Read sources, extract insights, build the wiki
3. **You explore** → Browse pages in Obsidian, follow connections
4. **Knowledge compounds** → Each source strengthens existing understanding

## The Pattern

This follows Andrej Karpathy's [LLM Wiki pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f):

- **Raw sources** (`raw/`) — Your curated input documents (immutable)
- **Wiki layer** (`wiki/`) — LLM-maintained knowledge (what you're reading now)
- **Schema** (`CLAUDE.md`) — How I maintain the wiki
- **Navigation** (`index.md`, `log.md`) — Find and track everything

## Three Main Workflows

### 📥 Ingest
Drop a source, I process it → new pages created, connections made

### ❓ Query
Ask a question → I synthesize from multiple sources, file the answer

### 🔍 Lint
Periodic health check → Find contradictions, orphans, gaps

**See [[WORKFLOW]] for step-by-step instructions.**

## Your Vault Structure

```
brain-wiki/
├── raw/              # Your source documents (immutable)
├── wiki/             # Knowledge base (you're reading this folder)
├── CLAUDE.md         # Schema & how I work
├── index.md          # Catalog of all pages
└── log.md            # History of all operations
```

## Getting Started

1. **Read [[WORKFLOW]]** — Daily workflow guide
2. **Find 3 sources** — Articles, notes, screenshots
3. **Save to `raw/`** — Organize into subfolders
4. **Tell me to ingest** — I'll process and create pages
5. **Explore in Obsidian** — Click wikilinks, use graph view

## Key Files

- [[WORKFLOW]] — How to use this wiki daily
- [[CLAUDE]] — Technical schema (read if curious about how I work)
- [[index]] — Catalog of everything
- [[log]] — Append-only history

## Why This Works

The tedious part of a knowledge base is **maintenance**: updating cross-references, keeping summaries current, noting contradictions. Humans abandon wikis because maintenance burden grows faster than value.

LLMs don't get bored. So I handle the bookkeeping. You handle curation and thinking. The wiki compounds because maintenance cost is near-zero.

## What's Next

→ [[WORKFLOW]] to get started
