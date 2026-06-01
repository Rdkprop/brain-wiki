---
name: adr-writing
description: Architecture Decision Records (ADRs) pattern for this vault. Consult
  BEFORE creating or editing any file in dev/adr/. Defines numbering, frontmatter,
  section structure, and status flow.
---

# Skill: ADR Writing

ADRs in this vault follow the MADR (Markdown Architecture Decision Records) format. Each ADR is a `.md` file in `dev/adr/`.

## Numbering

Files: `dev/adr/ADR-NNNN-short-slug.md`. NNNN is the next available integer, zero-padded to 4 digits. E.g., `dev/adr/ADR-0007-use-pgvector-for-rag.md`.

Before creating a new ADR:
1. READ all files in `dev/adr/` to find the next number.
2. Check if an ADR on the same topic already exists — if so, update it instead of creating a new one.

## Required Frontmatter

```yaml
---
title: Short imperative title of the decision
type: adr
status: proposed | accepted | rejected | superseded
decision-date: 2026-06-01
deciders: [Rohan]
tags: [tag1, tag2]
supersedes: []
superseded-by: []
---
```

## Section Structure

```markdown
# ADR-NNNN: <Short imperative title>

## Context

2–4 paragraphs describing the problem and what motivated this decision. Include:
- Observed symptom or business requirement
- Known constraints
- Wikilinks to related projects or concepts [[Project-X]]

## Decision

One direct paragraph. "We will use X." No adjectives. No "after careful analysis."

## Consequences

### Positive
- Short bullet

### Negative / trade-offs
- Short bullet

### Neutral
- Short bullet

## Alternatives Considered

Brief list. For each: why rejected in 1–2 sentences.

## References

- [[raw/research/...]]
- [[wiki/concepts/...]]
- External URLs when relevant
```

## Status Flow Rules

- `proposed` → can be freely edited
- `accepted` → **IMMUTABLE** except for status field change (accepted → superseded)
- `superseded` → link `superseded-by` to the new ADR number
- Never delete an old ADR. Mark it superseded and link forward.
- If you find a contradiction between two ADRs, **do not resolve alone** — report to Rohan.
