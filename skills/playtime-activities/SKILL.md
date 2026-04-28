---
name: playtime-activities
description: Catalog of 30+ activity types Playtime can generate — connect-the-dots, mad libs, mazes, stories, crafts, science, mindfulness, and more. Each type has a reference spec with format, age range, quality bar, and examples. Use this when picking and generating an activity.
---

# Playtime activity catalog

Each activity type has a reference file in `references/<slug>.md` with:

- **What it is**
- **Age range** (typical)
- **Skills it builds**
- **Format spec** (how to construct it correctly)
- **Quality bar** (what separates a delightful version from a mediocre one)
- **Variations**
- **Material requirements**
- **Print considerations**

## Catalog

### Worksheets & paper puzzles

| Slug | Type | Ages |
|---|---|---|
| `connect-the-dots` | Connect the dots | 3-10 |
| `mad-libs` | Mad libs | 5-12 |
| `maze` | Maze | 3-12 |
| `coloring` | Coloring page | 2-10 |
| `letter-tracing` | Letter tracing | 3-6 |
| `number-tracing` | Number tracing | 3-6 |
| `word-search` | Word search | 6-12 |
| `crossword` | Crossword (kid level) | 6-12 |
| `cryptogram` | Cryptogram / cipher | 6-12 |
| `sudoku` | Sudoku (picture or number) | 5-12 |
| `logic-grid` | Logic grid puzzle | 7-12 |
| `spot-the-difference` | Spot the difference | 3-10 |
| `i-spy` | I-spy / hidden picture | 3-10 |
| `matching` | Matching pairs | 2-6 |
| `pattern` | Pattern completion | 3-6 |
| `counting` | Counting page | 2-6 |
| `math-puzzle` | Math puzzle | 5-12 |
| `rebus` | Rebus puzzle | 5-10 |

### Stories & writing

| Slug | Type | Ages |
|---|---|---|
| `story` | Read-aloud story | All |
| `choose-your-own-adventure` | Choose-your-own-adventure | 4-12 |
| `comic-strip` | Comic strip template | 4-12 |
| `finish-the-story` | Story-starter prompt | 4-12 |
| `letter-writing` | Letter to someone | 4-12 |
| `journaling` | Journaling prompt | 4-12 |
| `interview-prompts` | Interview a family member | 4-12 |
| `time-capsule` | Time-capsule prompts | 3-12 |
| `poetry` | Poetry prompt (haiku, acrostic, free) | 5-12 |

### Drawing

| Slug | Type | Ages |
|---|---|---|
| `drawing-prompt` | Open-ended drawing prompt | 2-12 |
| `finish-the-picture` | Finish-the-picture | 2-8 |
| `how-to-draw` | Step-by-step how-to-draw | 3-12 |
| `map-making` | Map-making | 4-12 |
| `symmetry-drawing` | Mirror / symmetry drawing | 5-10 |

### Crafts & making

| Slug | Type | Ages |
|---|---|---|
| `craft` | Craft project | 2-12 |
| `origami` | Origami | 4-12 |
| `paper-airplane` | Paper airplane variation | 4-12 |
| `building-challenge` | Build with constraints (LEGO, cardboard) | 3-12 |
| `sticker-activity` | Sticker placement | 1-6 |
| `sensory-bin` | Sensory bin invitation | 1-5 |

### Active & embodied

| Slug | Type | Ages |
|---|---|---|
| `scavenger-hunt` | Scavenger hunt | 2-12 |
| `indoor-scavenger-hunt` | Indoor scavenger hunt | 2-10 |
| `movement-cards` | Movement / animal-walk cards | 2-8 |
| `yoga-flow` | Yoga flow | 3-12 |
| `dance-prompt` | Dance prompt | 2-10 |
| `freeze-dance` | Freeze dance | 2-8 |
| `pretend-play-scenario` | Pretend play scenario | 2-8 |
| `dramatic-play-prompt` | Dramatic play prompt | 4-10 |

### Science & exploration

| Slug | Type | Ages |
|---|---|---|
| `science-experiment` | Kitchen science experiment | 3-12 |
| `nature-observation` | Nature observation | 2-12 |
| `weather-journal` | Weather journal | 4-10 |
| `cooking-recipe` | Kid cooking recipe | 3-12 |

### Social-emotional & mindful

| Slug | Type | Ages |
|---|---|---|
| `feelings-check-in` | Feelings check-in | 2-10 |
| `gratitude-journal` | Gratitude journal | 4-12 |
| `acts-of-kindness` | Acts of kindness challenge | 3-12 |
| `mindfulness` | Mindfulness exercise | 4-12 |
| `concept-of-the-day` | Concept / virtue / vocabulary | 2-12 |
| `word-of-the-day` | Word of the day | 5-12 |

### Music & audio

| Slug | Type | Ages |
|---|---|---|
| `song-or-rhyme` | Song or rhyme with gestures | 0-6 |
| `rhythm-clap` | Rhythm and clap pattern | 3-8 |

### Magic & games

| Slug | Type | Ages |
|---|---|---|
| `magic-trick` | Simple magic trick | 5-12 |
| `riddle` | Riddle or brain teaser | 4-12 |
| `joke-of-the-day` | Joke / pun (small extra) | 4-12 |

## Picking strategy

See `commands/generate-playtime.md` step 3 for the full picker. Briefly:

1. Filter by age band
2. Filter by available materials
3. Filter out recent (last 5 days)
4. Filter out "didn't land" twice in a row
5. Weight by: recent loves (+3), current learning goals (+2), current obsessions (+2), domain rotation (-1 per day since variety)
6. Pick highest-weighted with small random component

## Variety

A great month touches:

- Cognitive (puzzles, logic, math)
- Language (mad libs, story, writing)
- Creative (drawing, craft, music)
- Physical (movement, scavenger hunt, yoga)
- Social-emotional (feelings, kindness, gratitude)
- Science (experiment, nature, cooking)
- Practical (life skills, real tasks)

## Reference files

I'll have detailed specs for the high-value, frequently-used types. For others, the catalog table above + the activity name + context from age band reference + frameworks is enough for the generator to do good work.

Reference files included:

- `connect-the-dots.md`
- `mad-libs.md`
- `maze.md`
- `story.md`
- `craft.md`
- `science-experiment.md`
- `scavenger-hunt.md`
- `feelings-check-in.md`
- `concept-of-the-day.md`
- `pretend-play-scenario.md`
- `comic-strip.md`
- `cooking-recipe.md`
- `how-to-draw.md`

For activity types not listed above, use this skill's catalog + your judgment + the age-band reference + frameworks. Quality bar applies universally:

✅ feels handmade for THIS kid
✅ right developmental level
✅ self-contained
✅ genuinely fun
✅ print-ready
✅ material-aware
✅ color-mode aware
