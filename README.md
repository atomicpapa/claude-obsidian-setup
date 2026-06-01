# Claude + Obsidian: An AI-Powered Second Brain

A personal knowledge system that connects Claude Code to your notes, calendar, tasks, email, and smart home — turning a general-purpose AI into an assistant that actually knows your life.

---

## The Concept

Most people use AI the way they use a search engine: open a tab, ask a question, close the tab. The conversation ends and nothing is remembered. The next session starts from zero.

This system takes a different approach. Instead of going to the AI, the AI lives inside your life — connected to the folder where your notes live, your calendar, your tasks, your email. It reads what you've written, remembers what matters, and can act on your behalf across the tools you already use.

The result is something closer to a personal chief of staff than a chatbot.

---

## The Three Layers

### 1. Obsidian — Your Vault

[Obsidian](https://obsidian.md) is a free, local-first note-taking app that stores everything as plain Markdown files on your computer. Your notes are yours — no cloud lock-in, no proprietary format, just text files in a folder.

This folder — your **vault** — becomes the shared memory between you and your AI. You write in it; Claude reads it, adds to it, searches across it. Over time it becomes a genuine second brain: interconnected notes, journal entries, research, project tracking, and a personal wiki all in one place.

### 2. Claude Code — Your AI

[Claude Code](https://docs.anthropic.com/claude-code) is Anthropic's command-line AI tool. Unlike the browser version of Claude, it can read and write files on your computer, run commands, and connect to external services through **MCP servers** (small background processes that bridge Claude to apps like Google Calendar, Gmail, Things 3, and Home Assistant).

Launching Claude Code from inside your vault gives it full context about your notes and your life. It's the difference between an assistant who works remotely via email and one who sits at your desk with access to your files.

### 3. Skills — Your Workflows

Skills are instruction files that teach Claude how to perform specific repeatable workflows. A morning briefing skill checks your calendar, pulls your tasks, looks at the weather, and generates a scannable note in your vault every morning. A journal skill draws out reflection through conversation and writes a structured entry. A weekly check-in synthesizes the week's notes and sets intentions for the next.

Skills live in `~/.claude/skills/` and are invoked with a slash command like `/morning-briefing` or `/journal`. Once written, they run the same way every time — no re-explaining, no configuration.

---

## What Makes This Different

**It builds up over time.** Your vault accumulates. Notes reference each other. Patterns emerge across weeks and months. The AI's responses improve because there's more context to work with.

**It's local and private.** Your vault is a folder on your computer. No cloud sync required, no third-party servers holding your notes. You can back it up with Git, sync it with iCloud or Syncthing, or keep it entirely offline.

**It's customizable to your life.** A `CLAUDE.md` file in your vault tells Claude about your specific setup — your folder structure, your timezone, your preferences, which MCP servers are connected. This is what separates a generic AI from *your* AI.

**It's built on open standards.** Obsidian uses Markdown. Claude Code uses MCP (an open protocol). Skills are plain text files. Nothing here is proprietary or locked to a single vendor.

---

## The Wiki Layer

One of the most powerful patterns in this system is the **LLM-maintained wiki** — a structured layer of synthesized knowledge that the AI builds and maintains from your raw notes.

The idea, drawn from [Andrej Karpathy's gist on using LLMs as a personal knowledge layer](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f), is a three-layer architecture:

- **Raw sources (human-owned, immutable)** — your dated notes, journal entries, transcripts, clippings. The AI reads these but never modifies them.
- **Wiki (AI-maintained)** — a `Wiki/<domain>/` folder for each topic you care about. Entity pages, timeline, index, and an append-only log. The AI creates and maintains these pages by ingesting your raw notes.
- **Schema** — your `CLAUDE.md` files that define the conventions the AI follows.

You might have a `Wiki/Health/` folder that synthesizes your medical notes into pages for each provider, condition, and medication — updated every time you ingest a new appointment note. Or a `Wiki/Projects/` that maintains decisions, components, and open threads for an ongoing project. The wiki grows smarter every time you add a source.

The three wiki skills (`/wiki-create`, `/wiki-ingest`, `/wiki-lint`) handle the full lifecycle: scaffolding a new domain, ingesting source notes with approval-gated page creation, and auditing for broken links and structural gaps.

---

## Where This Came From

This system is a synthesis of three ideas that each clicked into place separately.

### 1. The Vault Structure — Steph Ango

The folder structure and file conventions used throughout this guide are based on [Steph Ango's vault design](https://stephango.com/vault). Ango is the CEO of Obsidian, and his approach — plain files, dated notes, minimal hierarchy, no proprietary formats — is the philosophical foundation the rest of this system builds on. The principle: your notes should be readable without Obsidian, portable to any tool, and still legible decades from now.

### 2. The COG Framework — huytieu

The idea of connecting Claude Code to an Obsidian vault using `CLAUDE.md`, MCP servers, and skills came from [huytieu's COG second-brain repo](https://github.com/huytieu/COG-second-brain). COG stands for **C**ognition + **O**bsidian + **G**it. The repo demonstrated how to turn Claude Code from a general-purpose coding assistant into a personalized life OS — the vault's folder structure from Steph Ango was preserved and layered on top.

### 3. The Wiki Layer — Andrej Karpathy

The LLM-maintained wiki pattern was inspired by [Andrej Karpathy's gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f), which describes using LLMs as a synthesis layer over raw personal notes. The core insight: raw notes are human-owned and immutable, while a separate AI-maintained wiki layer synthesizes them into structured, evergreen knowledge. This was adapted and built into the vault as the `Wiki/` folder structure, with the three wiki skills (`/wiki-create`, `/wiki-ingest`, `/wiki-lint`) handling the full lifecycle.

---

## Getting Started

Choose your platform:

- **[Mac setup guide →](Claude-Obsidian-AI-Setup-Guide.md)** — step-by-step for macOS, using Terminal, Homebrew, Things 3, and obsidian-cli
- **[Windows setup guide →](Claude-Obsidian-AI-Setup-Guide-Windows.md)** — step-by-step for Windows, using PowerShell, winget, Todoist, and the Obsidian URI scheme

Both guides cover all six layers in order:

1. **Obsidian** — install, create your vault, structure your folders
2. **Claude Code** — install, get API access, launch from your vault
3. **CLAUDE.md** — teach Claude about your setup and preferences
4. **MCP servers** — connect Calendar, Gmail, tasks, smart home, and web search
5. **Skills** — build your daily briefing, journal, braindump, and weekly check-in workflows
6. **Plugins** — extend Claude Code with the Superpowers plugin, Skill Creator, and more

---

## Example Skills

The [`skills/`](skills/) folder contains ready-to-use skill files you can drop into `~/.claude/skills/` and start using immediately:

| Skill | Command | What It Does |
|---|---|---|
| `morning-briefing/` | `/morning-briefing` | Daily briefing: calendar, tasks, weather, news |
| `journal/` | `/journal` | Guided journal entry with reflection |
| `braindump/` | `/braindump` | Capture raw thoughts, auto-create tasks |
| `weekly-checkin/` | `/weekly-checkin` | Weekly synthesis and intention-setting |
| `wiki-create/` | `/wiki-create` | Scaffold a new wiki domain |
| `wiki-ingest/` | `/wiki-ingest` | Ingest source notes into a wiki |
| `wiki-lint/` | `/wiki-lint` | Audit a wiki for structural problems |

---

## License

MIT — use it, adapt it, make it your own.
