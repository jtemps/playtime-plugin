---
description: Generate a personalized supplies shopping list with affiliate-linked Amazon search URLs, tailored to the family's kids, budget tier, and what they already have.
---

You are generating a **shopping list** for the family — what to buy (or save from the recycling) to make tomorrow's packets richer.

# 0. Read state

```
~/.playtime/profile.json
```

If profile.json is missing → point to `/setup-playtime` and stop.

# 1. Pull the inputs

From profile.json:

- **Kids' ages** → determine which age band(s) to draw from (`skills/playtime-supplies/references/starter-kit-by-age.md`).
  - If kids span multiple bands, union the recommendations and dedupe.
- **`household.budgetTier`** → `starter` / `stocked` / `well-stocked`. Default to `stocked` if unset.
- **`household.materials`** → what they already have. Skip those.
- **`household.affiliateTag`** → resolve via `skills/playtime-supplies/references/affiliate-links.md`:
  - explicit user tag → use it
  - explicit `""` → no tag
  - unset → plugin default

# 2. Build the list

For each recommended item:

1. Look up the **search keyword** in `skills/playtime-supplies/references/by-category.md`.
2. Build the affiliate URL per the format in `affiliate-links.md`.
3. Render as a markdown link: `[Item name](URL)`.

## Group by tier

```
🎯 Buy now (starter)
- [Crayola crayons 24-pack](https://www.amazon.com/s?k=crayola+crayons+24+pack&tag=dailyplay-20)
- [Kids safety scissors (blunt-tip)](...)
...

📦 Nice to add (stocked)
- ...

✨ Aspirational (well-stocked)
- ...
```

If the user is on `starter`, show only the starter list and mention the next tier exists ("Want the stocked add-ons too?").
If on `stocked`, show starter (greyed/marked as "essentials") + stocked.
If on `well-stocked`, show all three.

## Always include the FREE section first

```
🆓 Save these from home (worth more than half the supplies you'd buy)
- Cardboard tubes (paper towel, TP)
- Egg cartons
- Cereal boxes / cardboard
- Bottle caps
- Old magazines / catalogs
- Mason jars / clear cups
```

## Always include the pantry section

```
🥄 Already in the kitchen probably
- Baking soda · Vinegar · Salt · Cornstarch · Vegetable oil · Food coloring · Flour
```

## Skip what they already have

If `materials` includes `crayons`, don't recommend crayons. Drop them silently — don't list "you already have ___" as filler.

## Estimate total

After the list:

```
~Estimated: $40-60 (starter only)
```

Use rough ranges, not exact prices. Pull from the budget tier in `starter-kit-by-age.md`.

# 3. The disclosure footer

If Amazon links were included, append one small italic line at the bottom:

> *Amazon links earn the plugin a few cents if you buy. Swap or remove the tag in `/edit-playtime-household`.*

If `household.affiliateTag === ""` (opted out), OMIT the line entirely.

# 4. Output structure

```
🛒 Playtime shopping list for the [LASTNAME] family

Built for: Maya (4) + Theo (almost 5)
Budget tier: stocked
Existing materials skipped: crayons, washable markers, glue sticks

🆓 Save these from home
[free list]

🥄 Already in the kitchen probably
[pantry list]

🎯 Buy now (starter)
[items]

📦 Nice to add (stocked)
[items]

~Estimated: $80-120 for the items above

[disclosure footer]
```

# 5. Offer the next step

End with:

> Want me to:
> - Save this list to `~/.playtime/shopping-list-YYYY-MM-DD.md`
> - Bump your budget tier (currently: stocked)
> - Mark items as "owned" in your profile so I stop recommending them

# Tone

- Practical, not pushy
- Lead with the free stuff — that's the actual unlock
- Honest about what's needed vs. nice
- Don't pad the list to inflate the affiliate take

# Important

- Never recommend a specific brand/product unless it's a category default (Crayola, Play-Doh, Magna-Tiles, Fiskars). We use search URLs, not ASIN URLs.
- Never include affiliate links in kid-facing content. This is for the parent only.
- If `affiliateTag === ""`, generate plain Amazon search URLs and omit the disclosure.
- Respect existing `materials` — silently skip duplicates.
