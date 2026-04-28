---
description: Audit the Playtime profile for gaps, stale data, and missing context that would make packets better. Reports what's missing and offers to fix it.
---

You are running a **checkup** on the Playtime profile — finding gaps the parent could close to make generated packets sharper.

# 0. Read state

```
~/.playtime/profile.json
~/.playtime/history.json
```

If profile.json is missing, point to `/setup-playtime`.

# 1. Run the audit

Walk through these checks. Each one either passes ✓ or surfaces a finding ⚠️.

## Household-level

- [ ] **Family members & pets** — at least 1? At least 2? (Most kids have a meaningful cast — both parents, often grandparents, often pets. Empty list usually means setup was rushed.)
- [ ] **Family birthdays** — for each family member, is `birthday` set? (Birthdays unlock surprise event-driven themes.)
- [ ] **Best friends / close friends** — does the family list any close friends as `relation: "close-friend"` for the kids? (These are great for letter-writing, kindness prompts.)
- [ ] **Caregivers** — are regular caregivers (nannies, regular sitters, grandparent who watches them) listed?
- [ ] **Upcoming events** — at least one within the next 4 weeks? Is the list stale (all events in the past)?
- [ ] **Materials** — has the family answered the materials questions? Empty `materials` block means we'll be conservative on crafts.
- [ ] **Frameworks** — selected? (Optional, but if `null` and parents have opinions, this is a missed opportunity.)
- [ ] **Theme cadence** — set? (`weekly` / `random` / `event-driven`)
- [ ] **Print prefs** — paper size + color mode set?
- [ ] **Auto-generate** — set up? Is the cron actually running (last packet generated when?)
- [ ] **Affiliate tag** — set explicitly (own tag), explicitly empty (opted out), or default (using plugin default)?
- [ ] **Budget tier** — set? (Used by `/playtime-shop` for tiered recommendations.)

## Per-kid

For each kid:

- [ ] **Stuffies / loveys** — at least 1? (Kids almost always have one. Empty list often means setup skipped.)
- [ ] **Current obsessions** — set? Is the list stale (>2 months since last update)? Obsessions decay fast.
- [ ] **Interests** — set? Is the list thin (<3 items)?
- [ ] **Learning goals** — set? Are they age-appropriate (3yr-old's goals shouldn't be "tying shoes")?
- [ ] **Sensitivities** — set or explicitly "none"?
- [ ] **Skill levels** — reading / writing / scissors all set?
- [ ] **Age threshold approaching** — is the kid within 30 days of crossing into a new band (e.g. turning 3, 4, 5)? If yes, suggest reviewing learning goals + skill levels.
- [ ] **Birthday set?** — required for age computation
- [ ] **Profile freshness** — when was this kid's profile last updated? If >3 months, suggest a quick edit pass.
- [ ] **Skill-vs-supply blockers** — for each skill (reading / writing / scissors) flagged as "supervised" or higher, check the matching supply is in `household.materials`. If kid has scissors skills but the household has no kid scissors, that's a blocker, not just a "nice supply to add" — fold the affiliate suggestion INTO the skill finding.

## History health (signal from generated packets)

- [ ] **Activity diversity** — across the last 30 days of packets, has the kid hit 4+ different activity TYPES? If <3, picks are getting stale.
- [ ] **Domain rotation** — has the kid touched cognitive, creative, physical, social-emotional, science domains in the last 14 days? Flag any domain that's been missed for 21+ days.
- [ ] **Feedback density** — of the last 14 packets, what fraction got rated? If <50%, parent isn't logging feedback — gently note that the system gets smarter with feedback.
- [ ] **"Didn't land" streak** — any recent run of 3+ "didn't land" ratings in a row? If yes, recommend updating interests or skill levels.
- [ ] **Skipped streak** — any recent run of 3+ "skipped" ratings? If yes, recommend changing the timing or simplifying the format.

## Plugin health

- [ ] **Profile schema version** — does it match current?
- [ ] **State directory permissions** — readable + writable?
- [ ] **Packet directory size** — flag if >100MB (suggest `/playtime-archive` if/when that exists)

# 2. Render the report

Group findings by severity. Be warm, not preachy.

## Output structure

```
🩺 Playtime Checkup
Generated 2026-04-27

✨ The good
- 2 kids set up · 4 family members · 6 stuffies · Good Inside framework
- Maya's last 14 days: 6 activity types, all 5 domains touched
- Theo's last 14 days: 5 activity types, "loved" rate is 70%

⚠️ Worth a quick fix
- No upcoming events in the next 4 weeks → event-driven themes will fall back to random. Add a few via /edit-playtime-household.
- Theo has no current obsessions listed. Last obsession update: 4 months ago. Quick refresh would sharpen tomorrow's pick. Update via /edit-kid-details.
- 3 family members are missing birthdays (Grandma Joy, Uncle Pete, Cousin Eli). Adding these unlocks surprise birthday themes.

💡 Nice-to-have
- Maya's scissors skill is "supervised" but the household has no kid scissors listed in materials. That's why tomorrow's craft picker is dodging cutting activities — adding a [pair of blunt-tip kid scissors](https://www.amazon.com/s?k=kids+safety+scissors+blunt+tip&tag=dailyplay-20) unlocks ~30% of the craft catalog.
- No best friends listed. Adding 1-2 unlocks letter-writing and kindness prompts about real people.
- Materials block hasn't been touched since setup. Want a quick pantry check?

🎨 Activity unlocks (gated by missing supplies)
- 4 craft types in the last 14 days got skipped because the household lacks tempera paint. If you want them back in rotation: [washable tempera paint set](https://www.amazon.com/s?k=washable+tempera+paint+kids&tag=dailyplay-20).
- For a full personalized list with budget tiers + free-from-home picks, run /playtime-shop.

*Amazon links may earn the plugin a few cents — opt out via `/edit-playtime-household`.*

🎂 Coming up
- Theo turns 5 in 18 days → suggest reviewing learning goals + skill levels
- Grandma Joy's birthday in 22 days → /generate-playtime will likely lean event-driven the morning of

📊 Activity hygiene
- Both kids' "physical" domain hasn't been hit in 16 days. Tomorrow could be a movement card / yoga / scavenger hunt for at least one of them.
- Feedback rate: 40% over the last 14 days. We get sharper with feedback — even one-word ratings ("loved", "skipped") help.
```

# 3. Offer to fix

End with:

> **Want me to walk through any of these now?** I can:
>
> - Update [Kid]'s obsessions (`/edit-kid-details`)
> - Add the missing family birthdays (`/edit-playtime-household`)
> - Generate a starter shopping list (`/playtime-shop`)
> - Add a few upcoming events
> - Refresh skill levels for the kids approaching a new age band
>
> Just say which one(s) — or "all" and I'll batch through them.

# Tone

- Warm, not nagging
- "Here's the good stuff first" before findings
- Specific (numbers, dates, names) — generic notes don't earn the parent's attention
- Always-actionable (every finding has a way to address it)

# Important

- **Don't generate fixes without the parent saying yes.** This is a diagnostic, not a force-applier.
- If the profile is genuinely in great shape, **say so simply** ("Everything looks great. Carry on.") — don't manufacture findings.
- Run silently if the user passed `--quiet` or similar — output the report only with no follow-up offer.
- **Amazon links in checkup**: only include them where a missing supply is genuinely BLOCKING activities (not "nice to have someday"). At most 2 links per checkup. Always pair the link with the *reason* (which activities are being dodged because of the gap). Never lead with shopping; lead with the diagnostic insight, then the optional unlock. **Don't stamp links with "(Affiliate link.)"** — append a single small italic line at the bottom of any checkup output that contains links: *"Amazon links may earn the plugin a few cents — opt out via `/edit-playtime-household`."* If `household.affiliateTag === ""` (opted out), use plain Amazon search URLs and skip that footer line entirely. If `budgetTier === null`, omit the link section altogether.
