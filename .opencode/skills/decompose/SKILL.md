---
name: decompose
description: Given a long note (typically a synthesis or a literature note that grew unwieldy), propose splits into atomic permanent notes. Outputs: title, target folder, new note content, and the list of [[wikilinks]] to add back to maintain connectivity.
allowed-tools: Read Write Edit Bash
references:
  - ../_meta/vault.md
---

# Decompose

## Procedure

1. Read the target note.
2. Identify candidate atomic notes: distinct concepts that could stand alone.
3. For each candidate, output:
   - Title
   - Target folder (query `obsidian "Obsidian" folders` first)
   - Draft of the new note's content (extract relevant prose from the source)
   - List of `[[wikilinks]]` to add (back to source and to siblings)
4. Output the decomposition plan for user approval.
5. On approval, create the new notes via `obsidian "Obsidian" create path="..."`, then update the source note with `[[wikilinks]]` to the new ones.

## Rules

- Atomic = one idea per note. If a candidate is still too long, decompose further.
- Each new note should be linkable from at least 2 places.
- Preserve the source's epistemic register.
- Voice rules: see `_meta/voice.md`.
