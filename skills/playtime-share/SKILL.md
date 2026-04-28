---
name: playtime-share
description: Convert a generated Playtime packet into shareable formats — PDF for printing or emailing, JPEG/PNG for texting, and a "share card" summary image for grandparents. Used by /playtime-share and optionally offered automatically after /generate-playtime.
---

# Playtime share

Turns the print-ready HTML packet into formats parents actually share.

## What this is for

A packet is generated as `~/.playtime/packets/YYYY-MM-DD/<kid>.html`. That HTML is great for printing. It's terrible for:

- **Texting Grandma** — she's not opening an HTML file
- **Saving for the year-end annual** — needs a portable file format
- **Showing the visiting parent at a playdate** — needs to render on their phone

This skill produces three output shapes:

1. **PDF** — full-fidelity, prints anywhere, attaches to email, archives well
2. **JPEG/PNG** — the packet rendered as one or more images, drops into iMessage/WhatsApp/Photos
3. **Share card** — a single 1080×1080 (Instagram-ratio) image with kid name + activity title + a one-line caption, designed to text in two taps and signal "look what we did today"

## How to use

1. Read the latest entry (or specified entry) from `~/.playtime/history.json` for the requested kid + date.
2. Locate the source HTML at `entry.filePath`.
3. Pick the format(s) requested by the user (or default to "all three").
4. Render via `references/rendering.md` (headless Chrome is the primary path).
5. Save outputs to `~/.playtime/shares/YYYY-MM-DD/<kid>-<format>.<ext>`.
6. For share cards, compose a fresh HTML from `references/share-card-template.html` first, then render.
7. Offer the user a draft message for texting (`references/text-template.md`).

## Reference files

- `references/formats.md` — when to use each format, sizing/quality notes
- `references/rendering.md` — headless Chrome commands, fallbacks, troubleshooting
- `references/share-card-template.html` — the 1080×1080 summary card template (with placeholders)
- `references/text-template.md` — draft messages parents can paste when sharing

## Important principles

- **Never modify the source packet HTML.** Always render from a copy or compose a fresh artifact.
- **Respect the `printSize` and `defaultColorMode` from profile.json** — the PDF should look exactly like what would have come out of the printer.
- **Strip the affiliate-link footer from the share card and from JPEG outputs.** Affiliate links are for the parent, not for grandparents or social shares. PDFs (which the parent keeps) can retain the footer.
- **Share cards are for the kid + family, not for marketing.** Yes, "Made with Playtime" sits in the bottom corner — but it's small, it's not the headline, and we never put a CTA on a kid's artifact. The credit is just enough that someone curious can find us.
- **No PII in filenames** beyond the kid's first name (already in profile). No phone numbers, no email addresses in any output.
