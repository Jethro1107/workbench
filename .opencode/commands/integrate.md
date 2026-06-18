---
name: integrate
description: Given a fleeting note (or a new literature note), identify which permanent notes should be updated, propose atomic splits via the decompose skill, and draft the changes for review.
primary-agent: researcher
references:
  - ../_meta/vault.md
  - ../_meta/voice.md
  - ../_meta/tags.md
  - ../.opencode/agents/researcher.md
  - ../.opencode/agents/writer.md
  - ../.opencode/agents/librarian.md
  - ../.opencode/skills/obsidian-cli/SKILL.md
  - ../.opencode/skills/link-suggest/SKILL.md
  - ../.opencode/skills/decompose/SKILL.md
  - ../.opencode/skills/tag-note/SKILL.md
  - ../.opencode/skills/provenance/SKILL.md
---

# Integrate

- Prior to any workflow:
  - Confirm whether a session workspace has been created for the current session
  - If it has not been created, run the `session-workspace` skill

## Inputs

- A fleeting note (typically from a daily note), or
- A path to a new literature note that should be integrated

## Workflow

1. **Researcher** reads the source material.
2. **Librarian** scans the vault using the `obsidian-cli` skill (`obsidian "Obsidian" search`) for permanent notes that are related.
3. **Researcher** produces an integration plan at `sessions/{id}/drafts/{date}-integrate-plan.md` with one entry per affected permanent note. Each entry lists:
   - The note's path
   - The proposed change type: (a) edit, (b) atomic split via `decompose`, (c) new permanent note
   - A draft of the change
4. **Researcher** runs `provenance` skill over the affected notes to verify the integration is properly sourced.
5. **Researcher** runs `link-suggest` skill to propose `[[wikilinks]]` between affected notes and the new content.
6. **User** reviews and approves the plan.
7. **Writer** applies the edits and creates any new notes. For new notes, the writer reads `_meta/voice.md` and applies `tag-note`.
8. **Writer** writes to Obsidian.

## Output

Edits to existing permanent notes, possibly new atomic permanent notes, and `[[wikilinks]]` between them. All changes sourced to the original fleeting/literature note.

## Cleanup

- Keep the integration plan in `sessions/{id}/drafts/` for the duration of the session for traceability.
