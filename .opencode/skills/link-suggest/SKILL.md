---
name: link-suggest
description: Given a note (draft or existing), find related notes in the Obsidian vault and propose [[wikilinks]] to add. Uses _meta/tags.md and the vault's existing links to identify candidates.
allowed-tools: Read Bash
references:
  - ../_meta/tags.md
  - ../_meta/vault.md
---

# Link Suggest

## Procedure

1. Read `_meta/tags.md` and `_meta/vault.md` to understand the taxonomy and vault structure.
2. Read the target note.
3. Use `obsidian "Obsidian" search query="..."` to find notes with overlapping tags or shared concepts.
4. For each candidate link, evaluate whether the `[[wikilink]]` would strengthen the graph.
5. Output a list of proposed links with a one-line justification for each.
6. Do not auto-apply; propose for user approval.

## Rules

- Prefer links that go both ways: the target note should also link back to the source.
- Don't suggest links to notes that are clearly out of scope.
- Quality over quantity: 1–3 strong links beats 10 weak ones.
- Verify the candidate note exists via `obsidian "Obsidian" files` before proposing.
