---
name: provenance
description: Given a permanent note, trace each factual claim back to a literature note and ultimately to a Zotero item key. Outputs a provenance map for auditing.
allowed-tools: Read Bash
references:
  - ../_meta/zotero.md
---

# Provenance

## Procedure

1. Read the target note.
2. For each factual claim (assertion that isn't common knowledge), find the literature note that supports it.
3. For each literature note, find the Zotero item key it cites.
4. Output a provenance map: `claim -> lit-note -> Zotero key`.
5. Flag any claims that cannot be traced to a Zotero item.

## Rules

- Common knowledge doesn't need a citation.
- Distinguish three states:
  - claim supported by lit-note (good)
  - claim supported only by another permanent note (weak)
  - claim with no support (gap)
- Don't fill gaps; report them.
- Local zotero access: see `_meta/zotero.md`.
