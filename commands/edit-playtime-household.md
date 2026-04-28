---
description: Edit household-level Playtime details — family members, pets, upcoming events, materials, frameworks, themes, print preferences.
---

Edit the household-level pieces of `~/.playtime/profile.json`. Use this for anything that's NOT kid-specific (for kids, use `/edit-kid-details`).

# Steps

1. **Read** `~/.playtime/profile.json`. If missing, point to `/setup-playtime`.

2. **Show what's here** — a friendly summary of the household block: family members + pets, upcoming events, materials on hand, chosen frameworks, theme cadence, print prefs. Don't dump JSON.

3. **Ask what they want to update.** Present options:

   - **Family members & pets** — add new (Grandma Joy just moved closer; new puppy), update details, retire (someone moved away), edit birthday
   - **Upcoming events** — add (birthday party, trip, doctor visit), remove (it happened), edit
   - **Materials on hand** — pantry/craft updates ("we ran out of glue", "we just got a glue gun")
   - **Parenting frameworks** — add, remove, change
   - **Theme cadence** — weekly / random / event-driven
   - **Print preferences** — color/B&W default, paper size
   - **Auto-generate schedule** — enable/disable, change time
   - **Budget tier** — starter / stocked / well-stocked / skip (controls supply recommendations)
   - **Family name / timezone**
   - **Preferences** (advanced) — Amazon link tag, other rarely-touched settings

   Multi-select OK; let them update several at once.

4. **For each section being edited:**

   ### Family members
   - Show the current cast as a small table (name · relation · birthday · 1-line note)
   - For adds: collect name, relation, birthday (optional), notes (lives where, loves what, a quirk)
   - For pets: collect kind (dog/cat/etc.) too
   - For removes: confirm — "are you sure? This will affect future packets that might mention them"
   - Use kebab-case `id` based on name

   ### Events
   - Show upcoming events sorted by date
   - For adds: date, description, which kid(s) it's most relevant to (optional — defaults to all)
   - For removes: just confirm

   ### Materials
   - Show current toggles by category
   - Add/remove items; ask about what changed

   ### Frameworks
   - Show current picks
   - Offer the catalog (see `skills/playtime-frameworks/SKILL.md`)
   - Multi-select to add; remove with confirmation

   ### Budget tier
   - Show current tier and what it controls (supply recommendations from `/playtime-shop` and `/playtime-checkup`)
   - Options: `starter` / `stocked` / `well-stocked` / `null` (skip recommendations entirely)

   ### Preferences (advanced)
   Only show this submenu if the parent picks it. Inside:
   - **Amazon link tag** — controls who earns the small commission on Amazon links the plugin generates.
     - Show current state: "plugin default" / `"yourname-20"` (own tag) / "opted out"
     - Options: keep default (`null`), use own tag (string like `"yourname-20"`), opt out (empty string `""`)
     - If they ask where to get their own tag, point to `affiliate-program.amazon.com`

5. **Show the diff** before writing — old → new in plain prose.

6. **Write back**, preserving everything else exactly. Bump household `updatedAt` timestamp.

7. **Append a note to history** (`~/.playtime/history.json`):

```json
{
  "type": "household-update",
  "date": "2026-04-27T12:00:00Z",
  "changes": ["added family member: Cousin Eli", "removed event: dentist visit"]
}
```

8. **Confirm** — short and warm:
   > Got it — household updated. Tomorrow's packets will know about [biggest change].

# Tips

- Family members are gold for stories, pretend play, letter-writing, interviews, birthday-themed packets, and acts of kindness. Encourage the family to keep this fresh — when someone new becomes important to the kids (new baby cousin, new teacher, new neighbor friend), add them.
- For pets: include personality details ("scared of the vacuum", "loves cheese") — they make great supporting characters in stories.
- For upcoming events: 4-8 weeks of lookahead is the sweet spot for event-driven theming.

# Important

- Don't touch kid-specific data here (use `/edit-kid-details`).
- If a family member shares a name with another (two grandmas both called "Grandma"), disambiguate with a nickname or city ("Grandma Joy" vs. "Grandma Boston").
- If birthday is in the future or implies negative age, ask again.
