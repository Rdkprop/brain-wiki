# CLAUDE.md — Brain Wiki Schema

Read this before every session. It defines how this vault is structured and what I am and am not allowed to do.

---

## Zone Map

```
brain-wiki/          ← vault root (Obsidian opens here)
├── CLAUDE.md        ← Zone 0: this file, operating rules
├── raw/             ← Zone 1: your curated sources, immutable
├── wiki/            ← Zone 2: I maintain this
└── dev/             ← Zone 3: collaborative, you drive, I co-pilot
```

### Zone Permissions

| Zone | Who writes | Rule |
|------|-----------|------|
| `raw/` | You only | I never edit or create files here. Read-only. |
| `wiki/` | Me (with your approval) | I create, update, link, and synthesize pages. |
| `dev/` | You primarily | I suggest edits, propose wikilinks, find related ADRs — but I wait for approval before writing. |
| `CLAUDE.md` | Either, deliberately | Only change when the schema itself needs to evolve. |

---

## raw/ — Zone 1 (Immutable Sources)

```
raw/
├── articles/    # Web clips, blog posts (via Obsidian Web Clipper)
├── research/    # Papers, reports
├── books/       # Chapter notes, highlights
├── notes/       # Your own fleeting notes, transcripts, screenshots
└── assets/      # Images referenced from notes
```

Files here stay exactly as you saved them — rough formatting, leftover ads, whatever. Immutability means I can always trace what I synthesized from.

**Obsidian Web Clipper:** clips save directly to `raw/articles/`. Set the output path to `raw/articles/{{title}}.md` in the plugin settings.

---

## wiki/ — Zone 2 (Agent-Maintained Knowledge)

```
wiki/
├── overview.md      # Master synthesis of everything
├── index.md         # Catalog of all pages
├── log.md           # Append-only operation history
├── entities/        # People, teams, orgs, projects
├── topics/          # Deep dives into a subject area
├── concepts/        # Standalone ideas and principles
└── syntheses/       # Your unique analysis, comparisons, insights
```

### Frontmatter (every wiki page)

```yaml
---
title: Page Title
type: entity | concept | topic | synthesis | reference
domain: personal | work | research | hobby
created: 2026-06-01
updated: 2026-06-01
related_pages:
  - page_name_1
confidence: high | medium | low
sources:
  - raw/articles/source_file.md
tags: [tag1, tag2]
---
```

**`domain:`** — how to split work vs personal. Use `work` for anything PropertyGuru/data-team related, `personal` for everything else. No separate folders needed — Dataview queries can filter on this field.

### File Naming

- lowercase with underscores: `compound_learning.md`, `rohan_kajgaonkar.md`
- Entities: `firstname_lastname.md` or `project_name.md`
- Concepts: `concept_name.md`
- Topics: `topic_deepdive.md`
- Syntheses: `comparison_x_vs_y.md` or `analysis_topic.md`

### Page Types

| Type | Purpose | Folder |
|------|---------|--------|
| `entity` | Person, team, org, project | `wiki/entities/` |
| `concept` | Abstract idea, principle | `wiki/concepts/` |
| `topic` | Deep-dive subject | `wiki/topics/` |
| `synthesis` | Your analysis/comparison | `wiki/syntheses/` |
| `reference` | Quick-lookup, checklist | `wiki/` root or appropriate subfolder |

---

## dev/ — Zone 3 (Collaborative Work/Project Notes)

```
dev/
├── adr/         # Architecture Decision Records
├── projects/    # Project debriefs, status notes
├── meetings/    # Meeting notes, action items
└── reading/     # Technical reading notes (not processed into wiki yet)
```

**Workflow:** You draft here — rough notes, half-formed thoughts, raw ADRs. I read dev/ pages and:
- Suggest wikilinks to relevant wiki/ pages
- Find earlier ADRs that contradict or relate
- Propose which dev/ content is ready to promote into wiki/

I do not auto-write into dev/. I propose, you approve.

**Naming convention (dev/):**
- ADRs: `adr/YYYY-MM-DD-decision-title.md`
- Meetings: `meetings/YYYY-MM-DD-topic.md`
- Projects: `projects/project-name.md`
- Reading: `reading/author-or-title.md`

---

## Wikilinks

Use `[[filename]]` (without `.md`). Link when meaningful, not everywhere.

```markdown
This relates to [[compound_learning]].
Compare with [[karpathy_llm_wiki]].
See also [[rohan_kajgaonkar]] for context.
```

If the target page doesn't exist yet, I note it as a TODO rather than inventing the page.

---

## Operations

### Ingest (raw/ → wiki/)

When you drop a source in `raw/`:

1. Read the source
2. Ask clarifying questions if the domain or emphasis is unclear
3. Create/update pages in appropriate `wiki/` subfolder
4. Update `wiki/index.md` and `wiki/log.md`
5. Report: pages created, pages updated, contradictions found, gaps suggested

### Query

When you ask a question:

1. Read `wiki/index.md` → identify relevant pages
2. Read those pages, dig into sources if needed
3. Synthesize with citations (`[[page]]`)
4. File the answer as a synthesis page if it's worth keeping
5. Update cross-references

### Dev Co-pilot

When you share a dev/ draft:

1. Read the file
2. Identify relevant `wiki/` pages and suggest wikilinks
3. Find related ADRs or project notes
4. Suggest whether any section deserves a wiki/ page
5. Propose edits — do not apply without approval

### Lint (Health Check)

Every 10–15 ingests, run:

```
Run a lint pass on wiki/
```

I'll check for: orphan pages, contradictions, stale claims, missing cross-references, concepts mentioned 3+ times with no dedicated page.

---

## Dataview Queries

Since you're using Dataview, here are useful query patterns:

**All work pages, recent first:**
```dataview
TABLE updated, confidence FROM "wiki"
WHERE domain = "work"
SORT updated DESC
```

**Orphan pages (no inbound links):**
```dataview
TABLE file.inlinks FROM "wiki"
WHERE length(file.inlinks) = 0
```

**Pages by type:**
```dataview
TABLE type, domain, updated FROM "wiki"
SORT type ASC
```

---

## Templater Templates

Suggested templates (save in `.obsidian/templates/`):

**Wiki page template:**
```markdown
---
title: {{title}}
type: 
domain: 
created: {{date:YYYY-MM-DD}}
updated: {{date:YYYY-MM-DD}}
related_pages: []
confidence: medium
sources: []
tags: []
---

# {{title}}
```

**Dev meeting note:**
```markdown
---
date: {{date:YYYY-MM-DD}}
type: meeting
project: 
attendees: []
tags: [meeting]
---

# {{date:YYYY-MM-DD}} — 

## Context

## Decisions

## Actions
- [ ] 
```

**ADR template:**
```markdown
---
date: {{date:YYYY-MM-DD}}
status: proposed | accepted | superseded
supersedes: 
tags: [adr]
---

# ADR: {{title}}

## Context

## Decision

## Consequences
```

---

## log.md Format

Append-only. Format:

```markdown
## [YYYY-MM-DD] ingest | Source Title
- Pages created: name1, name2
- Pages updated: name3
- Contradictions: none
- Gaps suggested: create [[missing_concept]]

## [YYYY-MM-DD] query | Question asked
- Answer filed at: [[synthesis_page]]

## [YYYY-MM-DD] lint | Health check
- Orphans: 2 fixed
- Contradictions: 0
- Suggestions: create [[missing_page]]
```

---

## Dos and Don'ts

### Do
- Read `raw/` freely
- Create/update pages in `wiki/` based on what you read
- Ask before creating a page in a domain you're unsure about (work vs personal)
- Link pages liberally (but meaningfully)
- Flag contradictions explicitly
- Propose dev/ edits — don't apply them

### Don't
- Edit files in `raw/` — ever
- Write to `dev/` without explicit approval
- Create duplicate pages (check index first, merge instead)
- Invent wikilinks to pages that don't exist — note as TODO instead
- Skip updating `log.md` after any operation

---

## Version

- Created: 2026-04-09
- Restructured: 2026-06-01 (4-zone schema, Dataview/Templater support, dev/ zone added)
