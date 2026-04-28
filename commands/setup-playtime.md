---
description: First-time setup for Playtime — create household profile, add kids, choose parenting frameworks, pick themes and print preferences.
---

You are running first-time setup for the **Playtime** plugin, which generates personalized morning activity packets for kids.

# Where state lives

All state lives at `~/.playtime/`. Create it if missing.

```
~/.playtime/
├── profile.json          # household + all kids
├── packets/              # generated packets, organized by date
│   └── YYYY-MM-DD/
│       └── <kid-name>.html
└── history.json          # feedback log + activity ledger
```

If `~/.playtime/profile.json` already exists, **stop and tell the user to run `/edit-kid-details` or `/add-kid-to-playtime` instead** — don't overwrite. Offer a one-line summary of what's there.

# Setup flow

Walk the user through this conversationally — warm and a little playful, not a clinical form. Ask in batches of 1-3 questions, not all at once. After each batch, confirm before moving on.

## 1. Household basics

- Family name or what to call this household (e.g. "The Tempelsmans")
- Time zone (best guess from system, confirm)
- Print size: **Letter** (8.5×11", US default) or **A4**
- Default color mode: **Color** or **Black & White** (they can override per generation)

## 2. Kids

For each kid, gather:

- Name (and a nickname if there is one — use it in greetings)
- Birthday (full date — we compute age in months, which matters a lot under 3yr)
- Pronouns (optional, default "they")
- Top interests (free text — dinosaurs, trucks, princesses, space, sharks, knights, ballet, etc.)
- Current obsessions (a "right now" list that decays — what's grabbing them THIS month)
- Things they're working on / learning goals (e.g. "letter sounds", "scissor skills", "regulating big feelings", "tying shoes")
- Sensitivities or things to avoid (loud sounds, scary themes, food allergies if cooking, anything that's hit a nerve)
- Skill levels:
  - Reading: pre-reader / sounding out / reading independently / fluent reader
  - Writing: scribbling / letters / words / sentences / paragraphs
  - Scissors: not yet / supervised / independent
- **Stuffies / loveys** (per kid) — names, what kind they are (bear, dog, dragon, blanket, doll), personality if any, anything special the family knows about them. These are gold for stories, pretend play, and bedtime. Examples:
  - "Bun-Bun" — a yellow bunny who is Theo's bedtime companion; very brave; loves carrots
  - "Mr. Bumps" — a stuffed elephant who is the family's worrier
- Anything else the parent wants the activity-generator to know

Ask "any more kids?" until they say no.

## 2b. Family members (household-wide cast)

After all kids are entered, gather **other people in the kid's world** — these get woven into stories, portraits, letter-writing prompts, interview activities, birthday-themed packets, and acts of kindness.

For each, collect:
- Name (or what the kids call them — "Grandma Joy", "Nana", "Papa", "Uncle Pete")
- Relation (parent, sibling[non-kid], grandparent, aunt/uncle, cousin, close-friend, pet, caregiver, etc.)
- Birthday (optional but valuable — fuels surprise birthday-themed packets)
- A few details that make them THEM (lives where, what they love, a quirk, what the kids notice about them)

Sweep through the obvious cast: both parents, grandparents on both sides, siblings older/younger than the kids, regular caregivers, and pets. Add cousins and close friends if the family wants. Don't make this exhaustive — the goal is the people who show up in the kids' lives most.

For pets: name, kind (dog/cat/fish/etc.), and a detail or two ("Biscuit, golden retriever, scared of the vacuum").

## 3. Materials on hand

What's available in the house? This bounds what we can ask for in crafts. Default-check the common ones, but let them adjust:

- Printer (color? B&W only?)
- Crayons / markers / colored pencils / paint
- Scissors (kid-safe + adult)
- Glue stick / liquid glue / tape
- Construction paper / cardstock / regular printer paper
- Cardboard boxes (saved for crafts?)
- Pipe cleaners, googly eyes, stickers, pom-poms
- Yarn / string
- Cotton balls
- Paper plates / cups
- Common kitchen science stuff (baking soda, vinegar, food coloring, salt, oil)
- Outdoor space (yard / balcony / park nearby)
- Recyclables they save (bottles, jars, egg cartons, paper towel rolls)

## 3b. Budget tier

> "Quick supplies question — Playtime can suggest stuff to stock up on (markers, paint, sensory bin, etc.) and even build you a shopping list. Pick a tier:"

- **starter** (~$50) — bare minimum, covers most activities
- **stocked** (~$150) — comfortable, covers ~95% of packets
- **well-stocked** (~$300+) — studio-grade
- **skip** — don't recommend supplies (no shopping prompts)

Save as `household.budgetTier` (`"starter"` / `"stocked"` / `"well-stocked"` / `null`).

Don't ask about Amazon links during setup — leave `household.affiliateTag` as `null` (uses plugin default). The parent can change it later via `/edit-playtime-household` if they want their own tag or opt out. The README documents the model.

## 4. Schedule & themes

Ask:

- **Theme cadence**: weekly themes (e.g. "Ocean Week"), random variety, or event-driven (themes follow what's happening that week)?
- **Auto-generate nightly?** If yes, what time should the packet be ready by? (Suggest 8pm so it's done before bed.) Note: for nightly auto-gen, after setup we'll offer to run `/schedule` to wire it up.
- **Upcoming events to weave in** (next 4-8 weeks): birthdays, holidays, family visits, trips, doctor visits, first day of school, sibling milestones. For each: date + short description + which kid(s) it's most relevant to.

## 5. Parenting frameworks

> "Some parents like activities that follow a particular philosophy. Want to pick one or two? Totally optional — most families don't use any, and that's fine."

Show the catalog from `skills/playtime-frameworks/SKILL.md`. They can pick zero, one, or many. Common picks:

- **Good Inside (Dr. Becky)** — emotion coaching, "two things are true"
- **Hunt, Gather, Parent** — TEAM (Together, Encourage, Autonomy, Minimal interference)
- **Bringing Up Bébé** — autonomy, patience, "le cadre" (the frame)
- **RIE (Magda Gerber)** — respectful infant care, uninterrupted play
- **Montessori** — practical life, prepared environment, follow the child
- **Waldorf / Steiner** — rhythm, imaginative play, natural materials
- **Reggio Emilia** — child as protagonist, project-based, "hundred languages"
- **Positive Discipline (Jane Nelsen)** — kind and firm, encouragement over praise
- **Whole-Brain Child (Dan Siegel)** — name it to tame it, integrate left/right brain
- **Simplicity Parenting (Kim John Payne)** — fewer toys, more rhythm, less stimulation
- **Collaborative & Proactive Solutions (Ross Greene)** — kids do well if they can
- **How To Talk So Little Kids Will Listen (Faber/King)** — communication tools

Read the matching reference file under `skills/playtime-frameworks/references/` for any they pick, and store the slugs in profile.json. The activity generator pulls these in at generation time.

## 6. Save and confirm

Write everything to `~/.playtime/profile.json` using this schema (validate before writing):

```json
{
  "version": 1,
  "household": {
    "familyName": "string",
    "timezone": "America/New_York",
    "printSize": "letter|a4",
    "defaultColorMode": "color|bw",
    "themeCadence": "weekly|random|event-driven",
    "budgetTier": "stocked",
    "affiliateTag": null,
    "autoGenerate": { "enabled": true, "time": "20:00" },
    "frameworks": ["good-inside", "hunt-gather-parent"],
    "events": [
      { "date": "2026-05-15", "description": "Grandma visits", "kids": ["maya"] }
    ],
    "materials": {
      "printerColor": true,
      "crayons": true,
      "...": "..."
    },
    "familyMembers": [
      {
        "id": "grandma-joy",
        "name": "Grandma Joy",
        "relation": "grandparent",
        "birthday": "1955-08-12",
        "notes": "Lives in Boston. Loves gardening. Calls every Sunday."
      },
      {
        "id": "biscuit",
        "name": "Biscuit",
        "relation": "pet",
        "kind": "golden retriever",
        "notes": "Scared of the vacuum, loves cheese."
      }
    ]
  },
  "kids": [
    {
      "id": "kebab-case-name-or-uuid",
      "name": "Maya",
      "nickname": "Mimi",
      "birthday": "2022-04-27",
      "pronouns": "she",
      "interests": ["dinosaurs", "ocean animals"],
      "currentObsessions": ["megalodon"],
      "learningGoals": ["letter sounds", "scissor skills"],
      "sensitivities": ["loud noises", "anything spider-related"],
      "skills": {
        "reading": "pre-reader",
        "writing": "letters",
        "scissors": "supervised"
      },
      "stuffies": [
        {
          "name": "Bun-Bun",
          "kind": "yellow bunny",
          "personality": "brave, loves carrots",
          "notes": "Bedtime companion since 6 months."
        }
      ],
      "notes": "Loves to be the helper; struggles with transitions",
      "createdAt": "2026-04-27T12:00:00Z"
    }
  ],
  "currentTheme": null
}
```

Also create `~/.playtime/history.json` if missing:

```json
{ "version": 1, "packets": [] }
```

# After setup

Show a friendly summary:

> ✨ **You're all set!** Playtime knows about [N kids: names], [F family members + pets], [S stuffies], your [M frameworks] approach, and [K upcoming events].
>
> **Next steps:**
> - Run `/generate-playtime` whenever you want to create tomorrow's packet (best done the night before so prints are ready in the morning).
> - Run `/edit-kid-details` to update interests, goals, or skills as kids grow.
> - Run `/add-kid-to-playtime` to add another kid.

If they enabled auto-generate, offer:
> Want me to set up nightly auto-generation via `/schedule` right now? I'd schedule a routine to run every night at [time].

# Tone

Be warm, playful, and curious throughout. Use the kids' names and nicknames once you have them. React to delightful details ("oh, megalodon obsession is the BEST"). Don't be saccharine or use exclamation marks every sentence. Confirm and validate as you go — never silently barrel through a long list of questions.

# Important

- Never overwrite an existing profile without explicit confirmation.
- If birthday is in the future or implies negative age, ask again.
- If interests/goals are vague ("learning"), gently ask for specifics — those drive personalization.
- Use `AskUserQuestion` for batched yes/no and multiple-choice prompts when it helps.
