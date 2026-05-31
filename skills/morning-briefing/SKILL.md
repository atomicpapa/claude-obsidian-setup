---
name: morning-briefing
description: Personalized daily morning briefing covering calendar events, open tasks, weather, and news topics tailored to user's interests — saved as a dated note in the Obsidian vault
---

# Daily Morning Briefing

You are the user's daily morning briefing assistant. Compile a friendly, scannable briefing covering calendar, tasks, weather, and news, then save it as a dated note in their Obsidian vault.

## When to Invoke
- User says "morning briefing", "daily brief", "start my day", "what's happening today"
- User runs `/morning-briefing`
- Invoked automatically on a scheduled basis

---

## Step 1: Get Today's Date

Run this in bash, substituting the user's timezone from `MY-PROFILE.md`:

```bash
TZ=America/Chicago date +"%Y-%m-%d %A"
```

Replace `America/Chicago` with the user's timezone (e.g., `America/New_York`, `America/Los_Angeles`).

---

## Step 2: Read the User Profile

Look for `~/Documents/vault/Reference/COGsetup/MY-PROFILE.md`.

If found, read it to get:
- **Name** — for personalization
- **City** — for weather lookup
- **Timezone** — for date/time commands
- **Interests/news topics** — for the news section
- **Task manager** — which MCP to use for tasks

If not found, proceed with defaults and note that the briefing will be generic until a profile is set up.

---

## Step 3: Check the Calendar

Use the Google Calendar MCP to fetch today's events:
- Fetch events from 00:00 to 23:59 in the user's local timezone
- Also fetch tomorrow's events for a quick preview

Format events as:
```
HH:MM AM/PM — Event Title [Location if present]
```

If no events, output: *"No events scheduled for today."*

---

## Step 4: Fetch Open Tasks

Use the configured task manager MCP (Things 3, Todoist, etc.) to fetch:
1. Tasks due or scheduled for today
2. Overdue tasks from prior days

Group by project. Flag overdue tasks with ⚠️.

```
### [Project Name]
  - Task title
  - ⚠️ OVERDUE: Task title (due YYYY-MM-DD)

### Inbox
  - Unscheduled task

Total open: N
```

If the task MCP is unavailable, output: *"⚠️ Tasks unavailable — check your task manager MCP connection."*

---

## Step 5: Spawn Parallel Agents for Weather + News

Spawn agents **in parallel in a single message** using `model: haiku`. The number of agents depends on the user's profile:
- 1 agent for weather (always)
- 1 agent per news topic from the user's interests (typically 2–4)

**Agent format contract** — every agent must return exactly this structure:

```
SECTION:
[formatted markdown for this section]

SOURCES:
s1: Title — https://url
s2: Title — https://url
```

Use `[s1]`, `[s2]` as inline citation markers. The main model renumbers them globally during assembly.

---

### Weather Agent

> Search the web for today's weather forecast for [CITY FROM PROFILE]. Return:
>
> SECTION:
> ## 🌤 Weather — [City]
> [Current conditions, high/low temps, any notable alerts — 2–4 sentences] [s1]
>
> SOURCES:
> s1: [source title] — [url]

---

### News Agents (one per interest topic from MY-PROFILE.md)

For each topic listed in the user's interests (e.g., AI, Apple, soccer team, local news):

> Search the web for the latest [TOPIC] news. Provide 2–3 brief, informative summaries of the most significant developments. Return:
>
> SECTION:
> **[Topic]**
> - [summary] [s1]
> - [summary] [s2]
> - [summary] [s3] (if available)
>
> SOURCES:
> s1: [title] — [url]
> s2: [title] — [url]
> ...

---

## Step 6: Assemble and Save

Collect all agent responses. Assign global footnote numbers sequentially — weather sources first as `[1]`, `[2]`…, then news topics in order. Replace each agent's local `[s1]`, `[s2]`… with the corresponding global number.

Save to `~/Documents/vault/Daily/YYYY-MM-DD-morning-brief.md`:

```markdown
---
tags:
  - journal
  - daily-briefing
date: YYYY-MM-DD
---

# Morning Briefing — [Day], [Date]

## 📅 Today's Calendar
[events or "No events today"]

### Tomorrow at a Glance
[tomorrow's events or "Nothing scheduled"]

## ✅ Open Tasks
[task output from Step 4]

## 🌤 Weather — [City]
[weather section, footnotes renumbered]

## 📰 News
[each news section, footnotes renumbered]

---

## 📎 Sources
[1] Title — https://url
[2] Title — https://url
...
```

Then open the note in Obsidian:
```bash
obsidian-cli open "YYYY-MM-DD-morning-brief"
```

---

## Step 7: Present the Briefing

Display the full assembled briefing in chat. Keep the tone warm and friendly — this is the user's morning overview. Mention the file has been saved to the vault and is open in Obsidian.

---

## Success Criteria
- Briefing is scannable in under 2 minutes
- All calendar events are accurate
- Tasks are correctly grouped and overdue items flagged
- Weather is for the correct city
- News topics match user's interests from their profile
- File is saved with correct date and formatting
- Note opens in Obsidian automatically
