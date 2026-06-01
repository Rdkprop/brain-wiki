---
description: Ingest a URL or file into the vault — saves to raw/, extracts concepts, presents plan, waits for confirmation before writing to wiki/
argument-hint: <URL | raw/ file path>
allowed-tools: WebFetch, Bash(curl:*), Bash(cat:*), Bash(ls:*), Bash(grep:*), Bash(find:*)
---

Ingest **$ARGUMENTS** into the vault following the ingestion workflow in CLAUDE.md.

Execute in this exact order:

## Step 1 — Determine source type

- If URL → consult the `defuddle` skill to extract clean content
- If local file path → read directly
- If ambiguous → ask before proceeding

## Step 2 — Save to raw/

Save the source content to the appropriate raw/ subfolder:
- Web article → `raw/articles/YYYY-MM-DD-slug.md`
- Research paper → `raw/research/YYYY-MM-DD-slug.md`
- Book notes → `raw/books/slug.md`
- Personal notes → `raw/notes/slug.md`

Slug = title in kebab-case, max 60 chars.

Frontmatter must include:
```yaml
---
title:
source-url:
captured-date:
author:
domain: work | personal
tags: []
---
```

## Step 3 — Analyse

Before writing anything to wiki/:
- Identify 3–7 key concepts
- Identify 1–3 entities (people, teams, orgs, projects)
- Run `ls wiki/concepts/ wiki/entities/ wiki/topics/ wiki/syntheses/` to find what already exists
- Check existing pages with `grep` for related terms — don't create duplicates

## Step 4 — Present plan and WAIT

Present this plan and stop. Do not write a single file in wiki/ until confirmed.

```
INGESTION PLAN
──────────────
Source: <title>
Saved at: raw/.../filename.md

New pages to create:
- wiki/concepts/concept_name.md — one line description
- wiki/entities/entity_name.md — one line description

Existing pages to update:
- wiki/concepts/existing.md — what will change (add source / add nuance)

Wikilinks to add:
- [[page_a]] ↔ [[page_b]]

wiki/index.md update: yes | no
wiki/log.md update: yes

Affects N files. Can I proceed?
```

## Step 5 — Execute (only after approval)

- Create/update wiki/ pages per the approved plan
- Use `[[wikilinks]]` in Title Case for concepts, never `[text](file.md)`
- Consult `obsidian-markdown` skill for correct syntax
- Every new page must have full frontmatter (title, type, domain, created, updated, related_pages, confidence, sources, tags)

## Step 6 — Report

List every file created or updated. Done.
