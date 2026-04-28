# Color vs. B&W design

## Default

Whatever `household.defaultColorMode` says (set during setup). The user can override per generation by passing `--bw` or `--color` to `/generate-playtime`.

## When color shines

- Younger kids (under 5) — color helps recognition
- Coloring pages — sometimes a few accent colors are pre-printed for context
- Theme illustrations — ocean blues, jungle greens
- Distinguishing groups in multi-set activities
- Charts, color-coded paths

## When B&W shines

- Coloring pages — the kid IS the color
- Connect-the-dots — black numbers and dots are clearest
- Mazes — high-contrast walls
- Mad libs — text-heavy
- Worksheets where the kid will fill in

## Design rules for B&W

1. **Don't reference colors in instructions.** "color the red part" → "color the section marked with stripes". Use textures, patterns, or labels (R/G/B) — not hue.

2. **Use texture for differentiation.** Hatching, stippling, dotted, dashed, solid — rich vocabulary in B&W.

3. **Use weight for hierarchy.** Bold for important things; thinner for context.

4. **Test mentally before generating.** "Would this still work if my printer ran out of cyan?"

5. **Borders and frames help.** B&W can feel sparse; charming line borders give warmth.

## Design rules for color

1. **One accent color per packet.** Pick from the palette in `SKILL.md`. Don't make it a Skittles bag.

2. **Use color sparingly.** Title underline, accent strokes, an illustrated motif. Most page = black on white.

3. **Test B&W fallback.** A color packet should still make sense if printed B&W (the parent's printer ran out — it happens).

4. **Don't rely on color to encode information.** If a maze has "follow the red path", also distinguish it some other way (thicker line, dashes vs. solid).

## Color palette

Pick one accent based on theme:

| Color | Hex | Pairs with |
|---|---|---|
| Sunshine | `#F4A261` | space, sun, summer, autumn |
| Mango | `#E76F51` | dragons, fire, warmth, wonder |
| Sky | `#2A9D8F` | ocean, water, animals, science |
| Berry | `#A23E48` | fairy tale, princesses, hearts, kindness |
| Forest | `#588157` | nature, dinosaurs (esp. herbivores), bugs |
| Indigo | `#3D348B` | space, night, magic, mystery |
| Rose | `#E85D75` | birthdays, friendship, love, joy |
| Slate | `#2B2D42` | knights, detective, history (also good as B&W substitute) |

## Setting accent in the template

```html
<body>
  <main class="packet color-color"> <!-- "color-color" or "color-bw" -->
```

In `<style>`:
```css
.packet.color-color { --accent: #2A9D8F; }
```

Then anywhere in CSS use `var(--accent)`.

## Print color adjust

Always include in CSS:

```css
body { -webkit-print-color-adjust: exact; print-color-adjust: exact; }
```

Without this, browsers strip background colors during print to save ink. We want the accent stripes, dashed borders, and any backgrounds to actually print.
