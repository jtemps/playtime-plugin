---
description: Update a kid's interests, obsessions, learning goals, sensitivities, or skills as they grow.
---

Edit an existing kid in `~/.playtime/profile.json`.

# Steps

1. **Read** `~/.playtime/profile.json`. If missing or no kids, point to `/setup-playtime`.

2. **List the kids** and ask which to edit. If they only have one, skip the question.

3. **Show the current profile** for that kid (formatted nicely, not raw JSON), then ask **what they want to update**. Present options:
   - Interests (the durable list)
   - Current obsessions (the "right now" list — gets refreshed often)
   - Learning goals
   - Sensitivities
   - Skill levels (reading / writing / scissors)
   - **Stuffies / loveys** — add new ones, retire old ones, update personality notes
   - Nickname
   - Notes
   - Pronouns
   - Birthday (only if it was wrong — don't change for "they got older", we compute age from birthday)

   Multi-select OK; let them update several at once.

4. **For each section being edited**, show the current value and offer to **add to / replace / remove from** it. Don't silently overwrite arrays. For free-text fields (notes), offer to append or replace.

5. **Show the diff** before writing — old value → new value, side by side or in plain prose.

6. **Write back**, preserving everything else exactly. Bump a `updatedAt` timestamp on the kid.

7. **Append a note to history** that the profile changed, so the activity generator can react. Add to `~/.playtime/history.json`:

```json
{
  "type": "profile-update",
  "kidId": "maya",
  "date": "2026-04-27T12:00:00Z",
  "changes": ["added interest: knights", "removed obsession: trucks"]
}
```

8. **Confirm** — short and warm:
   > Got it — [Name]'s profile updated. Tomorrow's packet will reflect [biggest change].

# Tips for keeping profiles fresh

When the user finishes editing, gently mention they can:
- Run this anytime an obsession peaks or fades
- Update learning goals as kids master things ("they've nailed letter sounds — what's next?")
- Add sensitivities the moment something hits a nerve

# Important

- Never edit a kid by name without showing the current profile first. Names can be ambiguous.
- If a kid's age has crossed a developmental threshold (e.g. just turned 3, just turned 5), proactively suggest reviewing learning goals and skill levels.
