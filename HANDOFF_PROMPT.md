# Teammate Handoff Prompt

Copy-paste the section below into a fresh Claude Code session to continue working on the keynote.

---

## The Prompt

I'm working on Alex Coulombe's interactive HTML keynote for his closing talk at HXR Harvard XR Conference on April 11, 2026. The talk is called "10 Lessons from 10 Years of Running an XR Enterprise Studio" — it covers Agile Lens from 2016–2025 with a bonus 2026 lesson.

**Repo:** https://github.com/ibrews/harvardxr-keynote
**Live:** https://ibrews.github.io/harvardxr-keynote/
**KB doc:** `~/knowledge/projects/harvardxr-keynote/README.md` — read this first for the full architecture, slide structure, and how to make edits.

The presentation is a single self-contained HTML file (`index.html`). All content is driven by a `SECTIONS` JavaScript config object at the top of the file — each section = one year (2016–2025) with 1-3 case study slides followed by a lesson slide. To change content, edit the config. To change visuals, edit the CSS. The engine at the bottom auto-generates all slides from the config.

Key features:
- Horizontal flowing slide transitions (CSS @keyframes)
- Case study slides (project + image + bullets) → lesson slide per year
- Monocle-shaped mini-map tracker (bottom-right) with a walking avatar
- Map finale slide that animates the full monocle journey with all 10 lessons
- Fist-pump avatar celebration at the center

Versioning: `/v1/` = original, `/v2/` = current. When you make a big change, create `/v3/` etc. and copy to root `index.html`. Always push to GitHub so Pages updates.

The Agile Lens KB (`~/knowledge/`) has detailed project files for every case study referenced in the talk — use those for accurate details. The portfolio site at https://website-ivory-omega-78.vercel.app/portfolio has project images.

**Workflow for edits:**
1. Edit `/Users/alex/harvardxr-keynote/v2/index.html` (or whichever is current)
2. Copy to root: `cp v2/index.html index.html`
3. Copy to preview path: `cp index.html /Users/Shared/Documents/xcodeproj/harvardxr-keynote.html`
4. Commit and push: `git add -A && git commit -m "..." && git push`
5. GH Pages auto-deploys (~1 min)

**My task:** [describe what you want to change here]

---

## Setting Up Agentation (For Visual Annotation Workflow)

Agentation lets your teammates click on elements in the live keynote and leave structured annotations that Claude Code can then act on. This makes design feedback much faster than describing changes in plain English.

### One-time Install (per machine)

Run this in a terminal:

```bash
npx -y agentation-mcp init
```

Walk through the wizard:
1. **"Set up MCP server integration?"** → press `Y`
2. **"HTTP server port [4747]:"** → press Enter to accept the default
3. **"Start server and test connection?"** → press `Y`

The wizard will:
- Register Agentation as an MCP server in your Claude Code config (`~/.claude.json`)
- Start the local HTTP server on port 4747
- Confirm the connection

### Activate in Claude Code

After install, **fully restart Claude Code** so it loads the new MCP server. You should then have access to tools like:
- `agentation_get_all_pending` — fetch open annotations
- `agentation_list_sessions` — list active annotation sessions
- `agentation_resolve` — mark annotations as resolved

### How to Use With the Keynote

1. Open the live keynote: https://ibrews.github.io/harvardxr-keynote/
2. The Agentation toolbar appears in the bottom-right of the page
3. Click any element (a slide title, image, button, etc.) and add a note ("make this bigger", "swap this image", "punchier wording")
4. In Claude Code, ask: *"Check Agentation for pending annotations and apply them"*
5. Claude reads the annotations via MCP, makes the edits, then resolves them

### Verify Setup

```bash
npx -y agentation-mcp doctor
```

This confirms the server is running and reachable.

### Common Issues

- **Tools not showing in Claude Code:** Restart Claude Code completely (quit + reopen, not just reload)
- **Port 4747 already in use:** Re-run `init` and pick a different port (e.g. 4748)
- **Server not found:** Check `~/.claude.json` for an `agentation` entry under `mcpServers`

---

## Quick Reference

| What | Where |
|---|---|
| Live keynote | https://ibrews.github.io/harvardxr-keynote/ |
| GitHub repo | https://github.com/ibrews/harvardxr-keynote |
| Local file (canonical) | `/Users/alex/harvardxr-keynote/v2/index.html` |
| KB doc | `~/knowledge/projects/harvardxr-keynote/README.md` |
| Agile Lens KB | `~/knowledge/projects/` |
| Portfolio for images | https://website-ivory-omega-78.vercel.app/portfolio |
