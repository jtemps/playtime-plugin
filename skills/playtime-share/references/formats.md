# Share formats

## PDF — for archiving, emailing, printing later

- **When**: parent wants to keep it, send via email, print at a different printer (e.g. at Grandma's house)
- **Specs**: matches `household.printSize` (Letter or A4), full-color (or B&W if profile says so), 0.5" margins
- **Filename**: `~/.playtime/shares/YYYY-MM-DD/<kid>-packet.pdf`
- **Notes**: Includes the affiliate-link footer (parent-facing). Embed all fonts. Compress images.

## PNG — for texting, screenshots, social

- **When**: parent wants to drop the packet directly into iMessage / WhatsApp / Notes
- **Specs**: 2x DPI for retina (so 1700×2200 for a Letter page), PNG-24, transparent background NOT required (white is fine and smaller)
- **Filename**: `~/.playtime/shares/YYYY-MM-DD/<kid>-packet.png` (single page) or `~/.playtime/shares/YYYY-MM-DD/<kid>-packet-1.png`, `-2.png`, ... (multi-page)
- **Notes**: Strip affiliate-link footer (third-party recipient shouldn't see it).

## JPEG — for texting (lighter file)

- **When**: same use as PNG but the parent or recipient is on a slow connection or wants a smaller file
- **Specs**: same dimensions, JPEG quality 88, white background
- **Filename**: `~/.playtime/shares/YYYY-MM-DD/<kid>-packet.jpg`
- **Notes**: Same affiliate-stripping as PNG.

## Share card — the headline image

This is the distinctive format. A single 1080×1080 image (Instagram square) that says, at a glance:

- Who: kid name + age (e.g. "Maya, 4")
- What: a one-sentence headline pulled from the activity title and theme
- When: the date, in a soft, hand-lettered style ("Wednesday, April 28")
- A small visual element pulled from the packet (the activity emoji or a tiny illustration)
- "Made with 🎈 Playtime" in the bottom corner, small

**Why 1080×1080?** It works as a text-message preview, an Instagram post, an iMessage tapback target, a WhatsApp status update — square is the universal format that doesn't get cropped weirdly.

**When to generate**: Always offered after `/generate-playtime` finishes (one tap to text Grandma). Also generated explicitly via `/playtime-share <kid> [date] --card`.

**Filename**: `~/.playtime/shares/YYYY-MM-DD/<kid>-card.png`

**Strip everything sales-y.** No affiliate links. No "buy supplies" prompt. The share card is the kid + the moment, period.

## Quality bar by format

| Format | Fidelity | Recipient | Affiliate footer? |
|---|---|---|---|
| PDF | Highest (vector, print-quality) | Parent / family archive | ✅ Keep |
| PNG | High (2x raster) | Friend / grandparent via text | ❌ Strip |
| JPEG | Medium (compressed raster) | Friend / grandparent via text | ❌ Strip |
| Share card | Stylized headline | Anyone (mass-shareable) | ❌ Never |
