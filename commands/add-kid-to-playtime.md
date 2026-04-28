---
description: Add a new kid to an existing Playtime household.
---

Add a new kid to `~/.playtime/profile.json`.

# Steps

1. **Read** `~/.playtime/profile.json`. If it doesn't exist, tell the user to run `/setup-playtime` first.

2. **Gather kid details** (same fields as `/setup-playtime` step 2):
   - Name + nickname
   - Birthday (full date)
   - Pronouns (default "they")
   - Interests
   - Current obsessions (this-month list)
   - Learning goals
   - Sensitivities
   - Skill levels (reading, writing, scissors)
   - **Stuffies / loveys** — names, kinds, personalities, notes (see `/setup-playtime` for examples — these power stories and pretend play)
   - Notes

   Ask conversationally, in batches. Don't hand them a form.

3. **Show a draft** of the new kid object before writing it back. Let them tweak before committing.

4. **Write** — append to `kids[]` array, preserve everything else exactly. Update `version` if needed. Use a unique `id` (kebab-case of name; if collision, append `-2` etc.).

5. **Confirm** with a short, warm summary:
   > 🎉 Welcome to Playtime, [Name]! Next packet you generate will include them. Their profile sits alongside [other kids' names].

6. **Offer**: "Want me to generate a fresh packet now that includes [new kid]?" → if yes, run `/generate-playtime`.

# Important

- Don't touch existing kids' data.
- If the family already has a kid with the same name, ask whether to use a nickname-based id to disambiguate.
- If birthday looks wrong (future date, age >18), ask again.
