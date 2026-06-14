---
name: literature-note
description: Given a Zotero item key (or a search query for new papers), produce a literature note in the Obsidian vault.
primary-agent: researcher
references:
  - ../_meta/vault.md
  - ../_meta/voice.md
  - ../_meta/tags.md
  - ../_meta/zotero.md
  - ../.opencode/agents/researcher.md
  - ../.opencode/agents/writer.md
  - ../.opencode/agents/librarian.md
  - ../skills/zotero-use/SKILL.md
  - ../skills/markitdown/SKILL.md
  - ../skills/tag-note/SKILL.md
---

# Literature Note

## Inputs

- A Zotero item key (preferred), or
- A search query (the librarian finds candidates; user picks one)

## Workflow

1. **Librarian** retrieves the item's metadata and PDF via the `zotero-use` skill.
2. **Librarian** extracts full text via the `markitdown` skill and writes to `sessions/{id}/inputs/{cite-key}.md`. The PDF is kept at `sessions/{id}/inputs/{cite-key}.pdf`.
3. **Researcher** reads the full text, plans the note structure, and writes an outline to `sessions/{id}/drafts/{cite-key}-outline.md`.
4. **Writer** reads `_meta/voice.md` and composes the draft to `sessions/{id}/drafts/{cite-key}-draft.md`.
5. **Researcher** inspects the draft. If revisions are needed, the writer iterates.
6. **Writer** applies the `tag-note` skill (reads `_meta/tags.md`, proposes 1–4 tags, confirms with user if creating a new note).
7. **User** approves the draft.
8. **Writer** writes to Obsidian. Confirm destination with the user before writing; default is `references/{cite-key}.md` if the `references/` template exists, otherwise follow user instruction.

## Output

A literature note in the Obsidian vault, with:

- Source metadata from Zotero (cite-key, authors, year, journal)
- Faithful prose following `_meta/voice.md`
- Frontmatter with tags from `tag-note`

## Cleanup

- Keep `sessions/{id}/inputs/{cite-key}.pdf` and `{cite-key}.md` for reference during the session.
- Drafts in `sessions/{id}/drafts/` can be deleted after the note is committed to Obsidian.
- `sessions/{id}/tmp/` is disposable; clean up when the session ends.
