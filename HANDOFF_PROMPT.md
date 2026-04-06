# Teammate Handoff Prompt

Copy-paste this into a fresh Claude Code session to continue working on the keynote.

---

I'm working on Alex Coulombe's interactive HTML keynote for his closing talk at HXR Harvard XR Conference on April 11, 2026. The talk is called "10 Lessons from 10 Years of Running an XR Enterprise Studio" — it covers Agile Lens from 2016–2025 with a bonus 2026 lesson.

**Repo:** https://github.com/ibrews/harvardxr-keynote
**Live:** https://ibrews.github.io/harvardxr-keynote/
**KB doc:** `~/knowledge/projects/harvardxr-keynote/README.md` — read this first for the full architecture, slide structure, and how to make edits.

The presentation is a single self-contained HTML file (`index.html`). All content is driven by a `SECTIONS` JavaScript config object at the top of the file — each section = one year (2016–2025) with 1-3 case study slides followed by a lesson slide. To change content, edit the config. To change visuals, edit the CSS. The engine at the bottom auto-generates all slides from the config.

Key features:
- Horizontal flowing slide transitions
- Case study slides (project + image + bullets) → lesson slide per year
- Monocle-shaped mini-map tracker (bottom-right) with a walking avatar
- Map finale slide that animates the full monocle journey with all 10 lessons
- Fist-pump avatar celebration at the center

Versioning: `/v1/` = original, `/v2/` = current. When you make a big change, create `/v3/` etc. and copy to root `index.html`. Always push to GitHub so Pages updates.

The Agile Lens KB (`~/knowledge/`) has detailed project files for every case study referenced in the talk — use those for accurate details. The portfolio site at https://website-ivory-omega-78.vercel.app/portfolio has project images.

**My task:** [describe what you want to change here]
