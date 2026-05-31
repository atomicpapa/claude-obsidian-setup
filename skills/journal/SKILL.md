---
name: journal
description: Guided personal journal entry saved to the Obsidian vault. Draws out reflection through conversation, then writes a structured, narrative entry with a reflection section.
---

# Journal Entry

## Purpose
Create a warm, personal journal entry through guided conversation. This isn't a form to fill out — it's a dialogue that draws out what the user actually wants to capture, then writes it up in a way that will be meaningful to read later.

## When to Invoke
- User says "journal", "journal entry", "write in my journal", "log my day"
- User says "I want to remember this", "make a note about today"
- User describes something that just happened and wants it recorded
- User runs `/journal`

---

## Step 1: Get Today's Date

```bash
TZ=[USER_TIMEZONE] date +"%Y-%m-%d"
```

Read the user's timezone from `~/Documents/vault/Reference/COGsetup/MY-PROFILE.md` if available. Default to UTC if not found.

---

## Step 2: Check for an Existing Entry

Check whether `~/Documents/vault/Journal/YYYY-MM-DD-journal.md` already exists.

- **If it doesn't exist:** Proceed normally.
- **If it already exists:** Tell the user: *"You already have a journal entry for today. Want me to add to it, or start a fresh one for a different topic?"* Wait for their answer before continuing.

---

## Step 3: Open the Conversation

Greet the user warmly. If their name is available from `MY-PROFILE.md`, use it.

Ask an open question to get them started:
- *"What's on your mind today?"*
- *"What do you want to capture from today?"*
- *"What happened that you want to remember?"*

Let them respond freely. Accept voice-to-text, stream-of-consciousness, bullet points — any format.

---

## Step 4: Ask Follow-Up Questions

Based on what they shared, ask **1–2 follow-up questions** to draw out more depth. Choose questions that feel natural given the topic:

**For events/experiences:**
- *"How did that feel in the moment?"*
- *"What surprised you about it?"*
- *"What do you think it means going forward?"*

**For decisions/plans:**
- *"What's driving that decision?"*
- *"What are you most uncertain about?"*
- *"What would success look like?"*

**For observations/ideas:**
- *"Where did that thought come from?"*
- *"What would you do with that if you acted on it?"*

Don't pepper them with all questions at once. Ask one, listen, then ask the next if it still feels useful.

---

## Step 5: Choose a Title

Pick a short, specific title (3–6 words) that describes the subject at a glance. It should be meaningful in a file list without any other context.

**Good examples:**
- `First Day at New Job`
- `Conversation with Dad`
- `Realizing I Need a Break`
- `Big Decision About the House`

**Avoid:**
- Vague titles like `Journal Entry` or `Today's Thoughts`
- Overly long titles

---

## Step 6: Choose Tags

Always include the `journal` tag. Then add 1–3 topic tags that accurately describe the content. Use short, lowercase, hyphenated tags:

**Common tags:**
- `family`, `health`, `work`, `finances`, `relationships`, `travel`
- `ideas`, `decisions`, `reflection`, `goals`
- `[person's name]` — if the entry is primarily about a specific person

Use good judgment — aim for 1–3 tags beyond `journal`. Don't over-tag.

---

## Step 7: Write the Entry

Create `~/Documents/vault/Journal/YYYY-MM-DD-[Title].md`.

**Frontmatter:**
```yaml
---
tags:
  - journal
  - [topic tag(s)]
date: YYYY-MM-DD
---
```

**Entry body — match structure to content:**

**For simple, single-topic entries** (one thing happened, one thing was captured):
Write 2–4 sentences. Clear and direct.

> Had a long overdue conversation with my brother today. We cleared the air about some old tension that's been sitting between us for months. It felt like a weight lifted. Want to keep the momentum and call him again next week.

**For entries with multiple distinct points:**
Open with a brief summary line, then use bullets for details.

> Productive work session — a lot moved forward today:
> - Finally shipped the feature that's been blocking the team for two weeks
> - Had a strong 1:1 with my manager — she flagged me for the upcoming project lead role
> - Realized I need to delegate more if I'm going to take that on

**For entries covering multiple topics:**
Use short `##` subheadings to separate topics.

**Writing guidelines:**
- Past tense, first person implied
- Just the facts plus the emotion — no padding
- If a specific detail is worth remembering (a name, number, date, decision), include it
- Use the minimum structure that makes the entry scannable

**Always end with a `## Reflection` section:**

```markdown
## Reflection

[2–4 sentences. What patterns or themes do you notice? What does this connect to? What might be worth sitting with?]
```

This should come from you, not from the user — offer a genuine observation based on what they shared.

---

## Step 8: Handle Append Case

If the user chose to append to an existing entry, add a `---` divider followed by the new content at the bottom of the existing file. Don't rewrite the existing entry.

---

## Step 9: Confirm

Open the note in Obsidian:
```bash
obsidian-cli open "YYYY-MM-DD-[Title]"
```

Tell the user the file was created (or updated), show the filename, and list the tags used.

---

## Conversational Guidelines

### Do:
- Be warm and genuinely curious
- Let silences breathe — don't rush to the next question
- Reflect back what you heard before asking a follow-up
- Notice emotional tone and acknowledge it gently
- Write the entry in the user's own voice, not a clinical summary

### Don't:
- Ask more than 2 follow-up questions
- Force structure onto free-form thoughts
- Write a generic, padded entry when a direct one would do
- Over-explain the reflection section — keep it honest and brief

---

## Success Criteria
- The conversation felt natural, not like filling out a form
- The entry is something the user will actually want to read later
- The title is specific and meaningful
- Tags are accurate and not excessive
- The reflection section offers a genuine observation
- File is saved and opens in Obsidian automatically
