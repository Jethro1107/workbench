---
name: synthesis
description: Given a topic or a permanent note, survey related literature notes in Obsidian, identify gaps, produce a thematic synthesis outline, and write a new permanent note.
primary-agent: researcher
references:
  - ../_meta/vault.md
  - ../_meta/voice.md
  - ../_meta/tags.md
  - ../_meta/zotero.md
  - ../.opencode/agents/researcher.md
  - ../.opencode/agents/writer.md
  - ../.opencode/agents/librarian.md
  - ../.opencode/skills/obsidian-cli/SKILL.md
  - ../.opencode/skills/zotero-use/SKILL.md
  - ../.opencode/skills/link-suggest/SKILL.md
  - ../.opencode/skills/decompose/SKILL.md
  - ../.opencode/skills/tag-note/SKILL.md
---

# Synthesis

## Inputs

- A topic (free text), or
- A path to an existing permanent note in the Obsidian vault

## Workflow

1. **Researcher** queries Obsidian (`obsidian "Obsidian" search query="..."`) for related literature notes and permanent notes.
2. **Librarian** queries Zotero (via `zotero-use`) for related sources not yet in the vault; flags gaps.
3. **Researcher** reads the relevant notes and produces a thematic outline at `sessions/{id}/drafts/{topic-slug}-outline.md`. The outline is thematic, not paper-by-paper.
4. **User** approves the outline.
5. **Writer** reads `_meta/voice.md` and composes the synthesis at `sessions/{id}/drafts/{topic-slug}-draft.md`. Uses `decompose` if the synthesis is dense enough to warrant atomic splits; uses `link-suggest` to propose `[[wikilinks]]` to related notes.
6. **Researcher** inspects the draft.
7. **Writer** applies `tag-note`.
8. **User** approves the draft.
9. **Writer** writes to Obsidian. Confirm destination with the user; default is `main/{topic-slug}.md`.

## Output

A permanent note synthesising the topic, with:

- Thematic structure (not paper-by-paper)
- `[[wikilinks]]` to all sources and related permanent notes
- Frontmatter with tags from `tag-note`
- Possibly: new atomic notes created via `decompose`

## Cleanup

- Drafts can be deleted after the note is committed.
- `sessions/{id}/queries/{topic-slug}.md` (the gap-analysis report) is worth keeping for the session.
