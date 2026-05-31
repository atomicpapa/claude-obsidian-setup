---
name: braindump
description: Quick capture of raw thoughts with intelligent classification into tasks, ideas, concerns, and insights — with automatic task creation and a structured vault note
---

# Braindump

## Purpose
Transform raw, unfiltered thoughts into organized intelligence. Accept whatever the user gives — stream-of-consciousness, voice-to-text, bullet fragments, worried rambling — and turn it into a structured note with clear action items, captured insights, and a brief synthesis.

## When to Invoke
- User says "braindump", "brain dump", "clear my head", "empty my brain"
- User says "I have a lot on my mind", "let me just get this out"
- User seems overwhelmed and needs to externalize their thinking
- User runs `/braindump`

---

## Step 1: Read the User Profile

Look for `~/Documents/vault/Reference/COGsetup/MY-PROFILE.md`.

If found, read it to get:
- **Name** — for personalization
- **Active projects** — to offer as domain options and detect project-specific content
- **Task manager** — to know which MCP to use when creating tasks

If not found, proceed without personalization.

---

## Step 2: Open the Session

Greet the user warmly (use their name if available). Keep it brief — the goal is to get out of the way and let them talk.

Ask:
> *"What's on your mind? Just let it out — ideas, worries, tasks, random thoughts. Any order, any format."*

Accept their input without interruption. It can be long, rambling, formatted or not. Don't judge, filter, or ask clarifying questions during capture.

---

## Step 3: Classify the Content

Once they're done, classify the domain based on content and, if the profile lists active projects, offer project-specific classification:

**If MY-PROFILE.md lists active projects:**
- **Personal** — individual life, relationships, health, home
- **Professional** — work, career, team
- **Project-specific** — related to one of their listed projects
  - Prompt: *"This touches on a few of your projects. Is this mainly about [Project A], [Project B], or mixed?"*
- **Mixed** — spans multiple areas

**If no profile:** Use personal / professional / mixed.

Note the classification — it determines where the file is saved and what tags are applied.

---

## Step 4: Analyze the Content

Apply this analysis framework to the full input:

### Content Inventory
- **Main themes** — 3–5 primary topics
- **Questions raised** — explicit and implicit questions in the content
- **Decisions being contemplated** — choices the user is weighing
- **Emotional tone** — excited, frustrated, anxious, energized, scattered, etc.
- **Energy level** — high / medium / low based on language and urgency

### Categorize Everything
Sort all content into four buckets:

| Category | Definition |
|---|---|
| **Tasks** | Concrete things to do — has a clear action |
| **Ideas** | Things to explore, concepts, possibilities |
| **Concerns** | Worries, risks, things that feel unresolved |
| **Insights** | Realizations, connections, things they figured out |

### Strategic Observations
- **Patterns** — themes that recur or connect across the content
- **Tensions** — places where ideas or concerns conflict
- **Priority signals** — what seems most urgent or important based on the content

---

## Step 5: Create Tasks

For every item in the **Tasks** bucket, create a task in the user's configured task manager MCP (Things 3, Todoist, etc.):
- Use the task title as written in the content (clean up phrasing minimally)
- Assign to Inbox unless the project is obvious from context
- Flag high-urgency tasks if the user's language suggests urgency

Tell the user how many tasks were created.

---

## Step 6: Generate the Braindump Note

Save to `~/Documents/vault/Reference/Braindumps/braindump-YYYY-MM-DD-HHmm-[slug].md`

Where `[slug]` is a 2–3 word kebab-case summary of the main theme (e.g., `work-stress-project`, `house-renovation-ideas`).

```markdown
---
type: braindump
domain: [personal|professional|project-specific|mixed]
project: [project-name]  # only if project-specific
date: YYYY-MM-DD
created: YYYY-MM-DD HH:MM
themes: ["theme1", "theme2", "theme3"]
tags:
  - braindump
  - [domain-tag]
energy_level: [high|medium|low]
emotional_tone: [primary emotion]
---

# Braindump: [Auto-generated descriptive title]

## Raw Capture
[Original user content, preserved exactly as provided]

---

## Analysis

### Main Themes
1. **[Theme]:** [what it is and why it matters]
2. **[Theme]:** [what it is and why it matters]
3. **[Theme]:** [what it is and why it matters]

### Questions Raised
- [Question that deserves further thought]
- [Question that needs an answer]

### Decisions Being Contemplated
- [Decision] — Options: [option A] vs. [option B]

---

## Categorized Content

### ✅ Tasks
- [ ] [Task 1] → *added to [task manager]*
- [ ] [Task 2] → *added to [task manager]*

### 💡 Ideas
- [Idea 1 — brief description]
- [Idea 2 — brief description]

### ⚠️ Concerns
- [Concern 1 — what's worrying them and why]
- [Concern 2]

### 🔍 Insights
- [Insight 1 — what they realized]
- [Insight 2]

---

## Synthesis

[3–5 sentences. What is this person *really* thinking about? Not a list — a genuine read of the underlying themes, tensions, or priorities. Write this as if explaining to a thoughtful friend what's going on for them right now.]

---

## Patterns & Connections
- **Recurring theme:** [what keeps coming up and what it might mean]
- **Tension:** [two things pulling in different directions]
- **Connection to previous thinking:** [[related-note]] *(if applicable)*

---

*Captured by braindump skill*
```

---

## Step 7: Confirm Completion

Open the note in Obsidian:
```bash
obsidian-cli open "braindump-YYYY-MM-DD-HHmm-[slug]"
```

Tell the user:
- File path where the braindump was saved
- How many tasks were created (and in which project if applicable)
- The 3-sentence synthesis from the note — read it back to them
- Offer: *"Want to go deeper on any of this, or does that capture it?"*

---

## Conversational Guidelines

### Do:
- Get out of the way during capture — don't interrupt
- Be genuinely curious in the synthesis
- Acknowledge emotional tone directly and briefly
- Write the synthesis in plain language, not bullet points
- Surface patterns the user might not have seen themselves

### Don't:
- Ask clarifying questions during the capture phase
- Dismiss or minimize concerns
- Turn the synthesis into another list
- Over-analyze — the point is clarity, not a dissertation

---

## Success Criteria
- User feels heard and their thoughts feel organized
- Every task has been captured in their task manager
- The synthesis is accurate and reads like genuine insight
- The categorization matches the actual content
- File is saved and opens in Obsidian automatically
- The whole session takes under 10 minutes
