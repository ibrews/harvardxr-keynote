# HarvardXR Keynote — AI Agent Handoff Prompt

> **Last updated:** 2026-04-12 (post-talk, fully delivered)
> **Status:** Talk delivered April 11, 2026. Deck is live and shared publicly.

---

## What Is This?

Single-file HTML keynote: "10 Lessons from 10 Years of Running an XR Enterprise Studio" by Alex Coulombe, closing keynote at Harvard XR Conference 2026.

- **Repo:** https://github.com/ibrews/harvardxr-keynote
- **Live:** https://ibrews.github.io/harvardxr-keynote/
- **Local:** `/Users/alex/harvardxr-keynote/index.html`
- **Spawned:** [Spatial Deck](https://github.com/ibrews/spatial-deck) (open-source template extracted from this)

---

## Architecture

Single `index.html` (~5600 lines). No build process. Three.js via CDN for constellation map.

```
index.html
├── <script> SECTIONS config (10 years + BONUS)
├── <style> All CSS
├── HTML shell (#deck, #tracker, nav, slide grid)
└── <script> Main engine (slide gen, media cycler, navigation,
    avatar, map, annotation, move mode, settings, SFX, etc.)
```

## Key Conventions

### SECTIONS Array
- `{ year, accent, lesson: {title, tagline, short, tags}, cases: [{title, subtitle, img, bullets}] }`
- `\n` in titles/bullets → line breaks. `<br>` and `<br><br>` in taglines (double = para-gap).
- `img: 'MEDIA_CYCLER'` needs explicit IIFE. `img: 'path'` auto-wraps. `img: ''` = gradient.

### Slide Numbering
- Settings = slide 0 (counter "00"), Cover = slide 1, etc.
- URL hash `#N` = 0-indexed. `history.replaceState` writes `#current`.

### URL Sharing Modes
- Default = view mode (presentation mode, edit chrome hidden)
- `?edit` = edit mode (all chrome, even on mobile)
- `?view` = explicit view. `?landscape` = orientation prompt.

### Mobile
- Auto-detect via `(pointer:coarse)`. Auto-enter presentation mode.
- Tap = advance step/slide. Swipe = navigate. 👁 button toggles chrome.

### Navigation
- `goTo(n)` — central nav. Calls `_killAllSfx()` first, then `playWhoosh()`.
- `nextVisible(from, dir)` — skips hidden slides.
- Arrow substep toggle in Settings (default: On) — arrows step through animations.
- Autoplay checkbox ("AUTO") inline with nav arrows.
- Playing spinner shows when timed content is still running.

### SFX
- AudioContext monkey-patched — all instances auto-register.
- `_killAllSfx()` closes all active contexts on slide change.
- Key sounds: whoosh (slide change), bing (media cycle), flash SFX (beard), ta-da (recap).

### Media Cycler
- `buildMediaCycler(slideEl, items, opts)` — pixelated reveal, dynamic canvas aspect ratio.
- Per-item: `flipH`, `loop`. Options: `imageDuration`, `portrait`, `enterDur`, `revealDur`, `exitDur`.
- Single images stop after reveal. Single videos loop.
- Manual mode: Shift+arrows.

### Move Mode & Annotations
- Move (M): drag/scale/rotate + undo/redo. Z-order buttons (▲▲/▲/▼/▼▼).
- Annotations (A): click to annotate, position coordinates exported.
- Layout grid (G): 4×3 zones A1-C4. Clipboard auto-copy after drag.
- Text editing: double-click, Shift+Enter for line breaks, Enter/Backspace for bullets.

### Slide Steps
- `slideSteps.set(slide, { current:0, steps:[fn1, fn2] })` in `setTimeout(fn, 0)`.
- Recap slide: 12 steps (10 lesson drops + "But wait..." + walker hop).

### Special Slides
- **Intro-Alex**: avatar walk + UE5 power-up + tallies (3 steps)
- **Intro-Lens**: monocle orbit + countdown (5 steps)
- **Intermission**: beard evolution at 2020 boundary
- **Recap**: "That's it! 10 Lessons from 10 Years" with drop-in grid
- **Map**: constellation ring + voxel morph
- **Close**: pixel Alex + voxel Alex with particle bursts

### The 10 Lessons (Final)
1. VR Is Theater (2016) — Shed, Macbeth, Late Show
2. It's OK to Pivot (2017) — ArchViz & R&D
3. Inbound Is Tricky (2018) — Intel, Samsung, High Fidelity
4. Outside-In Is the Edge (2019) — JetBlue, Connective Corridor, Equus, Ghosted
5. Tools Are Strategy (2020) — OnboardXR, UE Bet, Xsens/Agony, BPI
6. Require a Deposit (2021) — Christmas Carol VR, The Orchard
7. Protect R&D Time (2022) — Waldorf, DGH, Four Seasons, Body of Mine, AI Dickens
8. Scope Creep (2023) — FSLA, La Pasion XR
9. 98% Is Enough (2024) — Royal Caribbean, Stage Presence × RSC, DBOX Holodeck
10. Build a Community (2025) — Christmas Carol 5 Seasons, Unreal NYC & HalcyonVR

**Bonus:** Every Three Years, Everything Changes. Ship Anyway.

---

*Keep this file updated after significant changes.*
