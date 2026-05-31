---
name: weekly-checkin
description: Guided weekly reflection and synthesis covering wins, challenges, patterns, and priorities for the coming week — saved as a structured note in the Obsidian vault
---

# Weekly Check-In

## Purpose
A 10–15 minute guided reflection that synthesizes the week's notes, identifies patterns across domains, and sets clear intentions for the week ahead. This is a ritual, not a report.

## When to Invoke
- User says "weekly check-in", "weekly review", "end of week", "week in review"
- User says "reflect on my week", "how did my week go"
- User runs `/weekly-checkin`

---

## Step 1: Get Today's Date and Week Range

```bash
TZ=[USER_TIMEZONE] date +"%Y-%m-%d"
```

Calculate the past 7 days for context gathering. Note the week number (e.g., `2026-W22`) for the filename.

---

## Step 2: Read the User Profile

Look for `~/Documents/vault/Reference/COGsetup/MY-PROFILE.md`.

If found, read it to get:
- **Name** — for personalization
- **Active projects** — to review project-specific progress during the check-in
- **Role/occupation** — to frame professional questions appropriately
- **Task manager** — to pull completed/overdue tasks

---

## Step 3: Gather Context from the Vault

Scan these locations for content from the past 7 days:

1. **Daily notes** — `~/Documents/vault/Daily/` — recent dated files
2. **Journal entries** — `~/Documents/vault/Journal/` — any entries this week
3. **Braindumps** — `~/Documents/vault/Reference/Braindumps/` — recent captures
4. **Previous check-ins** — `~/Documents/vault/Reference/Checkins/` — last week's note for continuity

Read these files to build context before asking the user anything. Note recurring themes, unresolved concerns, and tasks mentioned.

---

## Step 4: Fetch Task Summary

Use the task manager MCP to pull:
- Tasks completed this week
- Tasks overdue from this week or earlier
- Tasks originally due this week that are still open

This gives context for the wins/challenges conversation.

---

## Step 5: Guided Reflection

Lead the user through reflection **one question at a time**. Wait for their answer before moving to the next question. Keep the tone warm and conversational — not clinical.

### Overall Assessment
*"How would you rate this week on a scale of 1–5? What's driving that number?"*

*(Listen. Acknowledge their answer before moving on.)*

### Wins
*"What were your biggest wins this week? Anything you're proud of, even if it felt small."*

### Challenges
*"What didn't go as planned? Where did things get hard?"*

### Patterns (informed by your vault scan from Step 3)
Offer 1–2 observations you noticed in their notes this week, then ask:
*"I noticed [theme] came up a few times this week. Does that resonate? Is there anything there?"*

### Forward Planning
*"What are your top 3 priorities for next week?"*

*"Anything from this week you want to handle differently going forward?"*

---

## Step 6: Generate the Check-In Note

Save to `~/Documents/vault/Reference/Checkins/weekly-checkin-YYYY-MM-DD.md`

```markdown
---
type: weekly-checkin
date: YYYY-MM-DD
week_of: YYYY-WW
created: YYYY-MM-DD HH:MM
rating: [1-5]
tags:
  - weekly-checkin
  - reflection
domains: [personal, professional, projects]
---

# Weekly Check-In — Week of [Date]

## Summary

**Week Rating:** [N]/5 — [User's own words for why]

**In Three Words:** [word], [word], [word]

---

## Wins
- [Win 1 — specific, celebratory]
- [Win 2]
- [Win 3 if applicable]

## Challenges
- [Challenge 1 — honest, clear]
- [Challenge 2]

---

## Patterns This Week

[2–3 paragraphs synthesizing themes you identified from vault scan + user's answers. What kept coming up? What tensions existed? How did the user's energy or thinking evolve over the week? Write this as genuine observation, not a list.]

### Connections to Last Week
[Brief note on continuity — did anything from last week's check-in carry forward? Was there follow-through on stated priorities?]

---

## Domain Snapshot

### Personal
[1–3 sentences on personal life this week — wellbeing, relationships, energy. Drawn from journal entries and user's answers.]

### Professional
[1–3 sentences on work this week — accomplishments, challenges, team dynamics.]

### Projects
[For each active project from MY-PROFILE.md:]

**[Project Name]**
- Progress: [what moved]
- Blockers: [what's in the way]
- Next step: [most important next action]

---

## Task Summary

**Completed this week:** [N] tasks
**Carried over / overdue:** [N] tasks

Notable completions:
- [Task 1]
- [Task 2]

Still open from this week:
- [ ] [Task]
- [ ] [Task]

---

## Intentions for Next Week

**Top 3 priorities:**
1. [Priority 1 — specific and actionable]
2. [Priority 2]
3. [Priority 3]

**What to do differently:**
- [Experiment or change 1]

**What to keep doing:**
- [Practice that's working]

**I'll know next week was successful if:**
- [Measurable or qualitative outcome]
- [Outcome]

---

## Reflections

[2–4 sentences of synthesis. What's the bigger picture? What does this week say about where the user is in their life or work right now? A genuine observation worth sitting with.]

---

*Generated by weekly-checkin skill*
```

---

## Step 7: Confirm Completion

Open the note in Obsidian:
```bash
obsidian-cli open "weekly-checkin-YYYY-MM-DD"
```

Tell the user:
- File is saved and open in Obsidian
- Read back the "Intentions for Next Week" section as a closing summary
- Offer: *"Want to capture any follow-up thoughts via a quick braindump?"*

---

## Conversational Guidelines

### Do:
- Let each answer breathe before asking the next question
- Reflect back what you heard — *"It sounds like [X] was the hardest part"*
- Be honest in pattern observations, even if it surfaces something uncomfortable
- Celebrate wins genuinely and specifically
- Keep the forward-planning section concrete and achievable

### Don't:
- Rush through all questions in one message
- Sugar-coat a hard week
- Write the patterns section as a bullet list — it should read as genuine observation
- Force positivity if the user had a rough week
- Skip the vault scan — context from actual notes makes the pattern section far more valuable

---

## Integration with Other Skills

After the check-in, offer:
- **Braindump** — if the conversation surfaced unresolved thoughts worth capturing
- **Knowledge consolidation** — if themes have recurred for multiple weeks and are worth synthesizing into a framework

---

## Success Criteria
- The conversation felt like a meaningful ritual, not a status update
- Patterns section draws on actual vault content, not just the user's answers
- Priorities for next week are specific and realistic
- User ends the session feeling clearer than when they started
- File is saved and opens in Obsidian automatically
- Total time: 10–15 minutes
