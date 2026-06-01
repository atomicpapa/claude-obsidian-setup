---
name: wiki-lint
description: Audit an LLM-maintained wiki domain for structural problems — broken links, orphan pages, ghost index entries, unresolved warning flags, frontmatter gaps, and concept gaps. Triggered by "wiki lint", "lint the wiki", "audit the wiki", or "check for broken links".
---

# Wiki Lint

Scan an LLM-maintained wiki for structural problems, broken references, unresolved flags, and gaps. Produces a categorized report and offers to auto-fix mechanical issues.

## Arguments

**Target wiki** — which wiki domain to lint (e.g., `Wiki/Health/`, `Wiki/Homelab/`). If the vault has only one wiki, use it. If there are multiple and none is specified, ask.

---

## Steps

### 1. Load Wiki Context

Read from the target wiki directory:

- **`CLAUDE.md`** — local schema and conventions
- **`index.md`** — full page catalog and stats
- **`log.md`** — recent operations (to understand what was recently ingested)

Get today's date:
```bash
TZ=[USER_TIMEZONE] date +%Y-%m-%d
```

Then glob all wiki pages:
```
Wiki/<domain>/**/*.md
```

Exclude `index.md`, `log.md`, `CLAUDE.md`, and `Timeline.md` from the content page list — they're infrastructure, not content pages.

---

### 2. Run the Seven Checks

Run all checks before reporting. Each finding gets a **severity**:

- 🔴 **Blocking** — broken reference that misleads or dead-ends a reader
- 🟡 **Warning** — unresolved flag, missing metadata, or structural gap
- 🔵 **Info** — low-priority improvement (orphan page, possible concept gap)

---

#### Check 1 — Index Drift

Compare pages on disk vs. entries in `index.md`:

- **Missing from index:** a `.md` file exists in the wiki folder but has no entry in `index.md` → 🟡
- **Ghost index entry:** `index.md` lists a page that has no corresponding file → 🔴

#### Check 2 — Orphan Pages

A page is an orphan if no other wiki page links to it with `[[Page Name]]`. Grep all wiki pages for the orphan's wikilink title.

- A page with zero inbound links is often a dead end or a stub never connected to the rest → 🔵
- Exception: `Timeline.md`, `index.md`, `log.md` are excluded — they're linked by convention, not inline.

#### Check 3 — Ghost Links

Scan all wiki pages for `[[Wikilink]]` patterns. For each link target, check whether a corresponding file exists in the wiki folder.

- Link target has no matching file → 🔴
- Include aliased links: `[[File Name|Display Text]]` — check the file name part, not the display text.

#### Check 4 — Broken Source Citations

Read the `sources:` list in every page's frontmatter. For each `[[Source Note Title]]`, check whether a file with that name exists in the vault.

- Source note referenced in frontmatter but not found in vault → 🟡 (may have been renamed or not yet created)

#### Check 5 — Unresolved Warning Callouts

Grep all wiki pages for `> [!warning]`. Each hit is an open flag from a prior ingest. List them with:

- File name
- Callout header (first line of the callout)
- Brief quote of the flagged content

Every open `> [!warning]` is a known unresolved issue → 🟡

#### Check 6 — Frontmatter Gaps

Every wiki page must have: `type`, `status`, `last_updated`, and at least one entry in `sources`. Flag any page missing one or more of these fields → 🟡

Also flag pages where `last_updated` is more than 90 days older than today (may be stale) → 🔵

#### Check 7 — Concept Gaps

Scan page body text for proper nouns that look like they might warrant a wiki page but don't have one. Heuristics:

- Names capitalized in a way that suggests a person, facility, organization, or system
- Entities mentioned 3+ times across pages with no `[[wikilink]]` around them
- Any name appearing as free text that is also referenced by wikilink elsewhere (inconsistent linking)

These are suggestions, not hard errors → 🔵

---

### 3. Present the Report

Group findings by severity. For each finding, include:

- **File** — which page has the issue
- **Issue** — what's wrong
- **Suggested fix** — what to do
- **Auto-fixable or needs judgment** — clearly labeled

Example format:

```
## 🔴 Blocking (N)

- **Ghost link:** [[Some Page]] in `OtherPage.md` → no matching file found
  Auto-fixable: create stub page, OR remove link if intentional

## 🟡 Warning (N)

- **Unresolved warning callout** in `ThreadName.md`
  > [!warning] Possible contradiction: source A says X, source B says Y
  Needs judgment: confirm which is correct when more information arrives

## 🔵 Info (N)

- **Orphan:** `PersonName.md` — no other page links to it
  Suggestion: add a link from Timeline or the relevant thread page
```

After presenting the report, ask the user:

> "Want me to auto-fix the mechanical issues (index drift, frontmatter gaps, stub pages for ghost links)? I'll leave the judgment items for you to resolve."

---

### 4. Apply Fixes (After Approval)

For auto-fixable items only — do not resolve judgment items without explicit direction:

- **Index drift:** add missing entries to `index.md`; remove ghost entries
- **Frontmatter gaps:** add missing fields (`last_updated: YYYY-MM-DD`, `type`, `status`)
- **Ghost link → stub:** if the user approves, create a minimal stub page for the missing target

Stub page format:

```markdown
---
type: person | facility | thread | service | concept
status: pending
last_updated: YYYY-MM-DD
sources: []
tags:
  - <domain-tag>
---

# Page Title

> [!note] Stub
> This page was created by wiki-lint to resolve a broken link. Populate when the relevant source is ingested.

## Related

- Timeline: [[Timeline]]
```

---

### 5. Append to Log

```markdown
## [YYYY-MM-DD] lint | <wiki domain>

Pages scanned: N
Findings: N blocking, N warnings, N info
Auto-fixes applied: <list or "none">
Open items requiring judgment: <brief list>
```

---

### 6. Report to User

Summarize:
- Total findings by severity
- What was auto-fixed
- What remains open (judgment items)
- Updated wiki stats if index was changed

---

## Notes on Check 7 (Concept Gaps)

This is the fuzziest check. Apply judgment:

- A name that appears once in a passing reference doesn't need a page
- A name that appears 3+ times across different pages probably should have one
- If the name is already linked (`[[X]]`) somewhere but not everywhere, suggest consistent linking rather than a new page
- Don't flag generic nouns — only proper nouns that act as entities (people, organizations, facilities, systems, policies)

---

## Success Criteria
- All seven checks run before any report is presented
- Findings are clearly categorized by severity and labeled auto-fixable vs. needs judgment
- Auto-fixes are applied only after user approval
- Log is updated with the lint pass summary
- User knows exactly what open issues remain and what action each requires
