---
name: playtime-print
description: HTML/CSS template and style guide for generating print-ready Playtime packets. Single-page or multi-page, color or B&W, US Letter or A4. Use this when rendering the final HTML file.
---

# Playtime print system

Every packet ends as a **single self-contained HTML file** the parent opens in a browser and Cmd+P's. No external fonts, no CDNs, no JS frameworks — just inline CSS and inline SVG. Should still print in 10 years.

## Output path

```
~/.playtime/packets/YYYY-MM-DD/<kid-id>.html
```

## Page setup

- Default: **US Letter** (8.5" × 11"). A4 if profile says so.
- Margins: **0.5"** all around.
- Orientation: portrait (default). Landscape only if the activity needs it (long maze, wide comic).
- One page per activity by default. Multi-page allowed for templates / cut-outs / longer content.

## Template

See `references/template.html` for the complete starter HTML. It includes:

1. `<head>` with:
   - `<meta charset="utf-8">`
   - `<meta name="viewport">` (so it looks decent on screen too)
   - `<title>` (kid name + date)
   - Inline `<style>` with:
     - `@page { size: letter; margin: 0.5in }` (or A4)
     - `@media print` rules: hide screen-only chrome, ensure backgrounds print
     - Print-friendly typography
     - Color-mode-aware classes
2. `<body>` with the Playtime layout

## Layout structure

```html
<body class="packet color-color">  <!-- or color-bw -->
  <header class="hello">
    <div class="greeting">Good morning, Maya!</div>
    <div class="meta">Tuesday · April 28 · ☀️</div>
  </header>

  <main class="activity">
    <h1 class="title">Maya's Megalodon Mystery</h1>
    <p class="instruction">Connect the dots from 1 to 20. Then color in the megalodon!</p>
    <div class="content">
      <!-- inline SVG, the activity content -->
    </div>
    <aside class="extras">
      <!-- 1-2 small bonus elements: a joke, a today's word, a tiny prompt -->
    </aside>
  </main>

  <footer class="grownup">
    <details>
      <summary>For grown-ups</summary>
      <p><strong>Materials:</strong> printer, crayons.</p>
      <p><strong>What this builds:</strong> number sequencing 1-20, fine motor, ocean vocabulary.</p>
      <p><strong>Framework note:</strong> [if any]</p>
      <p><strong>Extension:</strong> when she's done, ask her to tell you the megalodon's story. Write down her words.</p>
    </details>
    <p class="fineprint">✨ Brought to you by Playtime · dailyplaytime.com · 2026-04-28 · maya · activity #c01-2026-04-28</p>
  </footer>
</body>
```

## Style guide

### Typography

- **Body / instructions**: a warm serif (Georgia, Charter) or system handwriting-style (Bradley Hand, Marker Felt) — falls back gracefully via CSS font stacks. NO web fonts (offline-forever rule).
- **Title**: bold, larger, slightly playful — `font-size: 28-32px`, `font-weight: 700`
- **Greeting**: friendly, slightly handwritten feel — `font-size: 18-20px`
- **Body text**: `font-size: 14-16px` for early readers, `12-14px` for older
- **Fine print**: `font-size: 10px`, `color: #777`

### Color palette (color mode)

Use a **single warm-bright accent color per packet** + black text. Pick from a small palette to keep things charming:

- Sunshine: `#F4A261`
- Mango: `#E76F51`
- Sky: `#2A9D8F`
- Berry: `#A23E48`
- Forest: `#588157`
- Indigo: `#3D348B`
- Rose: `#E85D75`

Pick the accent based on theme/kid. Use it sparingly — title underline, accent lines, accent illustrations. Don't make it a full-color free-for-all.

### B&W mode

If `defaultColorMode: "bw"` (or user override):
- All ink = black
- Use `hatching`, `stipple`, `dotted`, `dashed` to differentiate elements that color would have differentiated
- Avoid color-coding any instructions ("color the red part" → "color the section marked R")
- Test mental B&W version of any color choice before generating

### SVG conventions

- All graphics are inline `<svg>` — never `<img>` (for offline + scalability)
- Use simple, charming line illustrations — kid-book aesthetic
- Stroke width: 2-3px
- Solid black for line art; accent color for fills (color mode only)
- Always include `viewBox` for scaling

### Print considerations

- `@media print` hides screen-only details (anchor cursors, etc.)
- `-webkit-print-color-adjust: exact; print-color-adjust: exact;` to force background colors to print
- Page breaks: `page-break-inside: avoid` on `.activity` and `.extras` so they don't split mid-block
- Hide `<details>` summary expansion arrow in print — print the parent footer expanded

### Greeting style

Date format: "Tuesday · April 28 · ☀️" with a weather emoji guess (sunny default; rainy on rainy days if known).

Greeting line uses kid's nickname if set, full name otherwise. Examples:
- "Good morning, Mimi!"
- "Hello, Theo!"
- "Up and at 'em, Sir Theo of the Castle!" (themed)

## Personalization checklist

Before saving, verify:

- [ ] Kid's name appears in header
- [ ] Kid's name or interest appears in title
- [ ] Date is correct
- [ ] Color mode honored
- [ ] Accent color chosen and applied
- [ ] All SVG inline; no external resources
- [ ] Margins set; @page rule present
- [ ] Print color-adjust set
- [ ] Grown-up footer present with framework note (if any)
- [ ] Activity ID stamped in fine print
- [ ] No emojis in `id` attributes or filenames

## Helper: inline SVG checks

When generating SVG:
- Open with `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 W H">` (set W/H to match your design)
- All text in `<text>` (no images of text)
- Group with `<g>` for related elements
- Avoid `filter`, `mask`, complex effects unless needed — keep printable

## Reference files

- `references/template.html` — the full HTML starter
- `references/color-vs-bw.md` — color mode design tips
- `references/svg-snippets.md` — common decorative motifs as inline SVG (waves, leaves, stars, sun, clouds, frame borders)
