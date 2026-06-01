# Your AI-Powered Second Brain — Windows Edition
## A Step-by-Step Guide to Building a Claude + Obsidian Life OS

*Version 1.0 — May 2026 | See [Mac version →](Claude-Obsidian-AI-Setup-Guide.md) for the Mac version*

---

## What Is This?

This guide walks you through building a personal AI system that genuinely knows you — your schedule, your notes, your tasks, your home, your life — and can act as a real assistant rather than just a chatbot you ask random questions.

The core idea: instead of AI living in a browser tab, you bring it into your actual life by connecting it to the tools you already use. By the end of this guide, you'll have:

- A personal **knowledge vault** where every note, idea, journal entry, and reference lives
- An AI (Claude) that can **read, write, and search** your vault on your behalf
- Claude connected to your **calendar, email, tasks, and more**
- Automated **daily briefings**, journal workflows, and weekly reviews
- A system that gets smarter the more you use it

This isn't a quick afternoon project — plan on a weekend to get the core layers running, then a few more sessions to customize it to your life. But each layer is independently useful, so you'll be getting value from day one.

---

## What You'll Need

- A **Windows 10 or 11 PC**
- An **Anthropic account** with API access or a Claude Pro/Max subscription
- About **$20/month** for Claude usage (API) or a Pro/Max subscription
- Willingness to spend time in PowerShell (it's not as scary as it sounds — we'll go step by step)

---

## A Note on Windows Paths

On Windows, your home folder is at `C:\Users\YourName\`. In PowerShell, you can use `$env:USERPROFILE` as a shortcut for this. So where the Mac guide says `~/Documents/vault`, on Windows that means `$env:USERPROFILE\Documents\vault` or simply `C:\Users\YourName\Documents\vault`.

Throughout this guide, we'll use PowerShell-style paths.

---

## Step 0: Set Up Your Tools

Before starting, install these foundations. We'll use **winget** — Windows' built-in package manager (available on Windows 10 1709+ and all Windows 11).

Open **PowerShell as Administrator** (search "PowerShell" in the Start menu, right-click → "Run as administrator") and run:

```powershell
# Install Windows Terminal (much better than the default terminal)
winget install Microsoft.WindowsTerminal

# Install Git (needed for version control)
winget install Git.Git

# Install Node.js (needed for Claude Code)
winget install OpenJS.NodeJS.LTS

# Install Obsidian
winget install Obsidian.Obsidian
```

After installing, **close and reopen PowerShell** (or Windows Terminal) so the new tools are recognized.

Verify Node.js installed correctly:
```powershell
node --version
npm --version
```
Both should print version numbers. If not, restart your terminal and try again.

---

# Layer 1 — Obsidian: Your Vault

## What Obsidian Is (and Why It's the Foundation)

Obsidian is a free note-taking app that stores everything as plain text files (Markdown) on your own computer. Unlike Notion, Evernote, or OneNote, your notes never go to someone else's server — they live in a folder on your PC called your **vault**.

Why this matters for an AI system: because Claude can read and write files on your computer, Obsidian becomes the shared memory between you and your AI. You write notes in Obsidian; Claude can read them, add to them, search across them, and create new ones. It's the foundation everything else builds on.

Obsidian also has a powerful linking system — you can link any note to any other note using `[[double brackets]]`, creating a web of connected knowledge. Over time your vault becomes a true second brain.

## Step 1: Launch Obsidian

If you installed via winget above, launch Obsidian from the Start menu. Alternatively download it from [obsidian.md](https://obsidian.md).

## Step 2: Create Your Vault

When Obsidian opens, it will ask you to create or open a vault.

1. Click **"Create new vault"**
2. Give it a name — `vault` works well (no spaces is best)
3. Choose where to save it — `Documents\vault` is a good spot
4. Click **Create**

Your vault is just a regular Windows folder. You can open it in File Explorer anytime.

## Step 3: Understand the Core Concepts

**Notes** are Markdown files (`.md`). Markdown is simple text formatting — `# Heading`, `**bold**`, `- bullet point`. Obsidian renders these beautifully, but underneath they're plain text files.

**Wikilinks** (`[[Note Name]]`) link any note to any other note. Type `[[` and start typing — Obsidian autocompletes. This turns your vault into a web of ideas rather than a pile of files.

**Frontmatter** is optional metadata at the top of a note:
```yaml
---
type: journal
created: 2026-05-31
tags: [health, fitness]
---
```
This lets you filter and query notes later.

## Step 4: Design Your Folder Structure

Here's a recommended starting structure — adapt it to your life:

```
vault\
├── Daily\              # Daily notes (one per day)
├── Monthly\            # Monthly summaries
├── Journal\            # Personal journal entries
├── Reference\          # Clippings, research, reference material
│   ├── Braindumps\     # Quick capture of raw thoughts
│   ├── Research\       # Topic deep-dives
│   └── Tech\           # Tech notes and guides
├── Wiki\               # Your personal wiki (organized by domain)
│   ├── Health\         # Health tracking, providers, medications
│   ├── Projects\       # Ongoing projects
│   └── People\         # Notes on people in your life
├── Templates\          # Note templates
└── Attachments\        # Images and files
```

To create folders: right-click in Obsidian's left sidebar → **"New folder"**.

## Step 5: Opening Notes Automatically (Windows Alternative to obsidian-cli)

On Mac there's a tool called `obsidian-cli` for automatically opening notes. On Windows, we use **Obsidian's URI scheme** instead — a built-in feature that opens any note directly.

Save this as a PowerShell script at `$env:USERPROFILE\Scripts\obsidian-open.ps1`:

```powershell
param([string]$NoteName)
$encoded = [System.Uri]::EscapeDataString($NoteName)
Start-Process "obsidian://open?vault=vault&file=$encoded"
```

Then whenever Claude needs to open a note, it can run:
```powershell
powershell -File "$env:USERPROFILE\Scripts\obsidian-open.ps1" "Note Name"
```

Add a note about this in your `CLAUDE.md` (covered in Layer 3) so Claude knows to use this command on Windows.

## Step 6: Recommended Community Plugins

Go to **Settings → Community plugins → Browse** in Obsidian. Turn off "Safe mode" when prompted.

**Essential:**
- **Templater** — powerful templates for new notes
- **Calendar** — calendar view for daily notes
- **Dataview** — query your vault like a database

**Recommended:**
- **Tasks** — enhanced task tracking
- **Tag Wrangler** — manage tags across your vault

---

# Layer 2 — Claude Code: Your AI

## What Claude Code Is (and How It's Different)

Claude Code is Anthropic's **command-line AI tool** — it runs in PowerShell and has the ability to read files, write files, run commands, and connect to external services. This is fundamentally different from Claude in a web browser, which can only see what you paste into the chat.

When you run Claude Code from inside your vault folder, Claude can read your notes, create new ones, search across everything, and connect to your calendar, email, and more.

## Step 1: Get API Access

Claude Code requires either:

**Option A — API Key (pay per use)**
1. Go to [console.anthropic.com](https://console.anthropic.com)
2. Create an account and add billing
3. Go to **API Keys** and create a new key
4. Save this key somewhere safe — you'll only see it once

**Option B — Claude Max Subscription**
1. Go to [claude.ai](https://claude.ai) and subscribe to Max ($100/month)
2. Claude Code is included and uses a different billing model
3. Better value if you use Claude heavily

## Step 2: Install Claude Code

In PowerShell (regular, not administrator):

```powershell
npm install -g @anthropic-ai/claude-code
```

Start Claude Code:
```powershell
claude
```

When prompted, enter your API key or log in with your Claude account.

## Step 3: Open Your Vault as the Working Directory

Always launch Claude Code from inside your vault folder:

```powershell
cd "$env:USERPROFILE\Documents\vault"
claude
```

**Add a shortcut** — open your PowerShell profile to edit it:
```powershell
notepad $PROFILE
```

If it says the file doesn't exist, create it:
```powershell
New-Item -Path $PROFILE -ItemType File -Force
notepad $PROFILE
```

Add this function to the file:
```powershell
function vault {
    Set-Location "$env:USERPROFILE\Documents\vault"
    claude
}
```

Save and close. Now reload your profile:
```powershell
. $PROFILE
```

From now on, just type `vault` in any PowerShell window to launch Claude in your vault.

## Step 4: Your First Conversations

Once Claude Code is running inside your vault:

```
Search my vault for any notes about [topic] and summarize what I have
```
```
Create a new journal entry for today reflecting on [something that happened]
```
```
Look at my Daily\ folder and tell me what I've been thinking about this week
```

---

# Layer 3 — CLAUDE.md: Teaching Claude Who You Are

## What CLAUDE.md Is (and Why It's the Secret Sauce)

Every folder that Claude Code works in can have a `CLAUDE.md` file — a plain text instruction file Claude reads at the start of every conversation. This is how you teach Claude about *your specific setup*, preferences, naming conventions, and life context.

Without `CLAUDE.md`, Claude gives generic answers. With it, Claude knows your folder structure, your timezone, how you like things written, and what tools are available.

## Step 1: Create Your CLAUDE.md

Create a file called `CLAUDE.md` in the root of your vault (the top-level folder). In Obsidian's sidebar, right-click → **New note** → name it `CLAUDE.md`.

## Step 2: Fill In Your CLAUDE.md

Here's a Windows-specific template:

```markdown
# CLAUDE.md — My Vault

> **IMPORTANT — Timezone:** I am in [YOUR CITY] ([YOUR TIMEZONE]).
> Always use this timezone for dates and times. Never use UTC.
> To get today's date in PowerShell: [System.TimeZoneInfo]::ConvertTimeBySystemTimeZoneId(
>   [DateTime]::UtcNow, "[YOUR_WINDOWS_TIMEZONE]").ToString("yyyy-MM-dd")

> **IMPORTANT — Open notes after creating:** After creating any note,
> open it in Obsidian using this PowerShell command:
> powershell -File "$env:USERPROFILE\Scripts\obsidian-open.ps1" "Note Name Without Extension"

> **IMPORTANT — Windows paths:** Use backslashes for file paths.
> Vault root is at: C:\Users\[USERNAME]\Documents\vault\

## About This Vault

This is my personal second-brain and knowledge management system.
It contains my journal, reference notes, project tracking, and personal wiki.

## About Me

- **Location:** [Your city, state]
- **Occupation:** [Your job/role]
- **Key interests:** [Things you want Claude to know about]
- **Family context:** [Whatever is relevant]

## Vault Structure

[Paste your folder structure here with descriptions of what goes where]

## File Conventions

- **Dated notes:** YYYY-MM-DD Description.md in vault root
- **Monthly notes:** YYYY-MM.md in Monthly\
- Use Obsidian wikilinks [[Like This]] for internal links
- Include YAML frontmatter on structured notes

## Tone and Style

- Keep responses conversational but concise
- Use Obsidian-flavored markdown (wikilinks, callouts, frontmatter)
- Ask before making big structural changes

## MCP Servers Available

[Fill this in as you add MCP servers in Layer 4]
```

**Finding your Windows timezone name** (for the date command above):
```powershell
[System.TimeZoneInfo]::GetSystemTimeZones() | Select-Object Id, DisplayName | Where-Object DisplayName -like "*Central*"
```
Replace `Central` with your region. Use the `Id` value (e.g., `"Central Standard Time"`).

---

# Layer 4 — MCP Servers: Connecting Claude to Your Life

## What MCP Servers Are (and What Becomes Possible)

MCP (Model Context Protocol) lets Claude connect to external services — your calendar, email, tasks, smart home, and more. Once connected, you can say things like:
- *"What's on my calendar tomorrow?"*
- *"Search my email for the invoice from last month"*
- *"Add 'call the dentist' to my tasks for Tuesday"*

MCP servers are configured in `%USERPROFILE%\.claude\mcp.json`.

## How to Edit mcp.json

Open PowerShell and run:
```powershell
# Create the .claude folder if it doesn't exist
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.claude"

# Create mcp.json if it doesn't exist
if (-not (Test-Path "$env:USERPROFILE\.claude\mcp.json")) {
    '{"mcpServers": {}}' | Out-File "$env:USERPROFILE\.claude\mcp.json" -Encoding utf8
}

# Open it for editing
notepad "$env:USERPROFILE\.claude\mcp.json"
```

The structure looks like this:
```json
{
  "mcpServers": {
    "server-name": {
      "command": "npx",
      "args": ["-y", "some-mcp-package"],
      "env": {
        "API_KEY": "your-key-here"
      }
    }
  }
}
```

---

## MCP Server 1: Obsidian Vault

**What it does:** Lets Claude read and write your vault notes through Obsidian's own API.

**Setup:**
1. In Obsidian, go to **Settings → Community plugins → Browse**
2. Search for **"Local REST API"** and install + enable it
3. In its settings, note the **API key** it generates
4. Add this to your `mcp.json`:

```json
"obsidian-vault": {
  "command": "npx",
  "args": ["-y", "mcp-obsidian"],
  "env": {
    "OBSIDIAN_API_KEY": "your-api-key-here",
    "OBSIDIAN_HOST": "localhost",
    "OBSIDIAN_PORT": "27123"
  }
}
```

**Test it:** Start Claude Code in your vault and ask: *"List the files in my Daily\ folder."*

---

## MCP Server 2: Google Calendar

**What it does:** Lets Claude read your calendar events and create new ones.

**Setup:**
```powershell
claude mcp add --transport http google-calendar https://mcp.claude.ai/mcp/google-calendar
```
A browser window will open — authorize with your Google account.

**Once connected, try:**
- *"What do I have going on this week?"*
- *"Schedule a dentist appointment for next Tuesday at 2pm"*

---

## MCP Server 3: Gmail

**What it does:** Lets Claude search your email and create draft replies.

**Setup:**
```powershell
claude mcp add --transport http gmail https://mcp.claude.ai/mcp/gmail
```
Authorize with your Google account when prompted.

**Once connected, try:**
- *"Search my email for any messages from [sender] in the last week"*
- *"Draft a reply to the last email from [person]"*

Note: Claude drafts emails — it never sends without you approving.

---

## MCP Server 4: Task Manager

Things 3 is Mac-only. Here are the best Windows alternatives:

### Option A — Todoist (Recommended for Windows)

Todoist has a well-maintained MCP server and works on Windows, Mac, iPhone, and Android.

1. Create a free account at [todoist.com](https://todoist.com)
2. Go to **Settings → Integrations → Developer** and create an API token
3. Add to `mcp.json`:

```json
"todoist": {
  "command": "npx",
  "args": ["-y", "@abhiz123/todoist-mcp-server"],
  "env": {
    "TODOIST_API_TOKEN": "your-api-token-here"
  }
}
```

### Option B — Microsoft To Do

If you're already in the Microsoft ecosystem:
```json
"microsoft-todo": {
  "command": "npx",
  "args": ["-y", "mcp-microsoft-todo"],
  "env": {
    "MS_CLIENT_ID": "your-client-id",
    "MS_CLIENT_SECRET": "your-client-secret"
  }
}
```
This requires registering an app in Azure — see the [mcp-microsoft-todo docs](https://github.com/search?q=mcp-microsoft-todo) for the full setup.

**Once connected (Todoist), try:**
- *"What tasks do I have due today?"*
- *"Add 'pick up dry cleaning' to my inbox"*
- *"Show me everything in my Work project"*

---

## MCP Server 5: Home Assistant *(Optional)*

**What it does:** Connects Claude to your smart home.

**Prerequisites:** Home Assistant running on your network.

**Setup:**
1. In Home Assistant, go to **Settings → Users → [Your User] → Long-Lived Access Tokens** and create a token
2. Add to `mcp.json`:

```json
"home-assistant": {
  "command": "npx",
  "args": ["-y", "mcp-home-assistant"],
  "env": {
    "HASS_HOST": "http://homeassistant.local:8123",
    "HASS_TOKEN": "your-long-lived-token-here"
  }
}
```

---

## MCP Server 6: Web Search *(Optional)*

Gives Claude the ability to search the web for current information:

```json
"searxng": {
  "command": "npx",
  "args": ["-y", "mcp-searxng"],
  "env": {
    "SEARXNG_URL": "https://searxng.instance.example.com"
  }
}
```

Find a public SearXNG instance at [searx.space](https://searx.space).

---

## Verifying Your MCP Setup

Restart Claude Code and run:
```
/mcp
```
Claude will show all configured servers and whether they're connected. If any show errors, double-check the config in `mcp.json` — usually it's a typo or a missing API key.

---

# Layer 5 — Skills: Custom Workflows

## What Skills Are (and How They Work)

Skills are instruction files that teach Claude how to perform a specific repeatable workflow. Invoke a skill once and Claude follows the same detailed process every time — no re-explaining required.

Skills live in `%USERPROFILE%\.claude\skills\`. Each skill is a Markdown file.

## Creating the Skills Folder

```powershell
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.claude\skills"
```

## Your Most Useful Skills to Build

### Skill 1: Daily Morning Briefing

Save as `%USERPROFILE%\.claude\skills\morning-briefing` (no file extension):

```markdown
# Daily Morning Briefing

When this skill is invoked, perform the following steps in order:

1. **Get today's date** using PowerShell:
   [System.TimeZoneInfo]::ConvertTimeBySystemTimeZoneId(
     [DateTime]::UtcNow, "YOUR_WINDOWS_TIMEZONE").ToString("yyyy-MM-dd")
2. **Check the calendar** — list today's and tomorrow's events from Google Calendar
3. **Check tasks** — list tasks due today from Todoist
4. **Check the weather** — search for today's weather in [YOUR CITY]
5. **Scan recent notes** — look at any notes created in the vault in the last 24 hours
6. **Generate briefing** — create a concise briefing note saved to
   Daily\YYYY-MM-DD-morning-brief.md
7. **Open the note** in Obsidian using:
   powershell -File "$env:USERPROFILE\Scripts\obsidian-open.ps1" "YYYY-MM-DD-morning-brief"

Format with clear sections: Calendar | Tasks | Weather | Recent Notes
Keep it scannable — this is a morning read, not an essay.
```

**To invoke it:** Type `/morning-briefing` in Claude Code, or say *"run my morning briefing"*.

### Skill 2: Journal Entry

Save as `%USERPROFILE%\.claude\skills\journal`:

```markdown
# Journal Entry

When this skill is invoked:

1. Get today's date using PowerShell
2. Check if a journal entry already exists for today in Journal\
3. Ask me: "What do you want to journal about today?"
4. Listen to my response, then ask 1-2 follow-up questions to draw out more detail
5. Create a journal entry in Journal\YYYY-MM-DD-journal.md with:
   - YAML frontmatter (type: journal, date, tags)
   - A narrative summary of what I shared
   - A "Reflection" section with any patterns or themes you notice
6. Open the note using the obsidian-open.ps1 script

Keep the tone warm and personal. This is a private journal, not a report.
```

### Skill 3: Braindump

Save as `%USERPROFILE%\.claude\skills\braindump`:

```markdown
# Braindump

When this skill is invoked, help me empty my brain:

1. Ask: "What's on your mind? Dump everything — ideas, worries, tasks, random thoughts."
2. Let me talk (or type) without interruption
3. When I'm done, organize what I shared into:
   - **Tasks** (things to do)
   - **Ideas** (things to explore)
   - **Concerns** (things worrying me)
   - **Insights** (things I realized)
4. Create tasks in Todoist for anything actionable
5. Save the full braindump to Reference\Braindumps\YYYY-MM-DD-braindump.md
6. Give me a 3-sentence summary of what I'm really thinking about

This should feel like a conversation, not a form.
```

### Skill 4: Weekly Check-In

Save as `%USERPROFILE%\.claude\skills\weekly-checkin`:

```markdown
# Weekly Check-In

When this skill is invoked:

1. Get the current date and calculate the past 7 days
2. Scan the vault for notes, journal entries, and braindumps from the past week
3. Check Todoist for completed and overdue tasks from this week
4. Ask me 3 reflection questions (one at a time):
   - "What were your biggest wins this week?"
   - "What didn't go as planned?"
   - "What's the most important thing for next week?"
5. Synthesize everything into a weekly check-in note:
   - Save to Monthly\YYYY-WW-weekly-checkin.md
   - Include: What happened | Wins | Challenges | Intentions for next week
6. Open the note in Obsidian

This is a 10-minute weekly ritual, not a performance review.
```

## How to Create Skill Files on Windows

In PowerShell, create a skill like this:
```powershell
notepad "$env:USERPROFILE\.claude\skills\morning-briefing"
```
Notepad will ask if you want to create the file — click Yes. Paste your skill content and save.

## How to Invoke Skills

- **Slash command:** `/morning-briefing`, `/journal`, `/braindump`
- **Natural language:** Just describe what you want

---

# Layer 6 — Plugins: Superpowers for Claude Code

## What Plugins Add

Plugins are pre-built packages that extend Claude Code with new slash commands and workflows — maintained by Anthropic and the community. They work identically on Windows and Mac.

Browse available plugins by typing `/plugins` inside Claude Code.

## The Most Useful Plugins

### Superpowers (Official — Highly Recommended)

```powershell
claude plugins install superpowers@claude-plugins-official
```

Adds structured reasoning workflows:
- `/brainstorming` — turns vague ideas into detailed plans before doing anything
- `/writing-plans` — creates step-by-step plans for complex projects
- `/systematic-debugging` — structured debugging when something isn't working

Without Superpowers, Claude jumps straight into action. With it, you get a thoughtful planning phase first.

### Skill Creator (Official)

```powershell
claude plugins install skill-creator@claude-plugins-official
```

Adds `/skill-creator` — helps you build skills through a guided conversation. Instead of writing skill files from scratch, describe what you want and it writes the skill for you. Highly recommended for growing your skill library.

### Context7 (Official)

```powershell
claude plugins install context7@claude-plugins-official
```

Gives Claude access to current documentation for software tools and libraries. Invaluable when asking Claude to help with any technical setup.

### Claude Memory (Community)

```powershell
claude plugins install claude-mem@thedotmack
```

Adds persistent memory — Claude saves facts about you that carry across conversations.

## Version-Controlling Your Vault with Git *(Recommended)*

Turn your vault into a Git repository for full change history and backup:

```powershell
cd "$env:USERPROFILE\Documents\vault"
git init
git add .
git commit -m "Initial vault setup"
```

Push to a private GitHub repository for offsite backup:
```powershell
gh repo create my-vault --private --source . --push
```
(Requires the GitHub CLI: `winget install GitHub.cli`)

---

# Putting It All Together: A Day in the Life

**Morning (5 minutes)**
You open Windows Terminal, type `vault`, and run `/morning-briefing`. Claude checks your calendar, scans your tasks, looks at the weather, glances at any notes from yesterday, and generates a clean morning brief that opens in Obsidian automatically. You read it over coffee.

**Throughout the day**
A thought occurs to you. You open Terminal and type:
*"Quick note: I want to research [topic] for [project]. Add a task to look into this."*
Claude creates a note in your vault and adds the task to Todoist. You never lose the thought.

**Evening (10 minutes)**
You run `/journal` and tell Claude how your day went. It asks a couple of follow-up questions, then writes a reflective journal entry and saves it to your vault.

**Weekly**
You run `/weekly-checkin`. Claude scans everything you wrote this week, asks you three reflection questions, and creates a synthesis note.

---

# Maintenance and Growth

## Keep CLAUDE.md Updated

Every time you add a new MCP server, change your folder structure, or discover Claude keeps making the same mistake — update your `CLAUDE.md`. It's the most important thing you can do to keep the system working well.

## Grow Your Skills Gradually

Start with the morning briefing and journal skills, use them for two weeks, then add the next one. Iterate based on what feels off.

## Review and Prune Your Vault

Once a month, look at what Claude has been creating. Delete what isn't working, reorganize what doesn't fit. The vault should serve you — not the other way around.

---

# Windows Troubleshooting

**"npm is not recognized"**
→ Node.js didn't add itself to PATH. Restart PowerShell. If still broken, go to System Properties → Environment Variables and add `C:\Program Files\nodejs\` to your PATH.

**"claude is not recognized"**
→ Same PATH issue. The global npm bin folder needs to be in PATH. Run `npm config get prefix` to find it, then add that path + `\` to your system PATH.

**PowerShell script execution blocked**
→ Run this once as Administrator: `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser`

**obsidian-open.ps1 not working**
→ Make sure the `Scripts` folder exists: `New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\Scripts"` and that you saved the script there correctly.

**mcp.json changes not taking effect**
→ Restart Claude Code completely (Ctrl+C, then `claude` again). Changes to `mcp.json` aren't hot-reloaded.

---

## Resources

- **Obsidian documentation:** [help.obsidian.md](https://help.obsidian.md)
- **Obsidian community forum:** [forum.obsidian.md](https://forum.obsidian.md)
- **Claude Code documentation:** [docs.anthropic.com/claude-code](https://docs.anthropic.com/claude-code)
- **MCP documentation:** [modelcontextprotocol.io](https://modelcontextprotocol.io)
- **Anthropic API console:** [console.anthropic.com](https://console.anthropic.com)
- **Windows Terminal:** [aka.ms/terminal](https://aka.ms/terminal)
- **winget documentation:** [learn.microsoft.com/windows/package-manager](https://learn.microsoft.com/windows/package-manager)

---

*This guide describes a living system — expect to iterate, experiment, and adapt it to your own life. The version that works best for you won't look exactly like anyone else's setup. That's the point.*
