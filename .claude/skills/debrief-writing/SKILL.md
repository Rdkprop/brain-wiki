---
name: debrief-writing
description: Debrief / post-mortem pattern for this vault. Consult BEFORE creating
  or editing any file in dev/debriefs/. Blameless format focused on learning, not
  attribution.
---

# Skill: Debrief / Post-mortem Writing

Debriefs document incidents or significant events. The goal is **learning**, not blame. Every debrief is blameless — describe systems and processes, never people.

## When to Create

- Production incident (any severity)
- Bug that took >2h to diagnose
- Technical decision that proved wrong and had to be reverted
- Sprint or project retrospective

## File Naming

`dev/debriefs/YYYY-MM-DD-short-slug.md`

## Required Frontmatter

```yaml
---
title: One sentence describing what happened
type: debrief
incident-date: 2026-06-01
severity: low | medium | high | critical
duration-minutes: 45
tags: [tag1, tag2]
related-projects: []
related-adrs: []
---
```

## Section Structure

```markdown
# Debrief: <title>

## TL;DR

3 sentences: what happened, impact, root cause.

## Timeline

Chronological. Use explicit timezone.

- 14:32 SGT — Alert fires
- 14:35 SGT — Investigation begins
- 14:48 SGT — Root cause identified
- 15:17 SGT — Resolved

## Root Cause

Honest technical analysis. No softened language.

## What Worked

3–5 bullets. Systems and processes that functioned correctly.

## What Didn't Work

3–5 bullets. No people names — describe the system or process failure.

## Action Items

Numbered checklist. Link to projects or ADRs where actions will be tracked.

- [ ] 1. Add timeout in [[Project-X]] for external API calls
- [ ] 2. Update [[ADR-0003]] with new constraint discovered

## Generalizable Learning

1–2 paragraphs. **Most important section.** What pattern applies beyond this incident?
Aim for a transferable rule: "Systems that X must Y."
This is what makes debriefs compound in value over time.
```
