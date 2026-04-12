# HarvardXR Keynote — Claude Code Instructions

## What This Is

Single-file HTML keynote: "10 Lessons from 10 Years" by Alex Coulombe, HXR 2026. Everything in `index.html`. No build process. Read `HANDOFF_PROMPT.md` for full architecture.

## Rules

- **Never split index.html.** Single-file design is intentional.
- **SECTIONS array** drives all content. Change data → slides update.
- **`\n`** in titles/bullets = line break. **`<br><br>`** in taglines = paragraph gap.
- **`img: 'MEDIA_CYCLER'`** needs a matching IIFE after the build loop.
- **Media**: store in `media/` subfolders, relative paths, resize to ≤2560px.
- **Videos >100MB**: transcode with ffmpeg before committing.
- **GIFs don't animate on canvas** — convert to MP4.
- **`slideSteps.set()`** must be in `setTimeout(fn, 0)`.
- **AudioContext is monkey-patched** — all instances auto-tracked, killed on slide change.
- **Commit after every change. Push frequently.** Update HANDOFF_PROMPT.md after significant changes.

## Key Patterns

```javascript
// Find a slide by case title
allSlides.find(s => s.querySelector?.('.case-title')?.textContent.includes('Title'))

// Media cycler
buildMediaCycler(slide, [{type:'image', src:'media/x.jpg'}, {type:'video', src:'media/x.mp4', loop:true}], {imageDuration:6000});

// Slide steps
setTimeout(()=>{ slideSteps.set(mySlide, {current:0, steps:[fn1, fn2]}); }, 0);

// Show playing indicator
window._showPlaying(5000); // "don't skip yet" for 5 seconds
```

## URL Params
- Default = view mode. `?edit` = full chrome. `?landscape` = orientation prompt.
- Hash `#N` = 0-indexed (settings=0, cover=1).

## Slide Counter
- Settings = "00", Cover = "01". Total excludes settings.
