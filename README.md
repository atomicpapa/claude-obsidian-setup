# Your AI-Powered Second Brain
## A Step-by-Step Guide to Building a Claude + Obsidian Life OS

*Version 1.0 — May 2026 | [Windows version →](README-Windows.md)*

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

- A **Mac** (this guide is Mac-focused; Windows/Linux work but some steps differ)
- An **Anthropic account** with API access or a Claude Pro/Max subscription
- About **$20/month** for Claude usage (API) or a Pro/Max subscription
- Willingness to spend time in the terminal (it's not as scary as it sounds — we'll go step by step)

---

# Layer 1 — Obsidian: Your Vault

## What Obsidian Is (and Why It's the Foundation)

Obsidian is a free note-taking app that stores everything as plain text files (Markdown) on your own computer. Unlike Notion, Evernote, or Apple Notes, your notes never go to someone else's server — they live in a folder on your Mac called your **vault**.

Why this matters for an AI system: because Claude can read and write files on your computer, Obsidian becomes the shared memory between you and your AI. You write notes in Obsidian; Claude can read them, add to them, search across them, and create new ones. It's the foundation everything else builds on.

Obsidian also has a powerful linking system — you can link any note to any other note using `[[double brackets]]`, creating a web of connected knowledge. Over time your vault becomes a true second brain.

## Step 1: Install Obsidian

1. Go to [obsidian.md](https://obsidian.md) and download the free app for Mac
2. Open the installer and drag Obsidian to your Applications folder
3. Launch Obsidian

## Step 2: Create Your Vault

When Obsidian opens, it will ask you to create or open a vault.

1. Click **"Create new vault"**
2. Give it a name (something like `vault` or `MyBrain` — keep it simple, no spaces is best)
3. Choose where to save it — `~/Documents/vault` works well (that's inside your Documents folder)
4. Click **Create**

You now have an empty vault. It's just a folder on your Mac — you can open it in Finder anytime.

## Step 3: Understand the Core Concepts

Before building out your structure, understand these three Obsidian fundamentals:

**Notes** are just Markdown files (`.md`). Markdown is a simple way of formatting text — `# Heading`, `**bold**`, `- bullet point`. Obsidian renders these beautifully, but underneath they're plain text files you can open in any text editor.

**Wikilinks** (`[[Note Name]]`) let you link any note to any other note. Type `[[` and start typing a note name — Obsidian will autocomplete. This is how your vault becomes a web rather than a pile of files.

**Frontmatter** is optional metadata at the top of a note, enclosed in `---`:
```yaml
---
type: journal
created: 2026-05-31
tags: [health, fitness]
---
```
This lets you filter, sort, and query notes later. You don't need to understand it deeply right now — just know it exists.

## Step 4: Design Your Folder Structure

A good vault structure makes everything easier later. Here's a recommended starting structure — adapt it to your life:

```
vault/
├── Daily/              # Daily notes (one per day)
├── Monthly/            # Monthly summaries
├── Journal/            # Personal journal entries
├── Reference/          # Clippings, research, reference material
│   ├── Braindumps/     # Quick capture of raw thoughts
│   ├── Research/       # Topic deep-dives
│   └── Tech/           # Tech notes and guides
├── Wiki/               # Your personal wiki (organized by domain)
│   ├── Health/         # Health tracking, providers, medications
│   ├── Projects/       # Ongoing projects
│   └── People/         # Notes on people in your life
├── Templates/          # Note templates
└── Attachments/        # Images and files
```

To create this structure in Obsidian: right-click in the left sidebar and choose **"New folder"**.

**Key principle:** Don't over-engineer this upfront. Start with a few folders and add more as you learn what you actually need.

## Step 5: Install the obsidian-cli Tool

This small command-line tool lets Claude open notes in Obsidian automatically after creating them. It's very useful.

1. Open **Terminal** (search for it in Spotlight with Cmd+Space)
2. If you have Homebrew installed, run:
   ```bash
   brew install obsidian-cli
   ```
3. If you don't have Homebrew yet, install it first:
   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   ```
   Then run the `brew install obsidian-cli` command above.

## Step 6: Recommended Community Plugins

Obsidian has a large plugin ecosystem. Go to **Settings → Community plugins → Browse** to find these. Turn off "Safe mode" when prompted.

**Essential:**
- **Templater** — powerful templates for new notes (auto-fill dates, prompts, etc.)
- **Calendar** — adds a calendar view for navigating daily notes
- **Dataview** — lets you query your vault like a database (advanced but powerful)

**Recommended:**
- **Tasks** — enhanced task tracking within notes
- **Tag Wrangler** — manage and rename tags across your vault

Install each by searching for it and clicking Install, then Enable.

---

# Layer 2 — Claude Code: Your AI

## What Claude Code Is (and How It's Different)

Claude Code is Anthropic's **command-line AI tool** — it runs in your Terminal and has the ability to read files, write files, run commands, and connect to external services. This is fundamentally different from Claude in a web browser, which can only see what you paste into the chat.

When you run Claude Code from inside your vault folder, Claude can:
- Read any note in your vault
- Create and edit notes on your behalf
- Search across your entire vault
- Run scripts and commands
- Connect to external services (more on this in Layer 4)

Think of it as the difference between hiring an assistant who works remotely via email (browser Claude) vs. one who sits at your desk with access to your files (Claude Code).

## Step 1: Get API Access

Claude Code requires either:

**Option A — API Key (pay per use)**
1. Go to [console.anthropic.com](https://console.anthropic.com)
2. Create an account and add billing
3. Go to **API Keys** and create a new key
4. Save this key somewhere safe — you'll only see it once

**Option B — Claude Max Subscription**
1. Go to [claude.ai](https://claude.ai) and subscribe to Max ($100/month)
2. Claude Code is included with Max and uses a different billing model
3. Better value if you use Claude heavily

For moderate usage, the API key approach costs roughly $5–30/month. Max is better if you're using it for hours every day.

## Step 2: Install Claude Code

1. Make sure you have Node.js installed. Check by running in Terminal:
   ```bash
   node --version
   ```
   If you get an error, install Node.js from [nodejs.org](https://nodejs.org) (download the LTS version)

2. Install Claude Code globally:
   ```bash
   npm install -g @anthropic-ai/claude-code
   ```

3. Start Claude Code:
   ```bash
   claude
   ```

4. When prompted, enter your API key or log in with your Claude account.

## Step 3: Open Your Vault as the Working Directory

This is the key move — always launch Claude Code from inside your vault folder. This gives Claude context about where everything is.

```bash
cd ~/Documents/vault
claude
```

You can also add this as a shortcut. In your Terminal, add this to your `~/.zshrc` or `~/.bash_profile`:
```bash
alias vault='cd ~/Documents/vault && claude'
```

Then you can just type `vault` to launch Claude in your vault.

## Step 4: Your First Conversations

Once Claude Code is running inside your vault, try these:

```
Search my vault for any notes about [topic] and summarize what I have
```
```
Create a new journal entry for today reflecting on [something that happened]
```
```
Look at my Daily/ folder and tell me what I've been thinking about this week
```

Claude will read your notes, respond with context, and can create new notes — all without you leaving the terminal.

---

# Layer 3 — CLAUDE.md: Teaching Claude Who You Are

## What CLAUDE.md Is (and Why It's the Secret Sauce)

Every project that Claude Code works in can have a `CLAUDE.md` file — a plain text instruction file that Claude reads at the start of every conversation. This is how you teach Claude about *your specific setup*, your preferences, your naming conventions, and your life context.

Without `CLAUDE.md`, Claude gives you generic answers. With it, Claude knows:
- Your vault's folder structure and what goes where
- Your timezone and date format preferences
- How you like things written (tone, style, length)
- Personal context that makes its responses actually useful
- Which tools and MCP servers are available

This is what separates a generic AI from *your* AI.

## Step 1: Create Your CLAUDE.md

Create a file called `CLAUDE.md` directly in your vault root (the top-level folder):

1. In Obsidian's left sidebar, right-click and choose **New note**
2. Name it `CLAUDE.md`
3. Start filling it in (see the template below)

You can also create it in Terminal:
```bash
touch ~/Documents/vault/CLAUDE.md
```
Then open it in any text editor.

## Step 2: Fill In Your CLAUDE.md

Here's a template to start from — personalize every section:

```markdown
# CLAUDE.md — My Vault

> **IMPORTANT — Timezone:** I am in [YOUR CITY] ([YOUR TIMEZONE]).
> Always use this timezone for dates and times. Never use UTC.
> When running `date` in bash, always use `TZ=[YOUR_TIMEZONE] date +%Y-%m-%d`.

> **IMPORTANT — Open notes after creating:** After creating any note,
> always open it in Obsidian immediately using
> `obsidian-cli open "<note name without .md>"`.

## About This Vault

This is my personal second-brain and knowledge management system.
It contains my journal, reference notes, project tracking, and personal wiki.

## About Me

- **Location:** [Your city, state]
- **Occupation:** [Your job/role]
- **Key interests:** [Things you want Claude to know about]
- **Family context:** [Whatever is relevant — partner, kids, pets, etc.]

## Vault Structure

[Copy your folder structure here, with brief descriptions of what goes where]

## File Conventions

- **Dated notes:** YYYY-MM-DD Description.md in vault root
- **Monthly notes:** YYYY-MM.md in Monthly/
- Use Obsidian wikilinks [[Like This]] for internal links
- Include YAML frontmatter on structured notes

## Tone and Style

- Keep responses conversational but concise
- Use Obsidian-flavored markdown (wikilinks, callouts, frontmatter)
- Ask before making big structural changes
- [Add any other preferences]

## MCP Servers Available

[You'll fill this in as you add MCP servers in Layer 4]
```

## Step 3: Keep It Updated

Your `CLAUDE.md` is a living document. Whenever you change your vault structure, add a new MCP server, or discover Claude keeps doing something wrong — update the file. Think of it as your AI's employee handbook.

A few tips:
- Use `> **IMPORTANT:**` callouts for rules Claude must always follow
- Be specific about dates, timezones, and naming conventions — these cause the most confusion
- Add examples when something is hard to explain in the abstract

---

# Layer 4 — MCP Servers: Connecting Claude to Your Life

## What MCP Servers Are (and What Becomes Possible)

MCP (Model Context Protocol) is a standard that lets Claude connect to external services — your calendar, email, tasks, smart home, and more. Each MCP "server" is a small background process that runs on your computer and acts as a bridge between Claude and a service.

Once MCP servers are connected, you can say things like:
- *"What's on my calendar tomorrow?"*
- *"Search my email for the invoice from last month"*
- *"Add 'call the dentist' to my tasks for Tuesday"*
- *"Turn off the living room lights"*

Without MCPs, Claude can only see your vault files. With them, Claude becomes a genuine life assistant.

MCP servers are configured in a file at `~/.claude/mcp.json` (a global config that applies to all your projects). You'll be editing this file to add each server.

## How to Edit mcp.json

1. Open Terminal and run:
   ```bash
   open ~/.claude/mcp.json
   ```
   If the file doesn't exist yet, create it:
   ```bash
   echo '{"mcpServers": {}}' > ~/.claude/mcp.json
   open ~/.claude/mcp.json
   ```

2. This opens the file in your default text editor. The structure looks like this:
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

Each server gets its own named block inside `"mcpServers"`. We'll add them one at a time below.

---

## MCP Server 1: Obsidian Vault

**What it does:** Lets Claude read and write your vault notes through Obsidian's own API, which preserves metadata and wikilinks better than reading raw files.

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

**Test it:** Start Claude Code in your vault and ask: *"List the files in my Daily/ folder."*

---

## MCP Server 2: Google Calendar

**What it does:** Lets Claude read your calendar events and create new ones on your behalf.

**Setup:**
1. Claude Code has official Google integrations. Run this command in Terminal:
   ```bash
   claude mcp add --transport http google-calendar \
     https://mcp.claude.ai/mcp/google-calendar
   ```
2. Claude Code will open a browser window asking you to authorize your Google account
3. Follow the authorization flow and grant calendar access

**Once connected, try:**
- *"What do I have going on this week?"*
- *"Schedule a dentist appointment for next Tuesday at 2pm"*
- *"What time is my meeting with [name] tomorrow?"*

**Important:** Add a note in your `CLAUDE.md` about which calendar to use by default if you have multiple Google calendars.

---

## MCP Server 3: Gmail

**What it does:** Lets Claude search your email and create draft replies.

**Setup:**
```bash
claude mcp add --transport http gmail \
  https://mcp.claude.ai/mcp/gmail
```
Authorize with your Google account when prompted.

**Once connected, try:**
- *"Search my email for any messages from [sender] in the last week"*
- *"Find the confirmation email for my Amazon order"*
- *"Draft a reply to the last email from [person]"*

**Note:** Claude can read and draft emails but won't send anything without you explicitly approving it. It's safe.

---

## MCP Server 4: Things 3 (Task Manager)

**What it does:** Connects Claude to the Things 3 app on your Mac, letting it read and create tasks.

**Prerequisites:** Things 3 must be installed (it's a paid app, $49.99 from the Mac App Store — worth it).

**Setup:**
```bash
npm install -g @anthropic-ai/mcp-server-things3
```

Add to `mcp.json`:
```json
"things3": {
  "command": "mcp-server-things3",
  "args": []
}
```

**Once connected, try:**
- *"What tasks do I have due today?"*
- *"Add 'pick up dry cleaning' to my inbox"*
- *"Show me everything in my Work project"*

**Alternative:** If you use a different task manager (Todoist, Reminders, OmniFocus), search for `mcp [your app name]` — there are community MCP servers for most popular apps.

---

## MCP Server 5: Home Assistant *(Optional)*

**What it does:** Connects Claude to your smart home. If you run Home Assistant, Claude can query device states and trigger automations.

**Prerequisites:** Home Assistant must be installed and running on your network.

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

**Once connected, try:**
- *"What lights are currently on?"*
- *"Turn off all the lights in the living room"*
- *"What's the temperature inside right now?"*

---

## MCP Server 6: Web Search *(Optional)*

**What it does:** Gives Claude the ability to search the web for current information. Claude's training data has a cutoff date — web search keeps it up to date.

The easiest option is to use a free SearXNG instance (a privacy-respecting search engine you can self-host or use a public instance of):

```json
"searxng": {
  "command": "npx",
  "args": ["-y", "mcp-searxng"],
  "env": {
    "SEARXNG_URL": "https://searxng.instance.example.com"
  }
}
```

Find a public SearXNG instance at [searx.space](https://searx.space) — pick one with good uptime.

---

## Verifying Your MCP Setup

After adding servers, restart Claude Code (`Ctrl+C` then `claude` again). Run this to check what's connected:

```
/mcp
```

Claude will show you all configured servers and whether they're connected successfully. If any show errors, double-check the config in `mcp.json` — usually it's a typo or a missing API key.

---

# Layer 5 — Skills: Custom Workflows

## What Skills Are (and How They Work)

Skills are instruction files that teach Claude how to perform a specific repeatable workflow. When you invoke a skill, Claude reads the instructions and follows them precisely — consistently, every time.

Think of skills as macros for your AI. Instead of explaining *"when I say create a journal entry, I want you to: check today's date, look at my recent notes, ask me what happened today, then create a new file in my Journal folder with this specific format..."* — you write that once in a skill file and invoke it with a single command.

Skills live in `~/.claude/skills/` (a folder in your home directory). Each skill is a Markdown file with instructions Claude follows when called.

## Your Most Useful Skills to Build

### Skill 1: Daily Morning Briefing

This skill runs every morning and gives you a personalized briefing. Here's a starter template — save it as `~/.claude/skills/morning-briefing`:

```markdown
# Daily Morning Briefing

When this skill is invoked, perform the following steps in order:

1. **Get today's date** using the system date command
2. **Check the calendar** — list today's and tomorrow's events from Google Calendar
3. **Check tasks** — list tasks due today from Things 3
4. **Check the weather** — search for today's weather in [YOUR CITY]
5. **Scan recent notes** — look at any notes created in the vault in the last 24 hours
6. **Generate briefing** — create a concise briefing note with all of the above,
   saved to Daily/YYYY-MM-DD-morning-brief.md
7. **Open the note** in Obsidian using obsidian-cli

Format the briefing with clear sections: Calendar | Tasks | Weather | Recent Notes
Keep it scannable — this is a morning read, not an essay.
```

**To invoke it:** Type `/morning-briefing` in Claude Code, or just say *"run my morning briefing"*.

### Skill 2: Journal Entry

Save as `~/.claude/skills/journal`:

```markdown
# Journal Entry

When this skill is invoked:

1. Get today's date
2. Check if a journal entry already exists for today in Journal/
3. Ask me: "What do you want to journal about today?"
4. Listen to my response, then ask 1-2 follow-up questions to draw out more detail
5. Create a journal entry in Journal/YYYY-MM-DD-journal.md with:
   - YAML frontmatter (type: journal, date, tags)
   - A narrative summary of what I shared
   - A "Reflection" section with any patterns or themes you notice
6. Open the note in Obsidian

Keep the tone warm and personal. This is a private journal, not a report.
```

### Skill 3: Braindump

Save as `~/.claude/skills/braindump`:

```markdown
# Braindump

When this skill is invoked, help me empty my brain:

1. Ask: "What's on your mind? Dump everything — ideas, worries, tasks, random thoughts."
2. Let me talk (or type) without interruption
3. When I'm done, organize what I shared into categories:
   - **Tasks** (things to do)
   - **Ideas** (things to explore)
   - **Concerns** (things worrying me)
   - **Insights** (things I realized)
4. Create tasks in Things 3 for anything actionable
5. Save the full braindump to Reference/Braindumps/YYYY-MM-DD-braindump.md
6. Give me a 3-sentence summary of what I'm really thinking about

This should feel like a conversation, not a form.
```

### Skill 4: Weekly Check-In

Save as `~/.claude/skills/weekly-checkin`:

```markdown
# Weekly Check-In

When this skill is invoked:

1. Get the current date and calculate the past 7 days
2. Scan the vault for notes, journal entries, and braindumps from the past week
3. Check Things 3 for completed and overdue tasks from this week
4. Ask me 3 reflection questions (one at a time):
   - "What were your biggest wins this week?"
   - "What didn't go as planned?"
   - "What's the most important thing for next week?"
5. Synthesize everything into a weekly check-in note:
   - Save to Monthly/YYYY-WW-weekly-checkin.md
   - Include: What happened | Wins | Challenges | Intentions for next week
6. Open the note in Obsidian

This is a 10-minute weekly ritual, not a performance review.
```

## How to Invoke Skills

In Claude Code, you can invoke skills two ways:

1. **Slash command:** Type `/skill-name` (e.g., `/journal`, `/morning-briefing`)
2. **Natural language:** Just describe what you want and Claude will recognize it if you've described triggers in your skill file

You can also add trigger phrases in each skill file:
```markdown
**Triggers:** "morning briefing", "daily brief", "start my day", "what's happening today"
```

## Tips for Writing Good Skills

- **Be specific about file locations** — don't say "save a note," say "save to Journal/YYYY-MM-DD-name.md"
- **Describe the format** — if you want bullet points, say so; if you want prose, say so
- **Add tone guidance** — "keep it warm and conversational" vs "be concise and clinical"
- **Include error handling** — "if the file already exists, append rather than overwrite"
- **Iterate** — use a skill for a week, then edit the file based on what felt off

---

# Layer 6 — Plugins: Superpowers for Claude Code

## What Plugins Add

While skills are instruction files you write yourself, **plugins** are pre-built packages that extend Claude Code's capabilities. They add new slash commands, new workflows, and new tools directly into Claude Code — maintained by Anthropic and the community.

Think of them like apps for your AI.

## Installing Plugins

Plugins are installed from the Claude Code marketplace using the `/plugins` command inside Claude Code, or via the command line.

To browse available plugins, type `/plugins` in Claude Code — this opens the plugin marketplace.

## The Most Useful Plugins

### Superpowers (Official — Highly Recommended)

The Superpowers plugin is a meta-layer that adds sophisticated reasoning workflows to Claude Code. It's the most impactful plugin for non-developers.

**Install:**
```bash
claude plugins install superpowers@claude-plugins-official
```

**What it adds:**
- `/brainstorming` — structured brainstorming session that turns vague ideas into detailed plans before doing anything
- `/writing-plans` — creates step-by-step implementation plans for complex projects
- `/systematic-debugging` — structured debugging workflow when something isn't working

**Why it matters:** Without Superpowers, Claude Code will jump straight into action. With it, you get a thoughtful planning phase first — which produces dramatically better results for complex projects.

### Context7 (Official)

Gives Claude access to up-to-date documentation for thousands of software libraries and tools.

```bash
claude plugins install context7@claude-plugins-official
```

Less useful for non-developers, but invaluable if you ever ask Claude to help with any technical setup.

### Skill Creator (Official)

Adds a `/skill-creator` command that helps you *build* skills through a guided conversation. Instead of writing skill files from scratch, you describe what you want and it writes the skill for you.

```bash
claude plugins install skill-creator@claude-plugins-official
```

**Highly recommended** — makes it much easier to grow your skill library over time.

### Claude Memory (Community — thedotmack)

Adds a persistent memory system to Claude Code. Claude can save facts about you that persist across conversations (beyond what's in CLAUDE.md).

```bash
claude plugins install claude-mem@thedotmack
```

### Commit Commands (Official)

If you use Git to version-control your vault (recommended!), this adds clean commit workflows.

```bash
claude plugins install commit-commands@claude-plugins-official
```

## Version-Controlling Your Vault with Git *(Recommended)*

One powerful pattern: turn your vault into a Git repository. This gives you:
- A complete history of every change to every note
- The ability to undo any edit Claude makes
- A backup if you ever need to restore

```bash
cd ~/Documents/vault
git init
git add .
git commit -m "Initial vault setup"
```

You can then push to a private GitHub repository for offsite backup. Everything is plain text, so it works perfectly with Git.

---

# Putting It All Together: A Day in the Life

Here's what a typical day looks like once all of this is running:

**Morning (5 minutes)**
You open Terminal, type `vault`, and run `/morning-briefing`. Claude checks your calendar, scans your tasks, looks at the weather, glances at any notes you made yesterday, and generates a clean morning brief that opens automatically in Obsidian. You read it over coffee.

**Throughout the day**
A thought occurs to you while you're at your desk. You open Terminal and type:
*"Quick note: I want to research [topic] for [project]. Add a task to look into this."*
Claude creates a note in your vault and adds the task to Things 3. You never lose the thought.

You get an email that needs a response. You say:
*"Find the email from [name] about [topic] and draft a reply saying [what you want to say]."*
Claude searches your Gmail, finds the thread, drafts a reply, and saves it as a draft. You review and send it yourself.

**Evening (10 minutes)**
You run `/journal` and tell Claude how your day went. It asks a couple of follow-up questions, then writes a reflective journal entry and saves it to your vault.

**Weekly**
You run `/weekly-checkin`. Claude scans everything you wrote this week, asks you three reflection questions, and creates a synthesis note. Over time, these become an invaluable record of how your life is actually going.

---

# Maintenance and Growth

## Keep CLAUDE.md Updated

Every time you add a new MCP server, change your folder structure, or discover Claude keeps making the same mistake — update your `CLAUDE.md`. It's the single most important thing you can do to keep the system working well.

## Grow Your Skills Gradually

Don't try to build 20 skills at once. Start with the morning briefing and journal skills, use them for two weeks, then add the next one. Skills that you actually use become tuned over time; skills you don't use just clutter your system.

## Review and Prune Your Vault

Once a month, look at what Claude has been creating in your vault. Are the notes useful? Is the folder structure holding up? Delete what isn't working, reorganize what doesn't fit. The vault should serve you — not the other way around.

## Resources

- **Obsidian documentation:** [help.obsidian.md](https://help.obsidian.md)
- **Obsidian community forum:** [forum.obsidian.md](https://forum.obsidian.md)
- **Claude Code documentation:** [docs.anthropic.com/claude-code](https://docs.anthropic.com/claude-code)
- **MCP documentation:** [modelcontextprotocol.io](https://modelcontextprotocol.io)
- **Anthropic API console:** [console.anthropic.com](https://console.anthropic.com)
- **Claude Code plugin marketplace:** accessible via `/plugins` inside Claude Code

---

*This guide describes a living system — expect to iterate, experiment, and adapt it to your own life. The version that works best for you won't look exactly like anyone else's setup. That's the point.*
