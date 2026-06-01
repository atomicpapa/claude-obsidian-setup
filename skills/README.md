# Example Skills

These are ready-to-use skills for Claude Code. Each skill is a folder containing a `SKILL.md` instruction file — this is the format Claude Code expects.

Copy any skill folder to `~/.claude/skills/` (Mac) or `%USERPROFILE%\.claude\skills\` (Windows) to install it. The folder name becomes the slash command. For example, the `journal/` folder becomes `/journal` inside Claude Code.

## Included Skills

| Skill | Command | What It Does |
|---|---|---|
| `morning-briefing/` | `/morning-briefing` | Daily briefing: calendar, tasks, weather, and news tailored to your interests |
| `journal/` | `/journal` | Guided journal entry — draws out reflection through conversation, then writes a structured note |
| `braindump/` | `/braindump` | Empties your brain — captures everything, classifies into tasks/ideas/concerns/insights, creates tasks automatically |
| `weekly-checkin/` | `/weekly-checkin` | Weekly reflection — synthesizes your vault notes, identifies patterns, sets intentions for next week |
| `wiki-create/` | `/wiki-create` | Scaffold a new LLM-maintained wiki domain with full folder structure, schema, index, log, and timeline |
| `wiki-ingest/` | `/wiki-ingest` | Ingest a source note into a wiki — proposes pages to create/update, writes after approval, maintains index and log |
| `wiki-lint/` | `/wiki-lint` | Audit a wiki for broken links, orphan pages, ghost index entries, unresolved flags, and frontmatter gaps |

## Installation

**Mac:**
```bash
cp -r skills/* ~/.claude/skills/
```

**Windows (PowerShell):**
```powershell
Copy-Item -Recurse skills\* "$env:USERPROFILE\.claude\skills\"
```

## Customizing

Each `SKILL.md` is plain Markdown — open it in any text editor and adjust:

- **morning-briefing:** Set your city, timezone, and news topics
- **journal/braindump/weekly-checkin:** Update folder paths if your vault structure differs from the guide
- **wiki-create/wiki-ingest/wiki-lint:** These work with any wiki domain — no customization needed to get started
- **All skills:** Adjust tone, output format, or steps to match your preferences

Skills read from `~/Documents/vault/Reference/COGsetup/MY-PROFILE.md` when available — creating this file with your name, city, timezone, active projects, and interests unlocks personalization across all skills. See the main guide for details.

## User Profile

Skills become more powerful when you create a user profile. Create `~/Documents/vault/Reference/COGsetup/MY-PROFILE.md` with content like:

```markdown
# My Profile

- **Name:** [Your name]
- **City:** [Your city]
- **Timezone:** [e.g., America/Chicago]
- **Occupation:** [Your role]
- **Active projects:** [Project 1], [Project 2]
- **News interests:** [Topic 1], [Topic 2], [Topic 3]
- **Task manager:** [Things 3 / Todoist / etc.]
```

The best skills are ones you've tuned to your own habits. Use these as a starting point.
