---
description: Convert a generated Playtime packet into shareable formats — PDF, JPEG/PNG, or a 1080×1080 share card for texting Grandma. Optionally drafts the message to send.
argument-hint: "[kid-name] [YYYY-MM-DD] [--card | --pdf | --png | --jpg | --all] (defaults: most recent packet for the only kid, or asks; defaults to --all)"
---

You are turning a generated packet into things parents actually share. This is the **virality nudge** of Playtime — a parent who texts Grandma a share card is doing free marketing AND giving Grandma a real moment.

# 0. Read state

```
~/.playtime/profile.json
~/.playtime/history.json
```

Use `skills/playtime-share/SKILL.md` and its references throughout.

# 1. Resolve which packet

Parse `$ARGUMENTS`:

- **Empty + 1 kid**: latest packet for that kid
- **Empty + multiple kids**: ask via `AskUserQuestion` which kid (and offer "all of them" if the parent wants a card for each)
- **`<kid-name>`**: latest packet for that kid
- **`<kid-name> YYYY-MM-DD`**: that specific packet
- **`<YYYY-MM-DD>` only**: that day's packet for the only kid (or ask if multi-kid)

Look up the entry in `history.json` `packets[]`. Confirm the source HTML exists at `entry.filePath`.

If no packet is found, say: "I don't have a packet to share for that. Run `/generate-playtime` first."

# 2. Resolve which formats

Parse format flags:

- `--card` — share card only (1080×1080 PNG, no affiliate footer)
- `--pdf` — full-fidelity PDF (with affiliate footer)
- `--png` — full packet as PNG (no affiliate footer)
- `--jpg` — full packet as JPEG (no affiliate footer)
- `--all` (or no flag) — share card + PDF + PNG

If the user is going to text it, the share card alone is usually right. If they're going to email it or print it elsewhere, PDF. If they want the whole thing as one image, PNG.

When unsure, default to **share card + PDF** and offer "want the full image too?" at the end.

# 3. Compose + render

For each requested format:

## Share card

1. Pull from the packet's `history.json` entry:
   - `kidName` (resolve `kidId` against `profile.json`)
   - kid's age (compute from birthday, e.g. "4yr 0mo" or just "4")
   - `date` → render as "Wednesday, April 28"
   - `theme` → e.g. "Ocean Week"
   - `title` → split into a 2-line headline (intelligent break, NOT 50/50 — the visual rhythm matters; "Maya's Megalodon" / "Mystery" lands better than "Maya's" / "Megalodon Mystery")
   - `activityType` → pick a representative emoji (mazes 🌀, mad libs 📝, science 🧪, drawing 🎨, story 📖, craft ✂️, scavenger hunt 🔎, yoga 🧘, etc.)
   - A **caption** — generate fresh: a 1-sentence, warm description of what the kid did. Pull from feedback notes if any ("Asked to do it three times!"), otherwise infer from the activity title and theme. Keep it under 100 characters.
   - A theme tag (uppercase, short): `OCEAN WEEK · DAY 3` or `BIRTHDAY MORNING` or just `MORNING PACKET`
   - Background color — vary by theme (ocean → soft blue `#E8F4FA`, jungle → soft mint `#E6F5E0`, sunny → soft cream `#FFF8EC`, evening/cozy → soft peach `#FBE4D5`)

2. Read `skills/playtime-share/references/share-card-template.html`. Replace placeholders:
   - `{{KID_NAME}}` → first name
   - `{{KID_AGE}}` → e.g. "AGE 4"
   - `{{DATE_PRETTY}}` → "Wednesday\nApril 28"
   - `{{ACTIVITY_EMOJI}}` → the emoji
   - `{{HEADLINE_LINE_1}}` → first line of the title
   - `{{HEADLINE_LINE_2}}` → second line (the highlighted bit)
   - `{{CAPTION}}` → the warm one-liner
   - `{{THEME_TAG}}` → the uppercase theme tag
   - `{{BG_COLOR}}` → the theme color

3. Save composed HTML to `~/.playtime/shares/YYYY-MM-DD/<kid>-card.tmp.html`.

4. Render via headless Chrome (per `skills/playtime-share/references/rendering.md`) at 1080×1080.

5. Save final PNG to `~/.playtime/shares/YYYY-MM-DD/<kid>-card.png`. Delete the `.tmp.html`.

## PDF

1. Render the source packet HTML directly to PDF (no modifications — keep affiliate footer).
2. Save to `~/.playtime/shares/YYYY-MM-DD/<kid>-packet.pdf`.

## PNG / JPEG (full packet)

1. Make a temp copy of the source HTML.
2. Strip the affiliate footer (look for the `<!-- AFFILIATE-FOOTER:START -->` ... `<!-- AFFILIATE-FOOTER:END -->` markers).
3. Render at 2x DPI per `rendering.md`.
4. For JPEG, post-convert with `sips` or `convert`.
5. Save and clean up the temp file.

# 4. Draft a share message (if user asked)

If the user said "text this to Grandma" / "send to <person>":

1. Resolve the recipient against `household.familyMembers`.
2. Pull the matching draft from `skills/playtime-share/references/text-template.md`.
3. Substitute the kid name, the recipient nickname, and (if available) the rating phrase from yesterday's feedback.
4. Show the draft message + the file path so the parent can copy and attach in their own messaging app:

```
Here's a draft message you can paste into iMessage / WhatsApp / etc:

> Hi Grandma Joy — Maya did this morning packet today and asked to do it three times.
> Thought you'd love to see it. ❤️

Attach: ~/.playtime/shares/2026-04-28/maya-card.png
```

We don't auto-send. Parent retains control of the actual share.

# 5. Wrap-up

Always end with a warm summary:

```
✨ Ready to share — Maya's Wednesday morning packet:

📱 Share card (best for texting)
   ~/.playtime/shares/2026-04-28/maya-card.png
   1080×1080 · 220KB

📄 PDF (best for emailing or printing elsewhere)
   ~/.playtime/shares/2026-04-28/maya-packet.pdf

🖼️  Full packet as image
   ~/.playtime/shares/2026-04-28/maya-packet.png

Want me to:
- Open these in Finder
- Draft a message to a specific family member
- Generate cards for all the kids' packets at once
```

Offer ONE thing the user is most likely to do next, not a wall of options.

# Important

- **Affiliate footer rules** (per `references/formats.md`):
  - PDF → keep
  - PNG / JPEG → strip
  - Share card → never include
- **Don't auto-send to anyone.** Always hand the artifact to the parent + offer a draft message.
- **Don't generate a share card for a "didn't land" packet.** Skip the offer or warn the parent: "yesterday's didn't land — you sure you want to share it?"
- **First-time share warmth**: if this is the parent's first share ever (no prior `~/.playtime/shares/` directory), add a one-line note: "FYI — share cards are designed to feel like a peek, not a pitch. We never include affiliate links or sales messaging in shareable artifacts."
