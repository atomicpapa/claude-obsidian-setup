---
name: wiki-create
description: Scaffold a new LLM-maintained wiki domain in the vault. Triggered by "create a wiki", "new wiki", "add a wiki for", or when the user wants to set up a new topic-specific wiki.
---

# Wiki Create

Scaffold a new LLM-maintained wiki domain in the Obsidian vault. Creates the folder structure, schema, index, log, and timeline, and registers it in the vault's CLAUDE.md.

## Arguments

The skill receives a **topic/domain name** (e.g., "Homelab", "Health", "ProjectName"). If not provided, ask what the wiki should cover.

---

## Steps

### 1. Gather Requirements

Ask the user (skip any they already answered):

1. **Domain name** — short label for the folder (e.g., "Homelab"). Will become `Wiki/<Name>/`.
2. **Purpose** — one-to-two sentence description of what this wiki covers and why it's worth maintaining as a persistent synthesis layer.
3. **Entity categories** — what kinds of pages will it hold? Propose defaults based on the domain and let the user adjust. Examples:
   - Caregiving wiki: `People`, `Facilities`, `Threads`
   - Fiction wiki: `Characters`, `Places`, `Factions`, `Lore`, `Books`
   - Tech/homelab wiki: `Services`, `Infrastructure`, `Guides`
   - Health wiki: `Providers`, `Conditions`, `Medications`, `Threads`
   - Research wiki: `Sources`, `Concepts`, `People`, `Arguments`
   - Project wiki: `Decisions`, `Components`, `People`, `Threads`
4. **Sensitivity** — any privacy or sensitivity notes for this wiki? (Default: none beyond standard vault privacy.)
5. **Existing source notes** — are there already notes in the vault that would feed this wiki? Don't ingest yet — just note them so the user knows where to start.

---

### 2. Create Folder Structure

```
Wiki/<Name>/
├── CLAUDE.md
├── index.md
├── log.md
├── Timeline.md
└── <Category>/        ← one subfolder per entity category
    └── .gitkeep
```

Create `.gitkeep` in each empty subfolder so the folders persist in git.

---

### 3. Write CLAUDE.md (Wiki-Local Schema)

This is the local instruction file for this wiki. Include:

**Purpose** — what this wiki covers, adapted from the user's description.

**Sensitivity** — any privacy notes from Step 1. Default to standard vault privacy if none.

**Layout** — the folder tree with a one-line description of each section.

**Frontmatter schema** — standardize for this wiki:

```yaml
---
type: <domain-specific types, e.g., person | facility | thread | service | character>
status: active | resolved | dormant | pending
last_updated: YYYY-MM-DD
sources:
  - "[[Source Note Title]]"
tags:
  - <domain-tag>
  - <topic>
---
```

**Standard conventions** (apply to all wikis unless domain requires otherwise):
- Always link back to raw source notes; mark unsourced claims `(unsourced — verify)`
- Never modify raw source notes — they are read-only
- Flag contradictions with `> [!warning] Contradiction` and keep both claims until resolved
- Log entries use `## [YYYY-MM-DD] <op> | <title>` format
- Every page must have a `## Related` section at the bottom with categorized wikilinks

**Operations** — brief description of the three wiki skills and when to use each:
- `wiki-ingest` — add new source notes to the wiki
- `wiki-lint` — audit for broken links, orphans, and unresolved flags
- `wiki-create` — (this skill) scaffold new domains

**Out of scope** — what this wiki does NOT cover (helps avoid scope creep over time).

---

### 4. Write index.md

```markdown
---
type: index
last_updated: YYYY-MM-DD
tags:
  - <domain-tag>
  - wiki-index
---

# <Name> Wiki — Index

Catalog of every page in this wiki. Updated on every ingest. See [[CLAUDE]] for local conventions and [[log]] for chronological history.

## Timeline

- [[Timeline]] — rolling narrative of events

## <Category 1>

_No pages yet._

## <Category 2>

_No pages yet._

<!-- repeat for each category -->

## Stats

- **Pages:** 0
- **Sources ingested:** 0
- **Last ingest:** —
- **Last lint:** —
```

---

### 5. Write log.md

```markdown
---
type: log
tags:
  - <domain-tag>
  - wiki-log
---

# <Name> Wiki — Log

Append-only record of ingests, queries, and lint passes. See [[CLAUDE]] for format.

---

## [YYYY-MM-DD] init | Wiki skeleton created

Created `Wiki/<Name>/` with `index.md`, `log.md`, `CLAUDE.md`, `Timeline.md`, and subfolders: <list categories>. No sources ingested yet.
```

---

### 6. Write Timeline.md

```markdown
---
type: timeline
status: active
last_updated: YYYY-MM-DD
sources: []
tags:
  - <domain-tag>
  - timeline
---

# <Name> Wiki — Timeline

Rolling narrative of events, chronological and summarized. Each entry links to raw source note(s) and relevant wiki pages.

Format per entry:
### YYYY-MM-DD — Short headline
One-to-three sentence summary. Sources: [[raw note]]. Related: [[wiki page]].

---

_Empty. Will be populated during the first ingest._
```

---

### 7. Register in Vault CLAUDE.md

Update `~/Documents/vault/CLAUDE.md` in two places:

1. **Vault structure tree** — add `Wiki/<Name>/` under `Wiki/` with a short comment describing what it covers
2. **Wiki pattern section** — add the new wiki to the list of active wikis, with its purpose in one line

---

### 8. Confirm

Tell the user:
- Folder created with full skeleton at `Wiki/<Name>/`
- List the entity categories that were scaffolded
- Remind them to use `/wiki-ingest` to start populating it
- If they mentioned existing source notes in Step 1, suggest those as first ingest candidates

---

## Success Criteria
- All files created with correct frontmatter and placeholder content
- CLAUDE.md clearly documents the schema and conventions for this wiki
- Vault CLAUDE.md updated so future sessions know this wiki exists
- User knows exactly what to do next (run `/wiki-ingest` with a source note)
