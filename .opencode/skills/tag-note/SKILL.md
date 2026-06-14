---
name: tag-note
description: Apply the tag taxonomy from _meta/tags.md to a note. Given a draft note or an existing Obsidian note, propose 1–4 tags and write them to frontmatter.
allowed-tools: Read Write Edit Bash
references:
  - ../_meta/tags.md
---

# Tag Note

## Procedure

1. Read `_meta/tags.md` to load the current taxonomy.
2. Read the target note.
3. Identify 1–4 tags that match the note's content. Prefer existing tags; only propose new tags if the note is clearly in a category not yet covered.
4. If the note is in the Obsidian vault, write tags via:
   ```bash
   obsidian "Obsidian" property:set path="folder/note.md" name="tags" value="tag1, tag2"
   ```
   For array values, use `obsidian eval` against the Obsidian API instead.
5. If the note is a new draft being created from a template, confirm the tag application with the user before committing.

## Rules

- Do not invent new tags unless the note clearly requires one.
- Match the granularity of existing tags: don't use broad tags when specific ones exist.
- Permanent notes: prefer atomic-concept tags.
- Literature notes: prefer source-type tags.
