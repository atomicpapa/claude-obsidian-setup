# Example Skills

These are ready-to-use skill files for Claude Code. Copy any of them to `~/.claude/skills/` (Mac) or `%USERPROFILE%\.claude\skills\` (Windows) to install them.

Skills have no file extension — the filename is the slash command. For example, the `journal` file becomes `/journal` inside Claude Code.

## Included Skills

| Skill | Command | What It Does |
|---|---|---|
| `morning-briefing` | `/morning-briefing` | Daily briefing: calendar, tasks, weather, recent notes |
| `journal` | `/journal` | Guided journal entry saved to your vault |
| `braindump` | `/braindump` | Brain-emptying session that captures tasks, ideas, and insights |
| `weekly-checkin` | `/weekly-checkin` | Weekly reflection and synthesis note |

## Installation

**Mac:**
```bash
cp skills/* ~/.claude/skills/
```

**Windows (PowerShell):**
```powershell
Copy-Item skills\* "$env:USERPROFILE\.claude\skills\"
```

## Customizing

Each skill file is plain Markdown — open it in any text editor and adjust:
- Replace `[YOUR CITY]` with your actual city in `morning-briefing`
- Change folder paths if your vault structure differs from the guide
- Adjust the tone, format, or steps to match your preferences

The best skills are ones you've tuned to your own habits. Use these as a starting point.
