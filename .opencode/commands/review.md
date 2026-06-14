---
name: review
description: Vault audit. Identify orphans, broken links, stale TODOs, and low-link-density permanent notes. Output a maintenance report grouped by severity.
primary-agent: librarian
references:
  - ../_meta/vault.md
  - ../_meta/tags.md
  - ../.opencode/agents/librarian.md
  - ../skills/obsidian-cli/SKILL.md
  - ../skills/link-suggest/SKILL.md
---

# Review

## Inputs

- Optional: a path to scope the review to a specific folder (default: entire vault)

## Workflow

1. **Librarian** runs vault hygiene queries:
   - `obsidian "Obsidian" orphans` — notes with no incoming links
   - `obsidian "Obsidian" unresolved` — broken `[[wikilinks]]`
   - `obsidian "Obsidian" tasks | grep "\[ \]"` — incomplete tasks
   - `obsidian "Obsidian" backlinks path="index/Zettelkasten.md"` and other hubs
2. **Librarian** runs `link-suggest` over the hub notes to identify under-linked notes.
3. **Librarian** groups findings by severity:
   - **Broken**: unresolved links, malformed frontmatter
   - **Stale**: TODOs older than N days (or all open TODOs in the report), notes untouched for a long time
   - **Sparse**: permanent notes with very few inbound links, especially on hub topics
   - **Orphan**: notes with no inbound links and no obvious parent
4. **Librarian** produces a maintenance report at `sessions/{id}/queries/review-{date}.md`.

## Output

A markdown report with sections by severity, each finding actionable (which file, what to do). No vault writes.

## Cleanup

- Keep the report in `sessions/{id}/queries/` for the session.
