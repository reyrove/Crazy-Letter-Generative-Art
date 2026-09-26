# Crazy Letter

**A seed-based generative system for framed wavy-line compositions.**

A catalogue of computational textile compositions for fashion, textile and surface design — algorithmically drawn, seed-documented, and ready for production.

---

## Overview

Crazy Letter is a generative design system rather than a single artwork. Each composition is built from a stack of horizontal wavy lines, drawn inside a framed border. Each line has its own rhythm — its own slightly uneven spacing, its own gentle rise and fall. Together they read as a page of writing, as a texture, as a weather diagram of the hand.

The system is designed for:

- **Fashion houses** adapting handwriting-like ornament for apparel and accessories
- **Textile studios** developing repeat patterns and yardage
- **Surface designers** working across print, wallpaper, and interior applications

Every composition can be licensed, adapted, or commissioned to a brief.

---

## Concept

A line, when it is *repeated* across a page, becomes a hand — wavy, quiet, and yours.

The handwritten line — irregular, rhythmic, endlessly variable — has always carried the mark of a hand. From manuscript margins to the ruled pages of a ledger, the repeated stroke is one of the oldest forms of drawn computation we have. Crazy Letter translates that structure into code. Each composition begins with a frame and unfolds inward through a stack of wavy horizontal lines — each one drawn with its own step size, its own drift, its own rise and fall.

The palette, the line weight, the margins, and the density of the lines are all derived from a single numeric seed.

Like the other still volumes in this series (Girih, Arachne, Celestial Grove, ChaotiColor, Citrus Mosaic, Crazy Knight Curve, Crazy Knight Line), **Crazy Letter is a static composition.** The plate, the framed plate, the surfaces, and the archive are all static frames. A page of writing is something you read; its character is stillness, not motion.

---

## Features

- **Seed-based generation** — every composition is defined by a numeric seed and can be regenerated exactly
- **Deterministic output** — the same seed always produces the same composition
- **Framed composition** — a light rectangular frame always wraps the wavy field
- **Per-line rhythm** — each line has its own step size, drift amplitude, and spacing
- **56 background tones** — from pure white through pastels to soft aqua and cream
- **56 ink tones** — from black through deep sea greens, warm browns, and dark purples
- **Named colours in metadata** — the panel displays human-readable colour names, not hex values
- **Adaptive surfaces** — one seed applied across print, scarf, textile, and wall formats
- **Archive** — eight curated seeds available for immediate loading
- **Download** — export the composition as a high-resolution PNG
- **Keyboard shortcuts** — `R` for new seed, `S` to save

---

## Project Structure

```
.
├── index.html          # Main catalogue page
├── images/
│   ├── fav.svg         # Favicon
│   ├── tote.png        # Mockup: tote bag
│   ├── tee.png         # Mockup: t-shirt
│   └── cushion.png     # Mockup: cushion
└── README.md
```

---

## How It Works

### The Seed

A numeric seed (a large integer) initializes a deterministic pseudo-random generator. From this seed, the system derives:

- Background colour (from a palette of 56 light tones)
- Line colour (from a palette of 56 ink tones)
- Margin size
- Line weight
- Line glow (shadow blur)
- Line spacing
- Per-line drift amplitude
- Per-line step size

Because the generator is deterministic, the same seed always produces the same composition — on any device, at any time.

### The Frame

Every composition begins with a light rectangular frame. The margin is derived from the seed — between roughly 1% and 2% of the canvas width. The frame is stroked in the same ink colour as the lines, with the same soft glow.

### The Wavy Lines

Inside the frame, the system draws horizontal lines from top to bottom:

1. A line spacing value is chosen, between roughly 1% and 4% of the canvas width.
2. Starting at the top margin, the system draws one line, then moves down by the spacing.
3. Each line is drawn as a chain of small segments, stepping across the frame.
4. At every step, the y position drifts slightly up or down — within a range set by the seed.
5. The step size itself is also seed-derived, so some lines are dashed, some are long.

The result is a field of wavy horizontal lines, each one slightly different, together reading as a page of hand-drawn writing.

### The Palettes

Two colour systems meet in every composition:

- **Background colours** — 56 light tones, from pure white (`#FFFFFF`) through pastels, creams, and soft aqua to a warm orange-tinted coral.
- **Ink colours** — 56 dark tones, from pure black through deep sea greens, forest greens, warm browns, dark scarlets, and rich purples.

Each of these is named. The metadata panel displays the human-readable name — for example, "Cotton Candy" or "Midnight Blue" — instead of a hex value.

### The Surfaces

The same seed is rendered across four surface formats. These are static frames — they represent the print-ready composition.

| Surface  | Aspect | Material          |
|----------|--------|-------------------|
| Print    | 1 : 1  | Cotton rag        |
| Scarf    | 3 : 1  | Twill silk        |
| Textile  | 4 : 3  | Fabric yardage    |
| Wall     | 2 : 3  | Wallpaper         |

Each surface uses the same underlying seed and structural logic — only the repeat, orientation, and scale change.

### Stillness

Like the rest of the still volumes, Crazy Letter does not animate. The plate is a single frozen frame — the composition is complete the moment it is generated.

This is a deliberate design choice. A page of writing is not a swarm. It is not a rotation. It is a stack of strokes, drawn once and left. Its stillness is what makes it print-ready in the strictest sense: what you see is what you get.

---

## Usage

### In the browser

1. Open `index.html` in any modern browser.
2. Click **New Seed** to generate a new composition.
3. Click **Download** to save the composition as a PNG.
4. Scroll to the **Archive** section and click any plate to load it into Plate 001.

### Keyboard shortcuts

| Key | Action          |
|-----|-----------------|
| `R` | New seed        |
| `S` | Save as PNG     |

### Reproducing a composition

Each composition is identified by an 8-digit seed label displayed in the metadata panel. To reproduce a specific composition, note the seed and regenerate it programmatically:

```js
const rng = new RandomGenerator(seed);
const features = buildFeatures(rng);
renderComposition(canvas, features, rng);
```

Because the generator is deterministic, this will produce the identical composition on any device.

---

## Technical Notes

- **No build step.** The system is a single HTML file with inline CSS and JavaScript.
- **No dependencies.** All drawing is done with the native Canvas 2D API.
- **Deterministic.** The `RandomGenerator` class uses a xorshift-based PRNG seeded by an integer, so identical seeds produce identical outputs.
- **Static rendering.** Every canvas renders a single frame. There is no animation loop.
- **Feature isolation.** Cover, framed plate, surfaces, and archive thumbnails each derive their own feature set from their own local RNG, without disturbing the main plate's state.
- **Named colour metadata.** The two palettes are stored as parallel arrays of hex values and human-readable names, so the UI can display names without reverse-engineering them from RGB.
- **Soft glow.** The frame and lines share a subtle shadow blur, tied to the line weight, which gives the composition its soft, hand-drawn feel.
- **Responsive.** The layout adapts from large desktop down to very small mobile devices (tested at 360px viewport width).
- **Accessible.** Supports `prefers-reduced-motion`. Pinch-zoom is enabled.

### Browser support

Tested in current versions of:

- Chrome / Edge
- Firefox
- Safari (desktop and iOS)

---

## Licensing

All Crazy Letter compositions are **seed-documented** and available for licensing across textile, surface, and print applications.

- **Standard licenses** cover single-product production runs.
- **Commercial use, custom editions, or exclusive rights** are available on request.

Each license is issued against a specific seed ID. Regeneration of the same seed produces the identical composition — ensuring reproducibility between artist, studio, and manufacturer.

For licensing enquiries: [reyhanehdaneshdoost@gmail.com](mailto:reyhanehdaneshdoost@gmail.com)

---

## Commission

Crazy Letter is a generative design system, not a fixed artwork. It can be adapted for specific briefs:

| Service     | Description                                                       |
|-------------|-------------------------------------------------------------------|
| Licensing   | Existing seeds from the archive, licensed for production use      |
| Commission  | New compositions designed to your palette, repeat, and product    |
| Systems     | A private generative tool built for your studio's ongoing use     |

To begin a conversation: [reyhanehdaneshdoost@gmail.com](mailto:reyhanehdaneshdoost@gmail.com)

---

## Series

Crazy Letter is part of a computational textile series. Each volume approaches ornament from a different structural angle:

| Volume                | Structure                    | Motion                     |
|-----------------------|------------------------------|----------------------------|
| Girih 1               | Islamic geometric            | Static                     |
| Arachne               | Rotating rings               | Static                     |
| Baroque Me Baby       | Baroque frames               | Static                     |
| Bezier 1              | Concentric curves            | Static                     |
| Bezier 2              | Single rotating curve        | Animated (plate)           |
| Brownian Graphe       | Graph networks               | Animated + interactive     |
| Celestial Grove       | Recursive branch trees       | Static                     |
| ChaotiColor           | Cellular automata            | Static                     |
| Citrus Mosaic         | Arc-and-triangle tiles       | Static                     |
| Crazy Knight Curve    | Knight's-tour smooth path    | Static                     |
| Crazy Knight Line     | Knight's-tour gradient       | Static                     |
| **Crazy Letter**      | **Framed wavy lines**        | **Static**                 |

The series is designed as a coherent whole — same page structure, same seed logic, same licensing and commission terms — so that each volume can be presented individually or as part of a larger body of work.

---

## Credits

- **Design & Generative System** — Reyhaneh Daneshdoost
- **Typefaces** — Cormorant Garamond · DM Mono
- **Platform** — Reyrove Studio
- **Edition** — Crazy Letter, Autumn 2026

### On AI tools

Where technical obstacles were encountered, AI tools were used for debugging and code optimization. Every structural, aesthetic, and conceptual decision remained the artist's own.

---

## Links

- Website — [reyrove.github.io](https://reyrove.github.io/)
- Instagram — [@rey._.rove](https://www.instagram.com/rey._.rove/)
- LinkedIn — [Reyhaneh Daneshdoost](https://www.linkedin.com/in/reyhaneh-daneshdoost-730481160/)
- X — [@reyrove](https://x.com/reyrove)

---

© Crazy Letter · All compositions reproducible by seed · Computational Textile Design