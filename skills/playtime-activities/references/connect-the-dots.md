# Connect the dots

## What it is

Numbered (or lettered) dots that, when connected in order, reveal a picture. The kid traces the path; the picture emerges.

## Age range

**3-10 years.** Tune the count and complexity to age:

| Age | Dot count | Numbering | Style |
|---|---|---|---|
| 3-4 yr | 5-10 | 1-10, big spacing | Single-line shape, no crossings |
| 4-5 yr | 10-20 | 1-15 or 1-20 | Single-line shape |
| 5-6 yr | 20-40 | 1-30 or A-Z | One main shape, some interior detail |
| 6-8 yr | 40-80 | 1-50, 1-100, or letter-then-number | Detailed scenes |
| 8-10 yr | 80-200 | Multi-path, color-coded segments | Complex art |

## Skills it builds

- Number/letter recognition + sequencing
- 1:1 correspondence
- Fine motor control
- Visual tracking
- Anticipation / inference (guessing the picture before it's done)

## Format spec

Generate as **inline SVG**. Required elements:

- A bounding box sized for printing (e.g. 7.5" × 9.5" inside Letter margins)
- Each dot is a `<circle>` with `r="6"` to `r="10"` depending on age
- Each dot has a `<text>` label nearby (offset 12-16px) — number or letter
- Dots are ordered such that drawing the path reveals a recognizable shape **of the kid's interest**
- A **light hint** of the final shape (very pale gray outlines or some pre-drawn elements) is OK for under-5; for older kids, surprise is the point
- Optional: 2-3 "decoy" details around the main shape (a sun, a wave, a tree) already drawn to add scene context

## Quality bar

- The picture is **personalized** to the kid's interests (a megalodon, a princess castle, a backhoe, a unicorn — not a generic teddy bear)
- The path **flows naturally** — no awkward criss-crosses, no impossible angles
- The reveal feels like a **delight** — the picture is recognizable + cool
- Numbers/letters are **legible** and unambiguous (don't put 6 next to 9 oriented confusingly)
- Final art looks **good colored OR uncolored** — works in B&W

## Variations

- **Letter sequence** (A-Z, alphabet path) for letter learners
- **Skip-counting** (2, 4, 6, 8...) for math practice
- **Multi-path** (color-coded: connect red 1-15, then blue 1-15) for older kids
- **Story-integrated** ("connect the dots to free the dragon!") — narrative wrapper
- **Fill-then-color** — connect first, color after

## Material requirements

Printer + a pencil/pen/crayon. That's it.

## Print considerations

- Solid black dots and labels (color mode "color" can use dark navy or kid's favorite color)
- B&W mode: ensure no color-coding ambiguity; numbers must read on plain paper
- Margin: 0.5" all around
- One activity per page

## Example brief (input → SVG output)

> Kid: Maya, 4yr 0mo
> Interests: ocean animals (esp. megalodon), counting
> Theme: ocean week
> Dots: 1-20

→ Output title: "Maya's Megalodon Mystery"
→ SVG of 20 dots that, when connected, form a stylized megalodon with a tail fin and a mouth full of triangle teeth. Pale wave lines pre-drawn around it. Big numbers (32px) next to dots. Dot radius 8px. A small "for grown-ups" footer at the bottom.

## Generation tip

Plan the picture path *first* (sketch the polygon outline of the target image as a list of points), then number the points in order. Use Bezier flow if you want curved final lines, but keep dot-to-dot as straight segments for simplicity.

For complex shapes: split into "outline 1-N" and "interior detail N+1-M" with a small visual cue to indicate "lift pencil here".
