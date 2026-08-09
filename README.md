# HarvardXR Keynote

**"10 Lessons from 10 Years of Running an XR Enterprise Studio"** — the closing keynote Alex Coulombe delivered at the Harvard XR Conference on April 11, 2026. A single-file interactive HTML presentation with animated pixel-art avatars, a live constellation map of all 10 lessons, media galleries, and Web Audio.

Live: **https://ibrews.github.io/harvardxr-keynote/**

Built on [Spatial Deck](https://github.com/ibrews/spatial-deck) — the open-source presentation framework extracted from this project.

## Quick Start

```bash
# No install — just open it
open index.html
```

Or visit the live version directly at [ibrews.github.io/harvardxr-keynote](https://ibrews.github.io/harvardxr-keynote/).

## Things to Try

1. **Open index.html (or the live URL) and press `→` or `Space`** — the deck advances through 10 years of XR studio lessons with pixelated image reveals and a swoosh transition.
2. **Navigate to the Constellation Map slide** — an animated ring of 10 glowing lesson-stars appears; click any node to jump directly to that lesson.
3. **Press `H` to toggle presentation chrome** — all UI disappears for a clean audience view; press `H` again to restore edit controls.
4. **Press `M` to enter Move Mode and drag any element** — the repositioned element's exact CSS coordinates auto-copy to your clipboard; drag to reposition, Shift+drag to scale.
5. **Press `N` to open the Presenter Popup** — a second window shows speaker notes, elapsed time, next-slide preview, and a live pacing indicator (green when on schedule, red when over).

## Tech

- Single `index.html` (~5600 lines) — no build process, works from `file://` or any static host
- Three.js (CDN) for the 3D constellation map
- Web Audio API for slide transitions and SFX
- Pixel-art avatars procedurally drawn on `<canvas>`

## License

All rights reserved. The framework is open-sourced separately as [Spatial Deck](https://github.com/ibrews/spatial-deck) (MIT).

## Support

If you like seeing this kind of thing get built and shared, [donations are always welcome](https://www.alexcoulombepresents.com/support) — they buy hardware, render time, and the freedom to keep giving most of this away.
