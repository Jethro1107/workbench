# Voice

Canonical writing voice for prose that enters the Obsidian vault. The writer agent reads this file before composing; treat its rules as binding.

## Register

- Authoritative, precise, information-dense, mechanistic.
- Every sentence carries weight. Focus on the *why* and *how*, not just the *what*.

## Shorthand

Use standard academic and domain-specific shorthand fluently, without expanding on first use.

- Connectives: i.e., e.g., vs., n.b., cf., et al., viz.
- Modifiers: incl., esp., w/, w/o, a/w, >, <.

## Format

Hybrid prose-clauses and deeply nested bullets. Do not default to standalone bold headers followed by block prose.

### Hybrid bullet

`- **Concept** - prose explanation:` followed by a nested list of sub-points.

```text
- **Cognitive Biases** - systematic patterns of deviation from norm or rationality in judgment:
    - _Anchoring_ - over-reliance on the first piece of information offered.
    - _Confirmation bias_ - tendency to search for, interpret, and recall info in a way that confirms preexisting beliefs.
        - *High prognostic value* in clinical decision-making contexts.
```

### Emphasis hierarchy

1. **Bold** — primary categories, structural anchors, major headings, core concepts.
2. *Italics* — sub-categories, specific classifications, critical implications, key terminology.

Avoid italics for entire sentences. Italics are for specific phrases or terms.

### Sectioning

- Default to the hybrid bullet body.
- Reserve H2 for genuinely distinct, multi-paragraph blocks.
- Most reference and synthesis notes: one or two H2s at most; the rest carried by the bullet hierarchy.

## Rules

- **Faithfulness first.** Every claim must be traceable to a source. Flag ambiguity with "The text suggests…" or "The authors appear to argue…".
- **Don't manufacture structure.** Let the source's content determine the form. No boilerplate scaffolds.
- **Preserve epistemic register.** Hedged sources get hedged notes; definitive sources get definitive notes.
- **Resolve exhibits inline.** "Figure 2 shows that…" must describe what Figure 2 actually shows. Never write "as shown in Figure 2" without explaining.
- **No preamble, no postamble.** The note begins at the title and ends with the last line of substance.
- **Use the vault's own conventions.** Read a few existing notes in the target folder before writing; mirror their frontmatter, tags, and naming.
