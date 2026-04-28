# Playtime

A Claude Code plugin that generates **personalized morning activity packets for kids** — one per kid, every night, ready to print before breakfast.

Connect-the-dots, mad libs, mazes, stories, crafts, kitchen science, mindfulness, pretend-play kits — tuned to each kid's age (in months for under-3), interests, current obsessions, learning goals, and your family's parenting framework.

Tracks what landed and what didn't. Gets better over time.

## What it does

- Asks about your kids during setup — names, ages (granular), interests, learning goals, sensitivities, what materials you have on hand
- Lets you pick a **parenting framework** (Good Inside, Hunt-Gather-Parent, Bringing Up Bébé, Montessori, Waldorf, Reggio Emilia, RIE, Positive Discipline, Whole-Brain Child, Simplicity Parenting, Ross Greene, Faber-Mazlish) — quietly threads it into instructions, parent footers, and emotion-coaching scripts
- **Generates one packet per kid** — never the same activity twice, balanced across cognitive, creative, physical, social-emotional, and science domains
- **Print-ready HTML** — Cmd+P and you're done. Color or B&W, US Letter or A4
- **Feedback loop** — asks how yesterday landed; tracks per-kid history; weighs future activities by what's working
- **Theme-aware** — weekly themes, random variety, or event-driven (Grandma is visiting → portrait + interview prompts)

## Slash commands

| Command | What it does |
|---|---|
| `/setup-playtime` | First-time setup — household, kids, family + pets, stuffies, materials, frameworks, themes |
| `/add-kid-to-playtime` | Add another kid (with their stuffies) |
| `/edit-kid-details` | Update a kid's interests, goals, skills, stuffies as they grow |
| `/edit-playtime-household` | Update household-level info — family members, events, materials, frameworks, themes |
| `/generate-playtime` | Generate tomorrow's packets (collects feedback on yesterday first) |
| `/playtime-checkup` | Audit the profile for gaps and stale data; suggest fixes |
| `/playtime-shop` | Personalized supplies shopping list with affiliate-linked Amazon search URLs |
| `/playtime-share` | Convert a packet into shareable formats — share card (texting Grandma), PDF, or PNG/JPEG |

## How to install

Playtime works in any Claude Code surface — the desktop app, the terminal CLI, the web app, or the IDE extensions. It ships with a marketplace manifest (`.claude-plugin/marketplace.json`) so it installs through Claude Code's standard plugin workflow.

For a parent-friendly walkthrough (with the recommended desktop-app path), see **[dailyplaytime.com/install](https://dailyplaytime.com/install)**.

### From inside Claude Code

In a Claude Code session (desktop app's Code tab, `claude` in your terminal, or claude.ai/code on the web), type these two slash commands:

```
/plugin marketplace add jtemps/playtime-plugin
/plugin install playtime@playtime-marketplace
```

That's it. To verify the plugin loaded, type `/plugin` to open the plugin manager.

### From a local clone (developer mode)

If you want to hack on the plugin locally:

```bash
git clone https://github.com/jtemps/playtime-plugin.git ~/playtime-plugin
```

Then in a Claude Code session:

```
/plugin marketplace add ~/playtime-plugin
/plugin install playtime@playtime-marketplace
```

### Once installed

The slash commands show up:

```
/playtime:setup-playtime
/playtime:add-kid-to-playtime
/playtime:edit-kid-details
/playtime:edit-playtime-household
/playtime:generate-playtime
/playtime:playtime-checkup
/playtime:playtime-shop
/playtime:playtime-share
```

Run `/playtime:setup-playtime` to begin.

### Updating

In a Claude Code session:

```
/plugin marketplace update playtime-marketplace
```

(If you installed from a local clone, run `git pull` in `~/playtime-plugin` first.)

## How state works

All state lives at `~/.playtime/`:

```
~/.playtime/
├── profile.json          # household + all kids
├── packets/              # generated packets, organized by date
│   └── YYYY-MM-DD/
│       └── <kid-name>.html
└── history.json          # feedback log + activity ledger
```

The plugin folder itself is stateless — clone it, share it, version-control it without leaking your kids' data.

## Suggested rhythm

The packets are designed to be generated **the night before** so they're ready in the morning. A nice flow:

1. After dinner: run `/generate-playtime`
2. Tell Claude how yesterday's packet went (~30 seconds)
3. Claude generates tomorrow's packets
4. Open the HTML files, print them, leave them on the breakfast table
5. Wake up to small humans doing delightful things

You can also use the `/schedule` skill to wire up nightly auto-generation. Setup will offer this if you opt in.

## Activity types

40+ types across:

- **Worksheets**: connect-the-dots, mazes, mad libs, word search, crosswords, sudoku, cryptograms, logic puzzles, math puzzles, spot-the-difference, I-spy
- **Stories & writing**: read-aloud stories, choose-your-own-adventure, comic strips, finish-the-story, letters, journals, interview prompts, time capsules, poetry
- **Drawing**: open prompts, finish-the-picture, step-by-step how-to-draw, map-making, symmetry
- **Crafts & making**: crafts, origami, paper airplanes, building challenges, sticker activities, sensory bins
- **Active**: scavenger hunts, movement cards, yoga flows, dance prompts, pretend play scenarios
- **Science**: kitchen experiments, nature observation, weather journals, cooking recipes
- **Social-emotional**: feelings check-ins, gratitude journals, acts-of-kindness, mindfulness, concept-of-the-day, word-of-the-day
- **Music**: songs and rhymes, rhythm and clapping
- **Magic & games**: simple magic tricks, riddles, jokes

## Ages

Granular by month for 0-3 yr (massive developmental change), then by year:

- 0-6 months · 6-12 months · 12-18 months · 18-24 months
- 2-3 yr · 3-4 yr · 4-5 yr · 5-6 yr
- 6-8 yr · 8-10 yr · 10-12 yr

Each band has its own developmental reference: what's happening, what works, what to skip, common obsessions to ride.

## Parenting frameworks

Pick zero, one, or many:

- **Good Inside** (Dr. Becky Kennedy)
- **Hunt, Gather, Parent** (Michaeleen Doucleff)
- **Bringing Up Bébé** (Pamela Druckerman)
- **RIE** (Magda Gerber)
- **Montessori**
- **Waldorf / Steiner**
- **Reggio Emilia**
- **Positive Discipline** (Jane Nelsen)
- **The Whole-Brain Child** (Dan Siegel & Tina Bryson)
- **Simplicity Parenting** (Kim John Payne)
- **Collaborative & Proactive Solutions** (Ross Greene)
- **How To Talk So Kids Will Listen** (Faber & Mazlish / King)

The chosen framework subtly shapes activity choice, instruction tone, and the "for grown-ups" footer — never preachy.

## Design principles

1. **Delightful and charming first.** Educational second. The packet has to genuinely thrill the kid.
2. **Print-ready forever.** Self-contained HTML, inline SVG, no external dependencies. Will work in 10 years.
3. **Personalized.** Kid's name in the title. Their interests in the activity. Not a generic worksheet.
4. **Material-aware.** Only asks for stuff your house has.
5. **Variety + memory.** Never repeat. Balance across cognitive / creative / physical / social-emotional / science.
6. **Feedback loop.** Gets better the longer you use it.
7. **Quiet ally on values.** If you've picked Montessori or Good-Inside, we know the moves; we don't lecture you about them.

## File structure

```
playtime-plugin/
├── .claude-plugin/
│   └── plugin.json
├── commands/
│   ├── setup-playtime.md
│   ├── add-kid-to-playtime.md
│   ├── edit-kid-details.md
│   ├── edit-playtime-household.md
│   ├── generate-playtime.md
│   ├── playtime-checkup.md
│   ├── playtime-shop.md
│   └── playtime-share.md
├── skills/
│   ├── playtime-curriculum/
│   │   ├── SKILL.md
│   │   └── references/age-*.md (11 age bands)
│   ├── playtime-frameworks/
│   │   ├── SKILL.md
│   │   └── references/<framework>.md (12 frameworks)
│   ├── playtime-activities/
│   │   ├── SKILL.md
│   │   └── references/<activity>.md (13 activity-type specs)
│   ├── playtime-print/
│   │   ├── SKILL.md
│   │   └── references/{template.html, color-vs-bw.md, svg-snippets.md}
│   ├── playtime-supplies/
│   │   ├── SKILL.md
│   │   └── references/{starter-kit-by-age.md, by-category.md, affiliate-links.md}
│   └── playtime-share/
│       ├── SKILL.md
│       └── references/{formats.md, rendering.md, share-card-template.html, text-template.md}
├── README.md
└── .gitignore
```

## Supplies

`/playtime-shop` builds a personalized supplies shopping list (paper, paint, scissors, sensory bin stuff) tuned to each kid's age and your `budgetTier` (`starter` ~$50 / `stocked` ~$150 / `well-stocked` ~$300+). The list always leads with **free items to save from home** (cardboard tubes, egg cartons, jars) and **pantry staples** (baking soda, vinegar, food coloring) before pointing to anything to buy.

`/playtime-checkup` flags missing supplies in its diagnostic report only when a gap is genuinely *blocking* activities. `/generate-playtime` may weave a single soft suggestion into the parent footer when the day's activity needed a material you don't have — with a substitute always offered, and a 30-day cooldown per item.

## Sharing

`/playtime-share` turns a generated packet into three artifacts:

- **Share card** — 1080×1080 PNG, optimized for texting (Grandma, the visiting playdate parent). Never includes affiliate links or sales messaging.
- **PDF** — full-fidelity, keeps the affiliate footer (private to the parent).
- **PNG / JPEG** — full packet as one image, with the affiliate footer stripped.

The command can also draft a paste-ready message resolved against your family graph ("Hi Grandma Joy — Maya did this morning packet today and asked to do it three times…"). It never auto-sends.

### Amazon links

The plugin generates Amazon **search URLs** (not specific product ASINs) tagged with `dailyplay-20` by default — search URLs don't rot, you pick the actual product, and a small commission flows to the plugin maintainer if you buy. You can swap in your own Amazon Associates tag, or opt out entirely (no tag, plain Amazon search), via `/edit-playtime-household` → Preferences → Amazon link tag. Heads-up on the [180-day rule](https://affiliate-program.amazon.com): if you sign up your own tag and don't drive 3 sales in the first 180 days, Amazon closes the account — most parents are best leaving the default in place. Plugin forking + Operating Agreement details live in `skills/playtime-supplies/references/affiliate-links.md`.

## License

MIT
