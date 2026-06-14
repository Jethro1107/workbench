---
name: consistency-check
description: Audit the workbench harness. Reads .opencode/ and _meta/, reports duplicated facts across files, broken references, permission mismatches, and command-body skill names that don't resolve. Read-only across the harness; does not touch zotero or the Obsidian vault.
allowed-tools: Read Glob Grep Bash
---

# Consistency Check

## Procedure

1. Inventory: list every file under `.opencode/agents/`, `.opencode/skills/`, `.opencode/commands/`, and `_meta/`.
2. For each agent, command, and skill file, parse the `references:` frontmatter array. For each referenced path, verify the file exists and the named content is there.
3. For each agent, cross-check its `permissions.skills` list against the set of skills actually used in command bodies and the set of skills the agent's own body mentions.
4. Scan agent, command, and skill bodies for any inline listing of content that already lives in a `_meta/` file (e.g. folder lists, tag taxonomies, voice rules). Flag duplications.
5. Scan command bodies for skill names that don't match any directory under `.opencode/skills/`.
6. Output a report grouped by severity: broken, mismatch, duplicated, ok.

## Output

A markdown report with sections for each severity. Each finding includes the file path, the line number or frontmatter key, and a one-line description of the issue. No auto-fix in v1.

## Rules

- Read-only. Never write to `.opencode/`, `_meta/`, the Obsidian vault, or zotero.
- The skill is designed to be promotable to an agent (e.g. `curator`) if scheduled audits or auto-fix are needed later. Keep the procedure self-contained so the promotion is mechanical.
- Report only; do not silently fix.
