---
name: braindump
description: Guided GTD-style mind sweep that clears your head through conversational domain prompts — captures tasks, ideas, concerns, and insights, then saves a clean note and creates tasks in your task manager
---

# Braindump — Mind Sweep

A guided brain-clearing session modeled on the GTD Weekly Review mind sweep. The goal is to pull everything out of your head — including things lurking in the back — through targeted trigger questions across life domains. Work through one domain at a time. Ask one question, listen, follow up if something needs drawing out, then move on.

## When to Invoke
- User says "braindump", "brain dump", "clear my head", "mind sweep"
- User wants to empty their head before a weekly review
- User feels scattered or overwhelmed and wants to offload

---

## Pre-Flight

Get today's date:
```bash
TZ=[USER_TIMEZONE] date +"%Y-%m-%d %H:%M"
```

Read `~/Documents/vault/Reference/COGsetup/MY-PROFILE.md` to get:
- User's name — for personalization
- Family members — to use as domain triggers (partner, kids, parents, etc.)
- Pets — to include as a trigger domain if listed
- Active projects — to prompt specifically during the Work domain
- Task manager — which MCP to use for task creation

---

## Step 1: Opening Anchor

Start with this question — or a natural variation of it:

> *"Is there one thing standing out as most important, or sitting at the front of your mind right now?"*

Let them respond freely. If it's big, acknowledge it briefly. If it leads somewhere naturally, follow it briefly — but don't let one thread take over the whole session. The goal is breadth first.

---

## Step 2: Domain Sweep

Work through each domain below in order. For each one:
- Ask the trigger question
- Wait for the response
- Ask one follow-up if something sounds unfinished or worth pulling on
- Capture everything — don't evaluate, just collect
- Move on

**Skip any domain already covered** in Step 1 or an earlier domain.

---

### 💼 Work & Projects
*"Anything at work sitting unfinished, or on your mind that you haven't dealt with yet?"*

If active projects are listed in MY-PROFILE.md, prompt specifically:
*"What about [Project Name] — anything there?"*

---

### 🏠 House & Home
*"Anything going on with the house? Repairs, maintenance, something you've been meaning to do or call someone about?"*

---

### 🐾 Pets
*(Include only if pets are listed in MY-PROFILE.md)*
*"How are [pet names] doing? Anything you've noticed or been meaning to take care of?"*

---

### 👵 Aging Parent / Caregiver Responsibilities
*(Include only if caregiving context is in MY-PROFILE.md)*
*"Anything on your mind with [parent's name] — care, appointments, how they're doing?"*

---

### 👨‍👩‍👧‍👦 Kids
*(Include only if children are listed in MY-PROFILE.md)*
*"How are the kids doing? Anything going on with [names] you've been thinking about?"*

---

### ❤️ Partner / Spouse
*(Include only if partner is listed in MY-PROFILE.md)*
*"Anything going on with [partner's name], or anything you've been meaning to do or talk to them about?"*

---

### 💪 Health & Fitness
*"How are you doing physically? Any health stuff — appointments, how the fitness routine is going, anything you've been putting off?"*

---

### 💻 Tech & Side Projects
*"Anything in your tech projects or side work nagging at you — something broken, something you want to build, or something you've been meaning to get to?"*

---

### 💰 Finances
*"Anything financial sitting in the back of your mind — bills, accounts, something you need to look into or act on?"*

---

### ✍️ Creative & Learning
*"Anything on the creative or learning side — a project, an idea you don't want to lose, something you've been meaning to get back to?"*

---

### 🧠 Open Net
*"Anything else floating around that we haven't touched on? Could be anything — commitments you've made, things you're waiting on, ideas you don't want to lose, something that's been bothering you."*

---

## Step 3: Categorize Together

Once the sweep is done, read back everything captured and sort it into four buckets:

| Bucket | What goes here |
|---|---|
| ✅ **Tasks** | Concrete actions — something to do, call, schedule, or decide |
| 💡 **Ideas** | Things to explore, possibilities, not yet actionable |
| ⚠️ **Concerns** | Worries, unresolved things, things weighing on you |
| 🔍 **Insights** | Realizations, patterns, things you figured out |

For anything that sounds like a task, confirm: *"Do you want me to add that to [task manager]?"* Create tasks in real time as confirmed — don't batch them for the end.

---

## Step 4: Create Tasks

For each confirmed task, use the configured task manager MCP with:
- `title`: the action, plainly stated
- `project`: if the domain makes it obvious (Health, Home, Family, etc.)
- `deadline`: if a date was mentioned
- `notes`: any context from the conversation

---

## Step 5: Save the Note

Save to `~/Documents/vault/Reference/Braindumps/braindump-YYYY-MM-DD-HHmm.md`:

```markdown
---
type: braindump
date: YYYY-MM-DD
created: YYYY-MM-DD HH:MM
tags:
  - braindump
---

# Braindump — [YYYY-MM-DD]

## What's Captured

### ✅ Tasks
- [task] → *added to [task manager]*
- [task] → *added to [task manager]*

### 💡 Ideas
- [idea]

### ⚠️ Concerns
- [concern]

### 🔍 Insights
- [insight]

---

## Raw Notes
[Everything shared, loosely organized by domain, in the user's own words]

---

## Synthesis

[2–4 sentences. What's actually going on right now? What themes recur, what's underneath the surface? Write this as a genuine read, not a bullet-point summary.]
```

Open the note in Obsidian:
```bash
obsidian-cli open "braindump-YYYY-MM-DD-HHmm"
```

---

## Step 6: Close

Tell the user:
- How many tasks were created and in which projects
- The file is saved and open in Obsidian
- Read the synthesis back — 2–4 sentences, honest and direct

Then offer:
*"Want to add anything else, or does that feel like everything?"*

---

## Tone & Flow

- **One question at a time.** Never stack multiple prompts in one message.
- **Follow the thread briefly, then move on.** One follow-up per domain max — depth is what other skills are for.
- **Don't judge or evaluate** what's shared. Just capture.
- **Be warm but efficient.** This isn't therapy — it's a sweep. Keep it moving.
- **Skip domains that feel truly empty.** If the answer is "nothing there," move on without pressing.
- **The synthesis should be honest.** If the session surfaced something heavy, name it. Don't soften it into a summary.
- **Adapt domain triggers to the user's life.** The MY-PROFILE.md tells you who matters to them — use those names and relationships, not generic placeholders.

---

## Customizing Your Trigger List

The domain sweep above is a starting point. Add, remove, or rename domains to match your life. Common additions:

- **Faith / Spiritual** — anything in this area you've been neglecting or thinking about?
- **Social / Friendships** — anyone you've been meaning to reach out to?
- **Learning** — anything you've been wanting to study or a course you haven't started?
- **Travel / Upcoming events** — anything coming up you haven't prepared for?

The best trigger list is the one that consistently surfaces things you'd otherwise forget.

---

## Success Criteria
- User feels lighter after the session — head is clear
- Nothing important got left behind
- Every actionable item is in the task manager
- The note is a faithful record of the session
- The synthesis captures what's really going on, not just what was said
