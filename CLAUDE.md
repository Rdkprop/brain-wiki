# CLAUDE.md — Brain Wiki Schema

Read this before every session. It defines how this vault is structured, what I am and am not allowed to do, and how operations work.

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
| `CLAUDE.md` | Either, deliberately | Only change when the schema itself needs to evolve. Ask before editing. |

---

## raw/ — Zone 1 (Immutable Sources)

```
raw/
├── articles/    # Web clips, blog posts (via Obsidian Web Clipper)
├── research/    # Papers, reports
├── books/       # Chapter notes, highlights
├── notes/       # Fleeting notes, transcripts, screenshots
├── daily/       # Daily stream-of-consciousness notes
└── assets/      # Images referenced from notes
```

Files here stay exactly as saved — rough formatting, leftover ads, whatever. Immutability means sources can always be traced.

**Obsidian Web Clipper:** set output path to `raw/articles/{{title}}.md`.

**Daily notes:** free-form stream of consciousness in `raw/daily/YYYY-MM-DD.md`. No forced structure. Used as input for weekly synthesis (see Weekly Synthesis Workflow below).

---

## wiki/ — Zone 2 (Agent-Maintained Knowledge)

```
wiki/
├── overview.md      # Master synthesis
├── index.md         # Catalog of all pages
├── log.md           # Append-only operation history
├── entities/        # People, teams, orgs, projects
├── topics/          # Deep dives into a subject area
├── concepts/        # Standalone ideas and principles
└── syntheses/       # Unique analysis, comparisons, insights
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

**`domain:`** — `work` for PropertyGuru/data-team, `personal` for everything else.

### Wikilink Conventions

- **ALWAYS** use `[[wikilinks]]` for internal links. **NEVER** `[text](file.md)`.
- Concepts use Title Case: `[[Lead Tracking]]`, `[[Active Home Seeker]]`
- Entities: `[[Rohan Kajgaonkar]]`, `[[PropertyGuru]]`
- File names remain lowercase_underscore; wikilinks display as Title Case via Obsidian aliases if needed.
- If a target page doesn't exist yet, note it as a TODO — never invent the page.

### File Naming

- lowercase with underscores: `compound_learning.md`, `active_home_seeker.md`
- Entities: `firstname_lastname.md` or `org_name.md`
- Concepts: `concept_name.md`
- Topics: `topic_deepdive.md`
- Syntheses: `comparison_x_vs_y.md` or `analysis_topic.md`

### Page Types

| Type | Purpose | Folder |
|------|---------|--------|
| `entity` | Person, team, org, project | `wiki/entities/` |
| `concept` | Abstract idea, principle | `wiki/concepts/` |
| `topic` | Deep-dive subject | `wiki/topics/` |
| `synthesis` | Analysis/comparison | `wiki/syntheses/` |
| `reference` | Quick-lookup, checklist | `wiki/` root |

---

## dev/ — Zone 3 (Collaborative Work/Project Notes)

```
dev/
├── adr/         # Architecture Decision Records
├── debriefs/    # Post-mortems and incident retrospectives
├── projects/    # Project notes, status, debriefs
├── meetings/    # Meeting notes, action items
├── snippets/    # Reusable code/config fragments
└── reading/     # Technical reading notes (not yet promoted to wiki/)
```

**Workflow:** You draft here — rough notes, half-formed thoughts, raw ADRs. I:
- Suggest wikilinks to relevant wiki/ pages
- Find earlier ADRs that contradict or relate
- Propose which dev/ content is ready to promote into wiki/

I do not auto-write into dev/. I propose, you approve.

**Naming conventions:**
- ADRs: `adr/ADR-NNNN-decision-slug.md` — consult `adr-writing` skill before creating
- Debriefs: `debriefs/YYYY-MM-DD-slug.md` — consult `debrief-writing` skill before creating
- Meetings: `meetings/YYYY-MM-DD-topic.md`
- Projects: `projects/project-name.md`
- Snippets: `snippets/description.md`
- Reading: `reading/author-or-title.md`

---

## Available Skills

Skills live in `.claude/skills/`. Consult the relevant skill before using these patterns:

| Skill | When to use |
|-------|------------|
| `obsidian-markdown` | **Always** — correct wikilink syntax, callouts, frontmatter, embeds |
| `obsidian-bases` | Creating `.base` database views |
| `json-canvas` | Creating `.canvas` whiteboards |
| `obsidian-cli` | Automating vault operations via `obsdmd` |
| `defuddle` | Fetching a URL — strips ads/nav, extracts clean content |
| `adr-writing` | Before creating or editing any file in `dev/adr/` |
| `debrief-writing` | Before creating or editing any file in `dev/debriefs/` |

---

## Operations

### Ingest (raw/ → wiki/)

Use the `/wiki-ingest` command for structured ingestion. The command handles:
1. Fetching/reading the source
2. Saving to the correct `raw/` subfolder
3. Analysing for concepts and entities
4. **Presenting a plan and waiting for approval** before writing to wiki/
5. Executing the approved plan
6. Updating `wiki/index.md` and `wiki/log.md`

**Plan-first rule:** If an ingest affects more than 5 files, I MUST show the full plan before executing. No exceptions.

### Query

Use the `/wiki-query` command. It:
1. Searches with `grep` before reading (keeps token cost low)
2. Reads up to 10 relevant files, prioritising wiki/
3. Answers with `[[wikilink]]` citations
4. Signals clearly when inferring vs citing
5. Never fabricates — says "not in vault" if the information isn't there

### Weekly Synthesis (Daily Notes → Wiki)

Periodically ask:
```
Synthesise the week reading raw/daily/YYYY-MM-DD.md through raw/daily/YYYY-MM-DD.md.
Identify: recurring themes, pending decisions, ideas ready for wiki/, links to existing ADRs.
Present as a report only — do not create files.
```

### Dev Co-pilot

When you share a dev/ draft:
1. Read the file
2. Identify relevant wiki/ pages and suggest wikilinks
3. Find related ADRs or project notes
4. Suggest whether any section deserves a wiki/ page
5. Propose edits — do not apply without approval

### Lint (Health Check)

Every 10–15 ingests:
```
Run a lint pass on wiki/
```
Checks: orphan pages, contradictions, stale claims, missing cross-references, concepts mentioned 3+ times with no dedicated page.

---

## Strict Limits

- **NEVER edit files in `raw/`** — ever, for any reason
- **NEVER run `git add`, `git commit`, or `git push`** — version control is manual
- **NEVER edit `CLAUDE.md` itself** — propose changes, let Rohan edit
- **NEVER delete files** without explicit confirmation ("yes, delete X")
- **NEVER write to `dev/`** without explicit approval
- **If an operation affects more than 5 files**, show the full plan before executing
- **If unsure which zone a file belongs to**, ask before acting
- **Create duplicate pages** — always check index first, merge instead
- **Invent wikilinks** to pages that don't exist — note as TODO

---

## Dataview Queries

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

Save in `.obsidian/templates/`.

**Wiki page:**
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

**Daily note:**
```markdown
---
date: {{date:YYYY-MM-DD}}
type: daily
tags: [daily]
---

# {{date:YYYY-MM-DD}}

## Today

## Decisions pending

## Ideas
```

**Dev meeting:**
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

**ADR:** consult `adr-writing` skill — use its template.

**Debrief:** consult `debrief-writing` skill — use its template.

---

## log.md Format

Append-only:

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

## Version

- Created: 2026-04-09
- Restructured: 2026-06-01 (4-zone schema, Dataview/Templater support, dev/ zone)
- Updated: 2026-06-01 (skills, slash commands, strict limits, plan-first ingestion, daily notes, dev/debriefs, dev/snippets)
