---
name: wiki-ingest
description: Ingest a raw source note into an LLM-maintained wiki domain. Triggered by "ingest", "wiki ingest", "add to the wiki", or when processing a vault note into the wiki layer.
---

# Wiki Ingest

Ingest a raw source note into an LLM-maintained wiki in the Obsidian vault. Reads the source, proposes pages to create/update, writes them after approval, and maintains the wiki's index, timeline, and log.

## Arguments

The skill receives two pieces of information:

1. **Source path** — a wikilink or file path to the note being ingested. If not provided, ask for it.
2. **Target wiki** — which wiki domain to ingest into (e.g., `Wiki/Health/`, `Wiki/Homelab/`). If the vault has only one wiki, use it. If there are multiple, ask.

---

## Steps

### 1. Load Wiki Context

Read these files from the target wiki directory:

- **`CLAUDE.md`** — local schema, frontmatter conventions, page types, sensitivity rules
- **`index.md`** — current catalog of all pages + stats (tells you what already exists)
- **`log.md`** — recent ingest entries (tells you what was recently processed; avoids re-ingesting)

Get today's date:
```bash
TZ=[USER_TIMEZONE] date +%Y-%m-%d
```

---

### 2. Read the Source

Read the source note in full. Keep in mind:

- Raw sources are **never modified** by this skill — read-only
- Sources are typically dated journal entries, reference notes, or other vault content
- The source may be dense (a full meeting transcript) or sparse (a brief observation)

---

### 3. Analyze and Propose

Extract entities, events, decisions, and facts from the source. Cross-reference against `index.md` to determine:

**Pages to create** — new entities not yet in the wiki (people, places, services, threads, concepts)

**Pages to update** — existing pages that gain new information from this source

**Pages deliberately skipped** — and why (unnamed references, single mentions not worth a page, out-of-scope content)

**Design decisions to apply:**

- **Entities:** Named individuals or specific items get their own page. Anonymous or incidental references (e.g., "the nurse on duty") are skipped until they're named or recur.
- **Grouping:** Multiple contacts from the same organization can be grouped on a single page (e.g., `Acme Support Team.md`) if they don't individually warrant separate pages.
- **Threads:** Create a thread page when a topic will accumulate across multiple sources over time. One-off facts can live on an entity page instead.
- **Contradictions:** If the source contradicts an existing wiki page, flag with `> [!warning] Contradiction` on the relevant page and keep both claims until resolved.
- **Stale data:** If a figure is from an old source or may be outdated, flag with `> [!warning]` and note what needs verification.
- **Tasks:** List tasks as prose bullets on the relevant thread page. Do not create wikilinks to task items — the task manager (Things 3, Todoist, etc.) is the authoritative system. The wiki maps tasks, not duplicates them.

---

### 4. Present Proposal

Show the user:

1. **Pages to create** — name, type, one-line description of content
2. **Pages to update** — name, what sections/info will be added
3. **Infrastructure updates** — Timeline, index, log (always updated; no per-source approval needed)
4. **Deliberately skipped** — what and why

**Wait for approval before writing.** The user may adjust scope, rename pages, or skip items.

---

### 5. Write Pages

After approval, write/update all approved pages.

**Frontmatter for new pages** (use page types from the wiki's `CLAUDE.md`):

```yaml
---
type: person | facility | thread | service | concept | character
status: active | resolved | dormant | pending
last_updated: YYYY-MM-DD
sources:
  - "[[Source Note Title]]"
tags:
  - <domain-tag>
  - <topic tags>
---
```

**Every wiki page must include:**

- A `## Related` section at the bottom with categorized wikilinks (threads, people, timeline, etc.)
- At least one source citation — if a fact has no source, mark it `(unsourced — verify)`
- Obsidian-flavored markdown: wikilinks `[[Page Name]]`, YAML frontmatter, callouts `> [!warning]` / `> [!note]`

**When updating existing pages:**
- Add new sections rather than rewriting existing content
- Append to the `sources` list in frontmatter
- Expand the `## Related` section with new links
- Do NOT change `status` unless the source explicitly resolves or supersedes something

---

### 6. Update Timeline

Append chronological entries to `Timeline.md` for any event-bearing content in the source. Format:

```markdown
### YYYY-MM-DD — Short headline
One-to-three sentence summary. Sources: [[raw note]]. Related: [[wiki page]], [[another page]].
```

Insert entries in chronological order (not always at the bottom — find the right date position). Add the source to Timeline's frontmatter `sources` list.

---

### 7. Update Index

Update `index.md`:

- Add new pages under the correct section with a one-line summary
- Keep entries **alphabetically sorted** within each section
- Update the Stats block: page count, sources ingested, last ingest date + source link

---

### 8. Append to Log

```markdown
## [YYYY-MM-DD] ingest | <Source Note Title>

Source: [[Source Note Title]]. Pages created:
- [[Page Name]] (type, status)

Files updated:
- [[Page Name]] — what changed

<Design decisions, flags raised, items deliberately skipped with reasoning>
```

---

### 9. Report to User

Summarize:
- Pages created (count + list)
- Pages updated (count + list)
- Any warnings or flags raised (contradictions, stale data, items needing follow-up)
- Updated wiki stats (total pages, total sources ingested)

---

## General Patterns

These hold true across most wikis and should guide judgment calls:

- **Thin pages grow over time.** Don't pad a new page — leave it sparse and let it fill in as more sources arrive.
- **Thread pages are the most-linked type.** They connect people, places, timeline events, and other threads. Invest in good thread pages.
- **Name discrepancies happen.** When two sources spell a name differently, pick a canonical spelling (prefer the more authoritative source) and flag the discrepancy with `> [!warning]`.
- **Forward risks are worth flagging.** If a source reveals something that could matter later, flag it even if it's not an immediate issue.
- **One source can touch many pages.** A brief note might create 1 page; a dense document might create 6 and update 5. Both are normal.
- **The index is the ground truth.** If something isn't in the index, it doesn't exist as far as future sessions are concerned. Keep it current.

---

## Success Criteria
- Source note is fully analyzed — nothing significant missed
- Proposal is clear and the user understands what will be created/updated before it happens
- All written pages have correct frontmatter, source citations, and a `## Related` section
- Timeline, index, and log are all updated
- No raw source notes were modified
- Flags and warnings are clearly communicated to the user
