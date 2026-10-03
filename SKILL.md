---
name: webos-app-assets
description: Generates and packages LG webOS TV app visual assets (icons, splash, tile color) for cre-cli game apps. Use when creating or updating app icons, splash screens, appinfo.json branding, LG Seller Lounge assets, or when the user asks for an app icon prompt for a TV game.
disable-model-invocation: true
---

# webOS App Assets

Generate branding assets for an LG webOS TV app. In a cre-cli project they live in the webOS app root, next to `appinfo.json` (e.g. `apps/lg-webos/`).

## Required assets

| Asset | Size | File | appinfo.json key |
|-------|------|------|------------------|
| Small icon | 80×80 PNG | `icon.png` | `icon` |
| Large icon | 130×130 PNG | `largeIcon.png` | `largeIcon` |
| Splash background | 1920×1080 PNG | `splashBackground.png` | `splashBackground` |
| App tile color | HEX | — | `iconColor` |
| App name | ≤ 20 characters | — | `title` |

**Rules:**
- Small and large icons must be identical except size.
- Same `iconColor` for both icon sizes (tile background on Home screen).
- Icons are flat and two-dimensional with a distinct silhouette: no gloss, bevel, drop shadow, or other visual effects.
- Icon background must be opaque (no transparency), with at least 5 px padding inside the 80×80 and 130×130 canvases.
- Splash must NOT be a black screen; minimal text (localization).
- `title` is the launcher title (≤ 20 characters), not the store title. A store title like `Brand – Descriptor` usually goes over 20; use just the brand (or a short form) in `appinfo.json` and keep the full store title for Seller Lounge.
- Seller Lounge also needs a **400×400 PNG** (`store-assets/icon-400.png`) — uploaded separately, auto-resized in store. The packaged 80/130 icons are only used for testing.
- Paths are relative to `index.html` (e.g. `icon.png`, not `/icon.png`).

## Workflow

```
Task Progress:
- [ ] Gather game name, visual theme, and palette from game.json / game UI
- [ ] Generate 400×400 master icon with image prompt below
- [ ] Resize to 80×80 and 130×130 (identical design)
- [ ] Generate 1920×1080 splash from same visual language
- [ ] Pick iconColor HEX from dominant icon background
- [ ] Update appinfo.json (title ≤20 chars, appDescription ≤60 chars optional)
- [ ] Place files in the webOS app root next to appinfo.json
```

## Icon generation prompt (copy and customize)

Replace bracketed placeholders before sending to an image generator:

```
Design a square app icon for a webOS TV game called "[GAME_NAME]".

Subject: [ONE_SENTENCE_GAME_CONCEPT — e.g. "classic falling-block puzzle with colorful tetrominoes"]

Style:
- Bold, simple, readable at small sizes (80px on TV)
- Flat, two-dimensional vector/game-art style with a distinct silhouette
- No gloss, bevel, drop shadows, or other visual effects
- No fine text, no UI chrome, no screenshots
- Centered symbol on a solid, opaque background (no transparency)
- High contrast; works on dark TV launcher tiles

Colors: [LIST 3–5 HEX VALUES from the game palette]

Composition:
- Single focal graphic (character, emblem, or 2–3 iconic game pieces)
- Safe padding ~12% on all sides
- Square 1:1 aspect ratio

Output: 400×400 px PNG, clean edges, opaque background.
```

### Splash prompt (same session, same style)

```
Design a 1920×1080 landscape splash/loading screen for the webOS TV game "[GAME_NAME]".

Same visual style and palette as the app icon: [HEX VALUES].

Layout:
- Dark but NOT pure black background (#0a0a12 to #1a1a2e range)
- Large centered game emblem or abstract motif from the icon
- Subtle ambient glow or soft grid; no busy detail
- No paragraph text; optional single-word title "[GAME_NAME]" only if it stays readable

Output: 1920×1080 PNG.
```

## Example: Spectrum Drop

**Prompt:**
```
Design a square app icon for a webOS TV game called "Spectrum Drop".

Subject: classic Tetris-style falling blocks with a modern neon spectrum palette.

Style: bold flat 2D game art, no visual effects, readable at 80px, centered tetromino cluster, no text.

Colors: #0a0a12 background, #fa1e1e red, #f1fa1e yellow, #42c6f0 cyan, #d838cb magenta, #4bd838 green.

Composition: 3–4 stacked colorful tetromino blocks forming a compact emblem, ~12% padding, opaque background, 1:1, 400×400 PNG.
```

**iconColor:** `#050509` (sampled from the generated icon's background, not copied from the prompt)

## After generation

1. Save master as `store-assets/icon-400.png`.
2. Resize:
   ```bash
   sips -z 80 80 store-assets/icon-400.png --out icon.png
   sips -z 130 130 store-assets/icon-400.png --out largeIcon.png
   ```
   Or use Python/Pillow if `sips` unavailable.
3. Update `appinfo.json`:
   ```json
   {
     "title": "Spectrum Drop",
     "icon": "icon.png",
     "largeIcon": "largeIcon.png",
     "splashBackground": "splashBackground.png",
     "iconColor": "#050509"
   }
   ```
4. Verify `title` ≤ 20 characters.

## Quick placeholder (no image AI)

When only a dev placeholder is needed, generate simple block-art icons with Pillow using the game's palette. Replace before store submission.
