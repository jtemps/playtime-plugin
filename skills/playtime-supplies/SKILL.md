---
name: playtime-supplies
description: Recommended supplies and starter kits for Playtime activities — tiered by household stage (toddler/preschool/elementary), budget tier, and category. Use this when generating shopping lists or when an activity wants to suggest a missing material via an Amazon search link.
---

# Playtime supplies

This skill catalogs **what's actually useful to have on hand** for Playtime activities, broken down by:

- **Age/stage** — toddler / preschool / early elementary / older
- **Budget tier** — starter / stocked / well-stocked
- **Category** — paper, drawing, cutting/glue, paint, sensory, building, recyclables, kitchen science, books

## How to use

1. Read `~/.playtime/profile.json` for kids' ages and current `materials` toggles.
2. Read budget tier from `household.budgetTier` (if set) — otherwise default to "stocked".
3. Read affiliate tag — `household.affiliateTag` if set, otherwise plugin default in `references/affiliate-links.md`.
4. Cross-reference what's missing vs. what's recommended for the household's age band(s) and budget tier.
5. Build a shopping list with affiliate-linked Amazon search URLs (see `references/affiliate-links.md`).
6. Always include an FTC disclosure footer.

## Reference files

- `references/starter-kit-by-age.md` — what to stock for each age band
- `references/by-category.md` — flat list with descriptions and search keywords
- `references/affiliate-links.md` — affiliate URL builder + disclosure language

## Budget tiers

| Tier | Total est. | Description |
|---|---|---|
| `starter` | ~$50 | The bare minimum to do most activities. Covers ~70% of generated packets. |
| `stocked` | ~$150 | Comfortable. Covers ~95% of packets without scrambling. |
| `well-stocked` | ~$300+ | Studio-grade. Sensory bins, real watercolors, a glue gun, the works. |

If `budgetTier` isn't set, default to `stocked` recommendations.

## Important principles

- **Recommend categories, not specific products.** We use Amazon **search URLs** with affiliate tags, not specific ASINs. Search URLs don't rot, and the parent picks the actual product.
- **Quality matters more than quantity.** A great pair of kid scissors lasts; a 10-pack of bad ones fights you forever.
- **Be honest about what's needed vs. nice.** A starter kit shouldn't sneak in a $40 watercolor set.
- **Reuse before buying.** Many activities use household recyclables (TP rolls, egg cartons, cardboard boxes) — point this out, don't recommend buying.
- **Disclose.** Every shopping list includes an FTC affiliate-link disclosure.
