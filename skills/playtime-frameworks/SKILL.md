---
name: playtime-frameworks
description: Parenting framework references (Good Inside, Hunt Gather Parent, Bringing Up Bébé, Montessori, RIE, Waldorf, Reggio Emilia, Positive Discipline, Whole-Brain Child, Simplicity Parenting, Collaborative & Proactive Solutions, How To Talk So Kids Will Listen). Use this when generating activities to subtly weave the household's chosen parenting philosophy into instructions, parent footers, and emotion-coaching scripts.
---

# Playtime parenting frameworks

The household may pick one or more parenting frameworks during `/setup-playtime`. When generating, **weave them in lightly** — not as preachy quotes, but as embedded scripts and design choices.

## How to use

1. Read `household.frameworks[]` from `~/.playtime/profile.json` (an array of slugs).
2. For each slug, read `references/<slug>.md`.
3. Apply the framework's "design implications" to:
   - The activity choice (some types fit some frameworks better)
   - The kid-facing instructions (tone, autonomy level, how choices are framed)
   - The "for grown-ups" parent footer (scripts for emotional moments, language to use)
   - The materials list (real tools vs. plastic; natural vs. synthetic)
4. **Never lecture the parent.** They picked the framework; we're a quiet ally.

## Catalog (slug → name)

| Slug | Name | Best fit for ages |
|---|---|---|
| `good-inside` | Good Inside (Dr. Becky Kennedy) | All |
| `hunt-gather-parent` | Hunt, Gather, Parent (Michaeleen Doucleff) | All, esp. 2-8 |
| `bringing-up-bebe` | Bringing Up Bébé (Pamela Druckerman) | All |
| `rie` | RIE / Magda Gerber | 0-3 |
| `montessori` | Montessori | All |
| `waldorf` | Waldorf / Steiner | All, esp. 0-7 |
| `reggio-emilia` | Reggio Emilia | All, esp. 3-8 |
| `positive-discipline` | Positive Discipline (Jane Nelsen) | 2+ |
| `whole-brain-child` | The Whole-Brain Child (Dan Siegel & Tina Bryson) | 2+ |
| `simplicity-parenting` | Simplicity Parenting (Kim John Payne) | All |
| `ross-greene` | Collaborative & Proactive Solutions (Ross Greene) | 4+ |
| `faber-mazlish` | How To Talk So Kids Will Listen (Faber & Mazlish / King) | 2+ |

## Multi-framework reconciliation

If the family picked multiple frameworks, look for **shared values** and lean into those. Most modern frameworks share: emotion-coaching, kid-as-capable, autonomy-with-warmth, problem-solving over punishment. Where they diverge (e.g. Bringing Up Bébé's "le pause" vs. RIE's quick attunement), **default to the warmer choice** — packets are joyful first, philosophy second.

## When in doubt

If no frameworks are selected, design with **default warm-and-respectful** sensibilities: kid is capable, feelings are welcome, choices are real, the activity is genuinely fun.
