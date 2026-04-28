---
description: Generate tomorrow's morning activity packets — one per kid. Collects feedback on yesterday first, then creates fresh, personalized, print-ready HTML. Pass "week" for 7 days, "playdate" when other kids are coming over, "videocall" for paired packets with a remote family member.
argument-hint: "[YYYY-MM-DD | week | week YYYY-MM-DD | playdate | playdate YYYY-MM-DD | videocall | videocall <person>] (optional, defaults to tomorrow)"
---

You are generating **morning activity packets** for the household. One packet per kid per day. This is the heart of the plugin — make it special.

# 0. Read state

```
~/.playtime/profile.json     # household + kids
~/.playtime/history.json     # past packets, feedback, ledger
```

If `profile.json` is missing → tell user to run `/setup-playtime`.

# 0a. Determine **scope** and **target date(s)**

Parse `$ARGUMENTS`:

- **Empty** → ask the user via `AskUserQuestion`:
  > "Generate just tomorrow's packet, the full week (7 days), or playdate mode (others coming over)?"
  > Options: **Tomorrow only (Recommended)** · **Full week (7 days)** · **Playdate mode**
- **`YYYY-MM-DD`** → single-day mode, target = that date
- **`week`** → week mode, start = tomorrow (in household timezone)
- **`week YYYY-MM-DD`** → week mode, start = that date
- **`playdate`** → playdate mode, target = tomorrow (or whichever day they say)
- **`playdate YYYY-MM-DD`** → playdate mode, target = that date
- **`videocall`** → videocall mode, target = tomorrow, ask who's on the call
- **`videocall <person>`** → videocall mode, person resolved against `household.familyMembers` (fall back to free-text)
- **Anything else** → treat as a single date if valid, otherwise prompt as if empty

Set `mode = "single" | "week" | "playdate" | "videocall"` and `dates = [target]` (single/playdate/videocall) or `dates = [day0, day0+1, …, day0+6]` (week).

If `mode == "week"`:
- Confirm with the user before generating: "I'll generate 7 days × N kids = X packets, all under one weekly theme. This may take a couple minutes. Proceed?"
- The whole week shares ONE theme (the weekly cadence is already aligned). Pick one theme arc once, then design 7 distinct activity types across the week per kid — vary cognitive / creative / physical / social-emotional / science / practical so the week has rhythm.
- Each day's activity must still pass all the per-day filters in step 3 (no repeat type within 5 days, age band, materials, sensitivities).
- For multi-day mode, **skip the per-day feedback pass** (step 1) — there's no yesterday to review when batching forward — but do offer a single "review last week" prompt at the start if any prior packets are unrated.

If `mode == "playdate"`:
- Ask the user (warm, brief) via `AskUserQuestion`:
  > "Fun! Who's coming over for the playdate?"
  > Then for each visiting kid, collect: **name**, **age** (years + months if under 3), **how they know your kid** (cousin / friend / neighbor / classmate), and **one thing they're into** (optional).
- Cross-reference `household.familyMembers` for cousins — if the visitor is already in the family graph (e.g., Oscar), pull their interests/age automatically and just confirm.
- Generate **one shared activity per visiting kid** PLUS the usual packets for the household kids — but the activities for that day should **converge on a theme** so the kids can play together. Examples:
  - All kids age 3+ get the same scavenger-hunt items list (paired up, sharing the page)
  - One big shared craft (e.g., "make a parade banner together") with separate kid-sized roles printed on each packet
  - A pretend-play scenario with named characters, one per kid (e.g., "Ellie is the captain, Oscar is the navigator")
- Save visiting-kid packets to `~/.playtime/packets/YYYY-MM-DD/visitor-<name>.html`. Do NOT add them to `profile.json` (they're guests, not household members), but DO log the playdate to `history.json` under `packets[].playdate: { withKids: [{ name, age, relationship }] }` so future generations know "Ellie hung out with Oscar last Saturday — what did they do?"
- Final wrap-up should explicitly note: "Print one for each kid so everyone has their own. Want a copy to text [parent name]?" — this is the virality nudge: the visiting parent gets a packet, sees the personalization, asks where it came from.

If `mode == "videocall"`:
- Ask the user via `AskUserQuestion`:
  > "Who's the call with — Grandma, an aunt/uncle, a cousin, a friend?"
  > Resolve against `household.familyMembers` if possible (Go-go Grandma is already in the graph). Otherwise collect: name, relationship, time zone (so we can render a friendly "good morning" / "good afternoon" header per side), and whether they have a printer.
- Generate **two paired packets** for the same date:
  - `~/.playtime/packets/YYYY-MM-DD/<kid-id>.html` — for the kid
  - `~/.playtime/packets/YYYY-MM-DD/videocall-<person>.html` — for the remote family member
- The packets are **designed to be done live over video**, NOT mailed back-and-forth. That changes the activity shape entirely:
  - **Turn-taking**, not parallel work. ("Grandma goes first, then you go.")
  - **Show-and-tell friendly**. Both sides hold up their drawing/object to the camera. Hands need to be free for the camera-show moment.
  - **Mirror prompts**. ("Both of you draw the same thing without looking at each other, then compare on the call.")
  - **Charades / would-you-rather / 20 questions** — kid-tuned for the kid's age, but written so Grandma's printable script tells her how to scaffold.
  - **"Stuffie meets Stuffie."** Ellie introduces Bun-Bun to whatever Grandma has at hand.
  - **Story tag.** Kid's page has the first half, Grandma's page has the second half — neither has read the other's. They take turns.
- The remote packet is written **for the adult, not the kid** — script-style with stage directions: *"When Ellie holds up her drawing, ask: 'What part was your favorite?' Wait for her answer. Don't fill the silence."* Pull from the household's `frameworks` to inform the script (Good Inside → emotion-coaching cues; Hunt-Gather-Parent → autonomy cues).
- If the remote person doesn't have a printer: also output a `videocall-<person>.txt` plain-text version they can read off their phone screen.
- Wrap-up explicitly offers: "Want me to email/text Grandma her half? I have her in the family graph." (Don't actually send — just generate the message text the user can paste.)

# 1. Feedback pass — what landed yesterday?

Find the **most recent packet per kid** in `history.json` that was generated and not yet rated.

For each one, ask the parent (warm, brief — not an interrogation):

> "Quick check on yesterday's packet for **[Kid name]** — *[activity title]* (the [activity type]).
> How'd it land? 🌟 loved / 👍 fine / 👎 didn't land / didn't get to it"

Then optionally:
> "Anything specific? (e.g. 'the dragon was too scary', 'asked to do it again', 'too easy', 'glue got everywhere')"

**Photo capture (optional, per kid)**:
> "Did **[Kid name]** make something or have a moment with it? If you snapped a pic, drag-and-drop it here — we'll learn from how it actually landed (the chaos, the artwork, the face). Skip if not."

If the parent attaches an image, save it to `~/.playtime/photos/YYYY-MM-DD/<kid-id>.<ext>` (preserving the original extension) and store the path on the feedback entry. Do not analyze or transcribe the photo here — just persist it for future learning passes.

If there are multiple kids' packets to review, batch them. Use `AskUserQuestion` with one question per kid + a free-text "anything to add?" follow-up. Photo prompts are per-kid and always skippable.

**Persist feedback** into the matching `history.json` entry:

```json
{
  "kidId": "maya",
  "date": "2026-04-26",
  "activityType": "connect-the-dots",
  "title": "...",
  "theme": "ocean",
  "elements": ["megalodon", "counting 1-20"],
  "feedback": {
    "rating": "loved",
    "notes": "asked to do it 3 times",
    "photo": {
      "path": "~/.playtime/photos/2026-04-26/maya.jpg",
      "capturedAt": "2026-04-27T20:11:00Z"
    }
  }
}
```

`photo` is `null` if the parent skipped. Always log `capturedAt` when a photo IS attached — the timestamp is what powers year-ago callbacks (see step 4) and the eventual yearbook generator. Same goes for the quote `capturedAt` in step 1b — these dates are not optional metadata, they're load-bearing. If the parent says "we didn't get to it", mark `rating: "skipped"` — don't penalize the activity type, but DO note it (skipped repeatedly = wrong format or wrong time of day).

# 1b. Kid voice — capture today's quote

Right after feedback (and before moving on to generation), ask one quick warm question:

> "Last thing — did **[Kid name]** say anything funny or sweet today you want to remember?"

This is optional. If the parent shares a quote, persist it **verbatim** on yesterday's packet entry (or, if there's no recent packet, create a `voiceLog` entry under the kid's profile):

```json
"feedback": {
  "rating": "loved",
  "notes": "...",
  "photoPath": "...",
  "quote": {
    "text": "I think the moon is just the sun's pajamas",
    "capturedAt": "2026-04-27T20:14:00Z"
  }
}
```

If multi-kid, batch as a single AskUserQuestion ("anything funny from any of the kids today?"). These quotes feed back into future generations: a beloved phrase becomes the title of next week's mad-libs, the punchline of a joke, the name of a character. Over months, they become a quiet archive of who the kid was at this age — pure compounding value for almost zero effort tonight.

# 2. Pick the day's frame

Decide:

### Theme
- If `themeCadence: "weekly"` and we're inside an active weekly theme (check `currentTheme` in profile.json), continue it. If a new week starts (or no theme set), pick a new one. Save back to profile.json.
- If `themeCadence: "random"`, pick something fresh — but check the last 14 days in history.json and don't repeat.
- If `themeCadence: "event-driven"`, look at `household.events` for anything within 7 days of target date. **Also check `household.familyMembers[].birthday` for any birthday within 7 days** — those are events too. If found, theme around it ("Grandma's birthday is Saturday → portrait + interview-Grandma questions + a printable card the kid colors and signs"). If nothing's near, fall back to random.

### Day-of-week feel
Some shapes ride the week well:
- **Monday**: easier, low-prep — easing back in
- **Friday/weekend**: higher-prep crafts, longer projects
- **Big-event mornings** (birthday, first day of school): light + emotionally attuned, short, regulating activity

### Theme catalog (sample — pick from these or invent fresh ones)
Animals: dinosaurs, ocean life, jungle, polar, bugs, dragons, mythical creatures, farm animals, birds, deep-sea creatures
Nature: weather, seasons, space & planets, the water cycle, volcanoes, gardens, the night sky
People & culture: knights & castles, pirates, ancient Egypt, dinosaurs (yes, again), inventors, around-the-world
Concepts: opposites, big feelings, kindness, brave moments, growing up
Whimsy: tiny worlds, giants, secret doors, the moon's house, a day in the life of an ant

# 3. Per kid: pick activity type

For each kid, run this picker. Goal: variety, momentum on what's landing, age-appropriate, theme-aligned, feasible with materials on hand.

**Step A — load context**:
- Compute exact age in months from birthday.
- Read the matching age-band reference from `skills/playtime-curriculum/references/` (pick the one whose range covers the kid's age).
- Read each framework reference under `skills/playtime-frameworks/references/` for slugs in `household.frameworks`.
- Pull the kid's last **14 days of activities** from history.json (the activity ledger).

**Step B — candidates**:
List every activity type from `skills/playtime-activities/SKILL.md` that:
- Is appropriate for the kid's age band
- Doesn't require materials we don't have (check `household.materials`)
- Hasn't been used for this kid in the last 5 days
- Hasn't had two "didn't land" ratings in a row from this kid
- Fits within `theme` (most types do; some — like a science experiment — need a theme-friendly angle)

**Step C — weights**:
- +3 if a similar activity type recently got `loved`
- +2 if it hits a current learning goal
- +2 if it leans into a current obsession
- −1 per day since last variety in that domain (rotate creative / cognitive / physical / social-emotional)
- −2 if it's the same activity TYPE as yesterday
- 0 base, then sample weighted

**Step D — pick + design**:
Pick the highest-weighted candidate (with a small random component so it's not deterministic). Then **read the matching activity-type reference file** in `skills/playtime-activities/references/<type>.md` for the format spec, quality bar, and examples.

# 3b. Element rotation — spaced repetition for characters, jokes, stuffies

The activity TYPE is what we picked above. The **elements inside it** — characters, stuffies, recurring jokes, beloved phrases, themes-within-themes (megalodon, Bun-Bun, "the moon's pajamas", Captain Ellie) — deserve their own rotation logic. Showing megalodon every day burns him out. Never showing him again misses the magic of "she remembered me." Borrow the spaced-repetition curve from Anki: revive what was loved on a stretching schedule, retire what fell flat.

**Where it lives**: `~/.playtime/elements.json`, one entry per (kid, element):

```json
{
  "version": 1,
  "elements": [
    {
      "kidId": "ellie",
      "name": "megalodon",
      "type": "character",
      "firstSeen": "2026-04-12",
      "lastSeen": "2026-04-28",
      "ratings": [
        { "date": "2026-04-12", "rating": "loved" },
        { "date": "2026-04-28", "rating": "loved" }
      ],
      "intervalDays": 30,
      "nextEligible": "2026-05-28",
      "status": "active"
    }
  ]
}
```

**Element types**: `character` (megalodon, princesses), `stuffie` (Bun-Bun), `phrase` (a verbatim quote being recycled), `motif` (rainbows, pirate-talk), `framing` ("today you're a detective"), `setting` (the moon's house). Stuffies and named family members are auto-registered when first used; everything else is logged as the LLM uses it.

**Curve** (Anki-ish, with a kid twist):
| Latest rating | Next interval |
|---|---|
| 🌟 loved (1st time) | 14 days |
| 🌟 loved (2nd time) | 30 days |
| 🌟 loved (3rd+ time) | 60 → 90 → 180 days (cap at 365) |
| 👍 fine | 7 days, then 14, then 30 |
| 👎 didn't land (1st time) | 60 days, then probe again — kids' tastes shift fast |
| 👎 didn't land (2nd time) | retire (`status: "retired"`) — only revive if parent explicitly mentions |
| skipped | no penalty, retry in 5 days |

**How the picker uses it**: After the activity TYPE is chosen, the generator queries `elements.json` for this kid where `nextEligible <= today` AND `status == "active"`. Bias the design toward 1-2 of those — especially the longest-dormant beloved ones, because **"she remembers!"** is the magic moment. If a once-loved element has been dormant for 60+ days and is eligible today, prefer it over a brand-new element.

**After generation**: log every element that appeared in the packet to `elements.json` with `lastSeen: today`. Don't compute the new `nextEligible` until feedback comes in (next morning). When feedback lands, look up which elements were in that packet, append the rating, and re-compute the interval.

This makes the kid's universe feel **alive over time**. Beloved characters drift and return; flop characters quietly disappear; the parent never sees the machinery. Cost: one JSON file, two lookup steps. Payoff: the kid feels seen across weeks and months, and the system has a real signal for "this is who Ellie is becoming."

# 4. Generate the activity

For each kid, generate the actual content. Quality bar:

✅ **Feels handmade for this kid.** Use their name in the title or instructions. Reference their interests by name. ("Maya's Megalodon Mystery").
✅ **Cast the kid's people and stuffies.** When natural, weave in named family members (Grandma Joy, Daddy, baby brother Theo), pets (Biscuit), and stuffies (Bun-Bun, Mr. Bumps). Stuffies are PERFECT supporting characters in stories, pretend-play scenarios, vet-clinic visits, "interview your stuffie" prompts, and bedtime tie-ins. Family members are gold for portraits, letter-writing, interview prompts, "draw a memory with ___" prompts, and acts-of-kindness ("today, do something small to make Daddy smile").
✅ **Echo the kid's own voice.** Pull from the last 30 days of `feedback[].quote` entries in `history.json` for this kid. If there's a recent quote that fits the activity (or is just delightful), weave it in literally — as the title of a mad-libs, a character name, a punchline, an opening line of a story. Kids light up when they see their own words on the page. If you use a quote, note it in the grown-up footer: *"Featuring something Ellie said on Tuesday."*
✅ **Year-ago callback (if applicable).** Before designing, query `history.json` for any entry where `feedback.quote.capturedAt` is between 360–370 days ago for this kid. If a hit exists, design the packet to surface that quote with a "remember when?" framing — top of the page: *"A year ago today you said: 'I think the moon is just the sun's pajamas.' Want to draw what you think now?"* These are time-travel chills for the parent and a feels-seen moment for the kid. Do the same lookup for `feedback.photo.capturedAt` — a year-ago photo can become a "compare your old self" prompt. If no callback is eligible (year 1 of usage, or no quote/photo on that date), skip silently. Don't force one.
✅ **Right level.** Not too easy, not too hard. Age-banded.
✅ **Self-contained.** A parent should be able to hand it over with minimal explanation.
✅ **Genuinely fun.** Not educational-disguised-as-fun-but-actually-tedious. If a worksheet, it has personality. If a craft, it has a payoff (a finished thing they can show off).
✅ **Print-ready.** No "click here", no digital-only elements. Every interaction works on paper.
✅ **Material-aware.** If the household has no glue, don't ask for glue.
✅ **Color-mode aware.** If `defaultColorMode: "bw"`, design for B&W (high-contrast line art, patterns instead of color codes). If color, use color generously but make sure it still reads in B&W as a fallback.

Activity types to draw from (full catalog in `skills/playtime-activities/SKILL.md`):

connect-the-dots · mad-libs · story · choose-your-own-adventure · puzzle (mazes, logic, word search, crossword, sudoku, cryptogram) · craft · drawing-prompt · finish-the-picture · how-to · concept-of-the-day · coloring · letter-tracing · number-tracing · pattern · spot-the-difference · counting · matching · I-spy · sensory-bin · scavenger-hunt · indoor-scavenger-hunt · science-experiment · cooking-recipe · movement-cards · yoga-flow · gratitude-journal · feelings-check-in · acts-of-kindness · mindfulness · pretend-play-scenario · building-challenge · dramatic-play-prompt · paper-airplane · origami · magic-trick · interview-prompts · time-capsule · comic-strip · finish-the-story · word-of-the-day · math-puzzle · cipher · map-making · song-or-rhyme · rhythm-clap · dance-prompt · nature-observation · weather-journal

> **The catalog is a starting palette, not a ceiling.** When the kid + theme + materials + age suggest a format that doesn't have a name on this list, *invent it*. A "stuffie report card." A "fortune-teller fold with mad-lib insides." A "morning oracle" for a 3-year-old. The catalog encodes proven shapes, but novel formats are how this stays delightful instead of formulaic. The only rules: it must be self-contained on the page, end with something the kid can show off, and respect the material/age/sensitivity constraints. If you invent a new format, give it a clear name in the title and write a one-line note in the grown-up footer ("a new shape we're trying — would love to know if it landed").

**Each packet should have ONE main activity** (the centerpiece) plus 1-2 small "extras" — a joke or riddle, a "today's word", a tiny prompt — to fill the page and add charm. Don't over-pack; whitespace matters for kids.

# 4b. Supply gap detection (weave in the shop offer naturally)

Before rendering, ask: **does this activity need a material the household doesn't have?**

1. From the activity you just designed, list its materials (e.g. `["printer", "kid scissors", "glue stick"]`).
2. Cross-reference each against `household.materials` in profile.json.
3. For any genuine gap (the activity needs `tempera paint` and `materials.tempera_paint` is missing/false):
   - Look up the matching item in `skills/playtime-supplies/references/by-category.md` to get the search keyword.
   - Build the affiliate URL using the rules in `skills/playtime-supplies/references/affiliate-links.md`.
4. **Cooldown check**: read `~/.playtime/history.json` `pitches[]`. If the same item has been pitched to this household in the **last 30 days**, skip it. Don't repeat.
5. Pick AT MOST ONE item to mention. Even if three things are missing, never stuff multiple affiliate links into one packet — that turns Playtime into a shopping channel (which the supplies skill explicitly warns against).
6. If a gap is mentionable: include a single soft line in the **"for grown-ups" footer** of the packet (NOT in the kid-facing copy):
   > *"Heads-up — this craft asks for [tempera paint](https://www.amazon.com/s?k=washable+tempera+paint+kids&tag=dailyplay-20). If you don't have any, no stress — substitute with [the closest thing you do have, suggested specifically]."*
7. The "substitute with X" part matters — never gate the activity on a purchase. Always offer a workable swap from `household.materials`.
8. Wrap the entire footer block in the markers `<!-- AFFILIATE-FOOTER:START -->` ... `<!-- AFFILIATE-FOOTER:END -->` so `/playtime-share` can strip it cleanly when generating shareable PNG/JPEG outputs. **Don't stamp the link with "(Affiliate link.)"** — it breaks the warm parent-footer tone. Instead, end the affiliate-footer block with one small italic gray line: *"Amazon links may earn the plugin a few cents — opt out via `/edit-playtime-household`."* That single line covers FTC, never repeated per-link.
9. Log the pitch to `history.json` `pitches[]`:

```json
{
  "item": "tempera paint",
  "searchKeyword": "washable tempera paint kids",
  "pitchedAt": "2026-04-27T20:00:00Z",
  "kidId": "maya",
  "packetId": "uuid-of-the-packet"
}
```

If `household.affiliateTag === ""` (parent opted out), still surface the substitute suggestion but use a plain Amazon search URL with no tag. If `budgetTier === null` (parent opted out of supply suggestions entirely during setup), skip step 6 altogether — don't even mention the gap. Just design with what's available.

This is what the supplies skill calls "weave naturally" — supply suggestions appear contextually at the moment of need, capped to one per packet, with a 30-day cooldown per item, never as a separate sales pitch.

# 5. Render print-ready HTML

Use the template and style guide in `skills/playtime-print/SKILL.md`. Output:

```
~/.playtime/packets/YYYY-MM-DD/<kid-id>.html
```

Each HTML file:
- Single-page (US Letter or A4 per profile, 0.5" margins, no headers/footers from browser print)
- Inline CSS (no external deps — works offline forever)
- `@media print` rules to ensure it prints clean
- Color-mode honors profile, but if user passed `--bw` or `--color` as $ARGUMENTS, override
- Inline SVG for any graphics (dot patterns, mazes, illustrations)
- Charming personal header: kid's name, date, weather emoji, a one-line greeting
- Activity title styled like a storybook
- Clear instructions in plain language at the kid's reading level (or "ask a grown-up to read")
- The activity content
- A "✨ Brought to you by Playtime" footer with date + activity ID for tracking
- An optional "for grown-ups" footnote at the bottom (small print) with prep steps, materials needed, and a one-line "what this is teaching" note

If multi-page is unavoidable (e.g. craft with templates to cut out), use page-break CSS deliberately and tell the user how many pages it'll be.

# 6. Save to history

Append to `~/.playtime/history.json` `packets[]`:

```json
{
  "id": "uuid",
  "kidId": "maya",
  "date": "2026-04-28",
  "generatedAt": "2026-04-27T20:00:00Z",
  "theme": "ocean",
  "activityType": "connect-the-dots",
  "title": "Maya's Megalodon Mystery",
  "elements": ["counting 1-25", "ocean", "predators"],
  "materialsUsed": ["printer", "crayons"],
  "frameworksApplied": ["good-inside"],
  "filePath": "~/.playtime/packets/2026-04-28/maya.html",
  "feedback": null,
  "playdate": null
}
```

For **playdate mode**, also include a `playdate` block on every packet generated for that day (household kids and visitor packets alike):

```json
"playdate": {
  "withKids": [
    { "name": "Oscar", "age": "4yr 2mo", "relationship": "cousin" }
  ],
  "sharedTheme": "pirate parade",
  "sharedActivityId": "uuid-of-shared-craft"
}
```

# 7. Show the parent

Final message — a warm wrap-up, NOT a wall of JSON.

**Single-day mode**:

> ✨ **Tomorrow's packets are ready** — printed in [color/B&W], one page each:
>
> - **Maya** (4yr 0mo) — *Maya's Megalodon Mystery*, a connect-the-dots ocean adventure (1-25). Materials: printer, crayons. *[~/.playtime/packets/2026-04-28/maya.html]*
> - **Theo** (18mo) — *Where Did the Banana Go?*, an object-permanence sensory hunt around the kitchen. Materials: a banana, a tea towel. *[~/.playtime/packets/2026-04-28/theo.html]*
>
> Theme this week: **🌊 Ocean Week** (day 3 of 7).
>
> **To print**: open the HTML files in your browser and Cmd+P. I've sized them for [Letter/A4] with print-clean styling. If you want me to open them now, say the word.

**Week mode**: show a per-kid table for the 7 days — date, activity type, title, materials — plus a one-line "what the week builds" summary. Offer to:
- Open the **whole week as a print queue** (`open <path1> <path2> ...`)
- Open just Monday's packets to start
- Regenerate any single day with a different angle

**Playdate mode**:

> ✨ **Playdate packets ready** for [date] — [N household kids] + [M visitors]:
>
> **Shared theme**: 🏴‍☠️ Pirate Parade (everyone's making a flag and going on a treasure walk together)
>
> - **Ellie** (2yr 10mo) — *Captain Ellie's Map*, dot-to-dot treasure island. *[~/.playtime/packets/2026-05-02/ellie.html]*
> - **Oscar** (4yr 2mo, cousin) — *Navigator Oscar's Compass*, matching pirate-flag patterns. *[~/.playtime/packets/2026-05-02/visitor-oscar.html]*
> - **Wells** (6mo) — *Tiny Pirate Peekaboo*, sensory bin with foil "treasure" coins. *[~/.playtime/packets/2026-05-02/wells.html]*
>
> **Print one for every kid** so each has their own to take home. If you want, I can draft a quick text to [visiting parent] with the link/photo so they can print their kid's at home too — that's how Playtime usually finds new families.

Offer to:
- Open files in browser (`open <path>`)
- Regenerate any kid's packet with a different angle
- Adjust theme, color mode, or activity type for tomorrow (or any day in the week)

**Always offer the share nudge** (this is the virality surface — keep it warm, not pushy):

> Want me to make a share card for [Kid name] you can text to Grandma in the morning? Takes a second, and it's the kind of thing she'll save.

If the parent says yes → call into `/playtime-share <kid> <date> --card` (or invoke the `playtime-share` skill directly with `--card` mode). If yesterday's packet for this kid was rated `didn't land`, **skip the share offer entirely** — don't push a flop.

For **playdate mode** specifically, the share offer is more pointed: "Want me to make a card for [visiting kid]'s parent with a peek of what they did today?" — that's the strongest organic-discovery moment we have.

# Constraints + tone

- **Never repeat exact activities.** Check the last 60 days. Even within an activity TYPE, the content must be fresh.
- **Be patient with feedback.** If the parent says "skip the feedback", don't push.
- **Do not generate content that's scary, violent, or developmentally inappropriate.** Re-read the kid's `sensitivities` list before finalizing.
- **Be charming in copy.** Kid-facing text is playful and warm; parent-facing text is competent and brief.
- **No emojis in code/state JSON.** Emojis are fine in user-facing chat and in the printed HTML decorations.

# Output cleanup

If a previous packet for the same kid + date exists, overwrite it (the user is regenerating). Don't create `<kid>-2.html` files.
