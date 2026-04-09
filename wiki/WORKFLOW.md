# Quick Start: Daily Wiki Workflow

Your brain-wiki is ready! This guide shows you exactly how to use it every day.

---

## 📥 Workflow 1: Ingest a Source (You → Me → Wiki)

This is the core workflow. When you find an article, paper, or interesting content:

### Step 1: Save to `raw/`
```
brain-wiki/raw/
├── articles/
│   └── my_article.md          # Paste article text here (or .pdf, .txt)
├── research/
│   └── research_paper.pdf
├── books/
│   └── chapter_notes.md
└── notes/
    └── podcast_transcript.txt
```

**How to get sources into `raw/`:**
- **Articles:** Use Obsidian Web Clipper (browser extension) or just paste text
- **PDFs:** Drag & drop into the `raw/` folder
- **Notes:** Paste your own notes
- **Screenshots:** Take screenshots, save to `raw/assets/`

### Step 2: Tell Me to Process It

Open Copilot Chat and paste this (copy-paste the template):

```
I've saved a new source to my brain-wiki:
- Path: raw/articles/[filename]
- Title: [Article/Source Title]
- Domain: [personal/learning/research]

Please ingest this source:
1. Read and summarize the key takeaways
2. Identify new entities, concepts, and connections
3. Create/update wiki pages
4. Update index.md and log.md
5. Suggest any contradictions or gaps

Let me know what you find!
```

### Step 3: Review My Work

I'll:
- ✅ Create new pages for key concepts
- ✅ Update existing pages with new context
- ✅ Link pages together with wikilinks
- ✅ Update `index.md` (catalog) and `log.md` (history)
- ✅ Ask clarifying questions if needed

**In Obsidian:**
- Refresh (Cmd+R) to see new pages
- Click on pages to read them
- Follow wikilinks to explore connections
- Use graph view to see the network

### Step 4: Give Feedback (Optional)

```
Looks good! A few notes:
- Emphasis X more than Y
- Consider linking [[concept_A]] to [[concept_B]]
- I disagree with Z, here's why...
```

I'll refine based on your feedback.

---

## ❓ Workflow 2: Ask a Question (You → Me → Answer + Wiki Page)

When you want to synthesize knowledge or explore a topic:

### Step 1: Ask in Copilot Chat

```
Question: [Your question]
Relevant pages: [[concept_1]], [[concept_2]]  (optional)
```

### Step 2: I Synthesize

I'll:
- Read relevant wiki pages
- Dig into the sources if needed
- Connect dots across topics
- Return a clear answer with citations

### Step 3: I File the Answer

If the answer is valuable, I'll create a new synthesis page:
- **File at:** `wiki/syntheses/[analysis_name].md`
- **Update:** Cross-references from related pages
- **Link back:** To the sources I used

**In Obsidian:**
- Look for the new synthesis page in the file list
- Follow the citations to original sources
- See connections in graph view

---

## 🔍 Workflow 3: Lint Pass (Maintenance)

Every 10-15 ingests, run a health check:

```
Run a lint pass on the wiki:
- Find orphan pages (no links)
- Find contradictions
- Find concepts mentioned but missing dedicated pages
- Suggest what to investigate next
```

I'll report issues and suggest fixes. This keeps your wiki healthy as it grows.

---

## 🎯 Tips & Best Practices

### Obsidian Tips
- **Quick switcher:** Cmd+O to jump between pages
- **Graph view:** Click the graph icon (top right) to visualize connections
- **Backlinks:** Right panel shows pages that link to current page
- **Search:** Cmd+Shift+F to search all pages

### Wiki Tips
- **Follow wikilinks:** Click `[[page_name]]` to explore
- **Review index.md:** It's your map of everything
- **Check log.md:** Grep recent entries: `grep "^## \[" wiki/log.md | tail -5`
- **Read CLAUDE.md:** It's the schema I follow

### Sourcing Tips
- **Obsidian Web Clipper:** Save articles directly to markdown
- **Mix types:** Articles, papers, books, notes, screenshots all work
- **Organize in `raw/`:** Subfolders help (articles/, research/, books/, etc.)
- **Local images:** Drag images into `raw/assets/` so I can see them

---

## 📋 Your First Week

**Day 1-2: Setup**
- ✅ Create 3-5 sources (articles, notes, or screenshots you find interesting)

**Day 3: First Ingest**
- ✅ Use Workflow 1 to process your first source
- ✅ Review the wiki pages I create
- ✅ Explore in Obsidian (follow links, use graph view)

**Day 4-5: Second Ingest**
- ✅ Add 2-3 more sources
- ✅ Let me process them
- ✅ Notice how connections start forming

**Day 6-7: Ask Questions**
- ✅ Use Workflow 2 to ask a synthesizing question
- ✅ Watch how I pull from multiple sources to answer
- ✅ See the synthesis page get filed back into the wiki

**Week 2+: Compound**
- Keep ingesting
- Ask questions
- Notice your wiki getting smarter and more connected
- Every entry builds on the last

---

## 🚨 Troubleshooting

**Q: Obsidian isn't showing my new pages**
- Try refreshing: Cmd+R
- Or close and reopen the tab

**Q: I don't see wikilinks working**
- Make sure the page name in `[[brackets]]` exactly matches the filename (without `.md`)
- Check CLAUDE.md for naming conventions

**Q: Graph view looks empty**
- It only shows pages that have links
- Once you ingest sources, I'll create pages with connections and the graph will populate

**Q: How do I push to GitHub?**
- You don't! I won't touch git. Make a commit manually whenever you want to save:
  ```
  cd brain-wiki
  git add -A
  git commit -m "Wiki updates after [topic] ingest"
  git push
  ```

---

## Next Steps

1. **Find 3 sources** (articles, notes, screenshots, PDFs)
2. **Save to `raw/`** (create subfolders as needed)
3. **Tell me to ingest** using the template in Workflow 1
4. **Explore in Obsidian** and come back with feedback

You're ready to start! 🚀

---

**Questions?** Ask in Copilot Chat and I'll help refine the workflow.
