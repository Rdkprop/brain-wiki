# 🧠 Brain Wiki

A personal knowledge base that grows and compounds as you learn. Built on the [LLM Wiki pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) by Andrej Karpathy.

## Structure

```
brain-wiki/
├── raw/              # Source documents (immutable)
│   ├── articles/
│   ├── research/
│   ├── books/
│   ├── notes/
│   └── assets/       # Images, PDFs
├── wiki/             # LLM-maintained knowledge base
│   ├── overview.md   # Master synthesis
│   ├── entities/     # People, concepts, organizations
│   ├── topics/       # Deep-dive topics
│   ├── connections/  # Cross-domain relationships
│   └── index.md      # Navigation index
├── CLAUDE.md         # Schema & workflow guide for LLM
├── index.md          # Catalog of all wiki pages
└── log.md            # Append-only operation log
```

## How it works

1. **You curate sources** → Drop documents into `raw/`
2. **LLM processes** → Reads sources, extracts knowledge, updates wiki
3. **You explore** → Browse in Obsidian, follow connections
4. **Knowledge compounds** → Each source strengthens existing synthesis

## Getting started

See `CLAUDE.md` for the full workflow and instructions for the LLM.

## Tips

- Use Obsidian's graph view to see knowledge structure
- Follow links in the wiki to discover connections
- The index.md is your map; log.md is your history
- Contradictions and gaps signal important research opportunities
