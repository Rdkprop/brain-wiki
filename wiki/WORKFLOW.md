# Daily Wiki Workflow

Four workflows. Use the one that fits what you're doing.

---

## Workflow 1: Ingest a Source

**When:** You find an article, paper, book chapter, or want to process your own notes.

### Step 1: Save to `raw/`

| What | Where |
|------|-------|
| Web article (via Obsidian Web Clipper) | `raw/articles/` |
| PDF paper or report | `raw/research/` |
| Book chapter notes or highlights | `raw/books/` |
| Your own notes, transcripts, screenshots | `raw/notes/` |
| Images | `raw/assets/` |

**Web Clipper setup:** In the plugin settings, set the output folder to `raw/articles/` so clips land there automatically.

### Step 2: Tell Me to Process It

```
Ingest this source: raw/articles/[filename]
Domain: work / personal  (pick one)
Emphasize: [anything specific you want highlighted]
```

I'll:
- Read the source
- Ask clarifying questions if the domain or emphasis is unclear
- Create/update pages in `wiki/` (entities/, concepts/, topics/, syntheses/ as appropriate)
- Update `wiki/index.md` and `wiki/log.md`
- Report: pages created, pages updated, contradictions found, gaps suggested

### Step 3: Review and Redirect

```
Looks good. Also link [[concept_A]] to [[concept_B]].
I disagree with the X section — here's my take: ...
Emphasize Y more, X less.
```

---

## Workflow 2: Ask a Question

**When:** You want to synthesize what you know, explore a topic, or connect ideas across sources.

```
Question: How does X relate to Y?
Relevant pages: [[page_1]], [[page_2]]  (optional hint)
```

I'll:
1. Read `wiki/index.md` → find relevant pages
2. Read those pages, dig into sources if needed
3. Synthesize with citations to `[[pages]]`
4. File the answer as a synthesis page if it's worth keeping
5. Update cross-references

---

## Workflow 3: Dev Co-pilot

**When:** You're drafting an ADR, writing up a project debrief, or taking meeting notes.

### Step 1: Draft in `dev/`

Use the naming conventions:
- `dev/adr/YYYY-MM-DD-decision-title.md`
- `dev/meetings/YYYY-MM-DD-topic.md`
- `dev/projects/project-name.md`
- `dev/reading/author-or-title.md`

### Step 2: Ask Me to Review

```
Review my draft at dev/adr/2026-06-01-switch-to-dbt.md
Suggest wikilinks and related context.
```

I'll:
- Read the draft
- Suggest `[[wikilinks]]` to relevant wiki pages
- Find earlier ADRs or project notes that relate or conflict
- Flag sections worth promoting to `wiki/`
- Propose edits — I won't apply them without your approval

---

## Workflow 4: Lint (Health Check)

**When:** Every 10–15 ingests, or whenever the wiki feels stale.

```
Run a lint pass on wiki/
```

I'll check for:
- Orphan pages (no inbound links)
- Contradictions between pages
- Stale claims (newer sources supersede older)
- Concepts mentioned 3+ times with no dedicated page
- Gaps worth investigating next

---

## Obsidian Quick Reference

| Action | Shortcut |
|--------|----------|
| Quick switcher (jump to any page) | Cmd+O |
| Search all pages | Cmd+Shift+F |
| Open graph view | Toolbar icon (top right) |
| Show backlinks | Right panel |
| Refresh vault | Cmd+R |

**Dataview examples:**

All work pages, recent first:
```dataview
TABLE updated, confidence FROM "wiki"
WHERE domain = "work"
SORT updated DESC
```

Pages with no inbound links:
```dataview
TABLE file.inlinks FROM "wiki"
WHERE length(file.inlinks) = 0
```

---

## Pushing to GitHub

I don't touch git. Commit manually when you want to save a snapshot:

```bash
cd brain-wiki
git add -A
git commit -m "Wiki updates: [topic ingested]"
git push
```

---

## Troubleshooting

**Obsidian not showing new pages** — Cmd+R to refresh, or close and reopen the tab.

**Wikilinks not resolving** — filename in `[[brackets]]` must match exactly (no `.md`, case-sensitive).

**Graph view looks empty** — it only renders pages that have links. Start ingesting sources and it'll populate.

**Vault opened at wrong folder** — vault should be opened at `brain-wiki/` (the repo root), not `brain-wiki/wiki/`.
