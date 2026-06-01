---
description: Search the vault and answer a question by synthesising accumulated knowledge — grep first, read second, never hallucinate
argument-hint: <question in natural language>
allowed-tools: Bash(grep:*), Bash(find:*), Bash(cat:*), Bash(ls:*)
---

Answer **$ARGUMENTS** by consulting the vault.

## Strategy

### 1 — Candidate search (no full reads yet)

```bash
grep -r -l --include="*.md" "keyword" wiki/
```

Start with `wiki/` (most synthesised). If insufficient, expand to `dev/` then `raw/`.
Extract 2–3 keywords from the question and search each.

### 2 — Focused reading

- Read up to 10 candidate files, prioritising `wiki/` over `dev/` over `raw/`
- If more than 10 are relevant, read the most relevant and mention the rest
- Do NOT read the entire vault — for broad searches Obsidian's own search is better

### 3 — Answer synthesis

- Answer in direct prose (not bullets unless the question asks for a list)
- Always cite sources via `[[wikilinks]]`:
  - "Per [[wiki/concepts/lead_tracking_sot]], ..."
  - "In [[dev/adr/ADR-0003]], the decision was..."
- If the answer requires inference (not literally in the vault), signal it:
  - "Inferring from [[X]] and [[Y]]: ..."
- If the vault doesn't have enough information, say so clearly. Never fabricate.

### 4 — Suggest next steps

If there's a clear gap: "There's no page on X yet. Want me to run /wiki-ingest on Y to fill that in?"

If a synthesis page would be worth creating: propose it, don't create it unsolicited.
