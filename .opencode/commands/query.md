---
name: query
description: Search both Zotero and Obsidian for sources and notes on a topic. Return a unified shortlist with PDF-availability flags and existing-note links.
primary-agent: librarian
references:
  - ../_meta/vault.md
  - ../_meta/zotero.md
  - ../.opencode/agents/librarian.md
  - ../.opencode/skills/zotero-use/SKILL.md
  - ../.opencode/skills/obsidian-cli/SKILL.md
---

# Query

## Inputs

- A topic (free text)
- Optional scope: zotero-only, obsidian-only, or both (default: both)

## Workflow

1. **Librarian** queries Zotero via `zot --local --library-id 0 --library-type user items list --query "..." --qmode everything --limit 20`.
2. **Librarian** queries Obsidian via `obsidian "Obsidian" search query="..."`.
3. **Librarian** checks PDF availability for shortlisted Zotero items (`zot ... items children ITEM_KEY`).
4. **Librarian** produces a unified shortlist with:
   - Zotero items (cite-key, year, first author, title, journal, PDF available?)
   - Obsidian notes (path, title, link count, last modified)
   - Gap analysis: sources cited in notes but not in zotero, or zotero items with no note
5. Save the report to `sessions/{id}/queries/{topic-slug}.md`.

## Output

A markdown report grouping candidates by relevance, with a clear "best candidates" section at the top. No vault writes.

## Cleanup

- The report in `sessions/{id}/queries/` can be kept for the session and deleted with the rest of `sessions/`.
