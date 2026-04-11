# Keynote Handoff Prompt

Copy-paste the section below into a fresh Claude Code session to continue working on the keynote.

---

## The Prompt

I'm working on Alex Coulombe's interactive HTML keynote for his closing talk at HXR Harvard XR Conference on April 11, 2026. The talk is called **"10 Lessons from 10 Years of Running an XR Enterprise Studio"** — it covers Agile Lens from 2016–2025 with a bonus 2026 lesson.

- **Repo:** https://github.com/ibrews/harvardxr-keynote
- **Live:** https://ibrews.github.io/harvardxr-keynote/
- **Canonical local file:** `/Users/alex/harvardxr-keynote/index.html`
- **Media assets:** `/Users/alex/harvardxr-keynote/media/` (e.g. `media/shed/Shed_Axon.png`, `Shed_Video.mp4`, `Shed_Photo.jpg`)
- **Image assets in root:** `monocle-white.png`, `monocle-black.png`, `GoldAuthorized.png`, `ue5logo.png`
- **Portfolio images:** https://website-ivory-omega-78.vercel.app/portfolio

**Workflow for edits:**
1. Edit `/Users/alex/harvardxr-keynote/index.html` directly (it is the canonical file)
2. Preview locally in Chrome (just open the file — no server needed; hard-refresh after edits)
3. Commit and push: `git add index.html && git commit -m "..." && git push`
4. GitHub Pages auto-deploys in ~1 min

---

## Architecture Overview

**Single self-contained HTML file.** All logic, styles, content, and SFX are inline. No build step. No dependencies except Google Fonts (Space Grotesk) loaded from CDN.

### File Structure (top to bottom)

| Section | Lines (approx) | What it does |
|---|---|---|
| `<script>` SECTIONS config | ~22–147 | **Edit this to change content.** Defines all 10 years, case studies, lesson slides. |
| `<style>` CSS | ~150–580 | All visual styling. Slide layouts, animations, tracker, close slide, etc. |
| `<body>` HTML shell | ~580–590 | Minimal: `#deck`, `#tracker`, `#counter`, `#brand`, nav arrows |
| Main `<script>` | ~590–end | All JS: slide building, animation, audio, navigation, tracker |

### SECTIONS Config (the main content lever)

Each entry = one year (2016–2025):

```javascript
{
  year: 2016,
  accent: 'teal',          // teal | purple | amber | rose — drives all color accents for that year
  cases: [
    {
      title: 'The Shed @ Hudson Yards',
      subtitle: 'Our first major VR architecture project',
      img: 'MEDIA_CYCLER',  // 'MEDIA_CYCLER' triggers the cycling media player; '' = placeholder; URL = image
      bullets: ['...', '...', '...'],
    },
    { ... }
  ],
  lesson: {
    title: 'VR Is Theater.\nNot Film. Not Games.',  // \n = line break in the big heading
    tagline: 'Long explanation text...',
    tags: 'THE SHED · MACBETH · THE LATE SHOW',     // small tag line at bottom of lesson slide
    short: 'VR IS THEATER',   // used on case study eyebrow tag AND future use
  },
}
```

**To add/edit content:** change `title`, `subtitle`, `bullets`, `tagline`, `tags`, `short` in the config. To change images, replace `img` URLs or set to `''` for placeholder.

---

## Slide Order (runtime, after reorder annotations)

The JS bakes a REORDER at the bottom of the slide builder section that swaps slides [1] and [2] at runtime. Final deck order:

1. **Cover** — title card, speaker info
2. **Intro Alex** — walking avatar enters, tally counter, UE5 power-up
3. **Venn diagram** — avatar orbits 3 icons (XR + Theater + Tech), distinct pitched notes per icon
4. **Lens slide (2016)** — monocle close-up, avatar shrinks into eyepiece, chiptune journey to 2016
5–34. **Year slides** — for each of the 10 years: lesson slide first, then 1–2 case study slides
35. **Bonus 2026** — "The Demo Will Break. Budget for It."
36. **Map** — animated constellation journey across all 10 years
37. **Close** — "Let's Keep Building" / 4 QR codes / monocle backdrop

**Navigation:** arrow keys, on-screen buttons, or touch swipe. `goTo(n)` is the central nav function.

---

## Key Systems

### Web Audio API (all SFX — no audio files)

Every sound is synthesized with oscillators, noise buffers, and biquad filters. Key functions:

| Function | Sound | Where |
|---|---|---|
| `playWhoosh()` | Soft whoosh on every slide advance | `goTo()` |
| `playVennNote(idx)` | Pitched note (Am·G·F·E) when avatar reaches a Venn icon | Venn slide |
| `playTallyTick()` / `playTallyChing()` | Counter tick + landing ching | Intro Alex slide |
| `startLensHum()` / `stopLensHum()` | 3-oscillator hum while monocle "machine" runs | Lens slide |
| `play2016Sfx()` | Chiptune 12-note melody for the journey to 2016 | Lens slide step 4 |
| `playShrinkSfx()` | Descending SFX when avatar shrinks | Lens slide step 3 |
| `playLensSpawnSfx()` | Spawn SFX when avatar appears | Lens slide step 2 |
| `playBing()` (inside buildMediaCycler) | Soft low bell (330→440 Hz, 0.03 gain, 2.2s) when media cycles | Shed / any MEDIA_CYCLER slide |

### SVG Pixel-Art Avatar (`buildAvatar`)

`buildAvatar(parentSVGGroup, { withBeard, scale })` — appends a Stardew-style pixel-art figure. Local coords: roughly x ∈ [-18, 18], y ∈ [-43, +28] (head top to boots bottom). Used on: Venn slide, Intro slide, Lens slide, Map finale, Minimap tracker.

- `withBeard: false` = 2016–2019 Alex
- `withBeard: true` = 2020–2025 Alex
- The tracker triggers a Pokémon-style beard evolution animation when crossing 2020

### Reusable Media Cycler (`buildMediaCycler`)

```javascript
buildMediaCycler(slideEl, items, opts)
```

- `items`: `[{type:'image'|'video', src:'...'}]` — any mix of images and videos
- `opts`: `{ imageDuration: 6000, enterDur: 700, revealDur: 1600, exitDur: 500 }`
- Behavior: pixelated slide-in from right → de-pixelate (chunky → crisp) → flash → hold → soft bing → re-pixelate → fly left → next item
- Images hold for `imageDuration` ms; videos hold until the `ended` event (natural completion, no loop)
- Attach to any slide that has `img: 'MEDIA_CYCLER'` in SECTIONS — the slide builder renders a `<div class="media-cycler-mount">` which this function targets
- Currently wired up for: **The Shed @ Hudson Yards** (Axon → Video → Photo)

### Minimap Tracker (bottom-right)

- Uses `monocle-white.png` as the ring background, scaled/positioned using exact PNG geometry constants: `imgW=852, imgH=877, ringCX=407, ringCY=432, ringR=374`
- Walking avatar moves around the ring, one stop per year (10 positions via `tPos(i)`)
- Bonus year (2026) lands on the monocle's eyepiece
- Year label (`#tracker-year`) in teal appears above the SVG
- `updateTracker()` is called on every slide change

### Case Study Eyebrow Tag

Format: `CASE STUDY · [SHORT LESSON TITLE]` — the `short:` field from the SECTIONS config. Uses the year's accent color (`var(--teal)`, `--purple`, `--amber`, `--rose`).

### Close Slide

4 QR codes rendered via `api.qrserver.com`:
1. `agilelens.com` — amber badge "Newly upgraded!"
2. This presentation (`ibrews.github.io/harvardxr-keynote/`) — teal badge "Show your kids!"
3. Unreal NYC Discord (`discord.gg/qBqnPM5W7M`) — purple badge "Join the community!"
4. UE5 Training (`alexcoulombepresents.com`) — rose badge "Learn from Manhattan's best (& only!) authorized training center!"

Backdrop: `monocle-white.png` at 7% opacity, ring centered on the slide (130vmin wide).

### Favicon

Canvas-drawn sprite head of Alex at 64×64 (dark bg + pixel-art hair/face/beard), injected dynamically as a `<link rel="icon">` data URI. Code lives in a `<script>` tag just before `</head>`.

---

## Editing Cheat Sheet

| Task | What to change |
|---|---|
| Change slide content | `SECTIONS` config at top of file |
| Add a new case study year | Add entry to `SECTIONS[]` array |
| Add a MEDIA_CYCLER to another case | Set `img:'MEDIA_CYCLER'` in SECTIONS, add a `buildMediaCycler(...)` call after the shed one |
| Change lesson short title (eyebrow tag) | `short:` field in `lesson:` object |
| Change QR codes | `drawAllQR()` function near bottom of file |
| Change close slide text | `cls.innerHTML` string in the `// ── Close ──` section |
| Change tracker appearance | `buildTracker()` IIFE |
| Tweak any SFX | Find the matching `play...()` function and adjust oscillator frequency / gain values |
| Change accent colors | `accent:` field in SECTIONS → drives CSS via `.accent-purple`, `.accent-amber`, `.accent-rose` |

---

## Setting Up Agentation (Visual Annotation Workflow)

Agentation lets you click on elements in the live keynote and leave structured annotations that Claude Code can then act on.

```bash
npx -y agentation-mcp init
```

Walk through the wizard (accept defaults). Then fully restart Claude Code. You'll get tools like `agentation_get_all_pending` — use them to fetch and apply pending annotations.

---

## Quick Reference

| What | Where |
|---|---|
| Live keynote | https://ibrews.github.io/harvardxr-keynote/ |
| GitHub repo | https://github.com/ibrews/harvardxr-keynote |
| Local canonical file | `/Users/alex/harvardxr-keynote/index.html` |
| Media assets | `/Users/alex/harvardxr-keynote/media/` |
| Agile Lens KB | `~/knowledge/projects/` |
| Portfolio images | https://website-ivory-omega-78.vercel.app/portfolio |
