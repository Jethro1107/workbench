---
name: literature-note
description: Given a Zotero item key (or a search query for new papers), produce a source-faithful literature note in the Obsidian vault through a phased, collaborative workflow with the user.
primary-agent: researcher
references:
  - ../_meta/vault.md
  - ../_meta/tags.md
  - ../_meta/zotero.md
  - ../.opencode/agents/researcher.md
  - ../.opencode/agents/writer.md
  - ../.opencode/agents/librarian.md
  - ../.opencode/skills/zotero-use/SKILL.md
  - ../.opencode/skills/markitdown/SKILL.md
  - ../.opencode/skills/tag-note/SKILL.md
  - ../.opencode/skills/link-suggest/SKILL.md
  - ../.opencode/skills/session-workspace/SKILL.md
---

# Literature Note

A source-faithful literature note produced through phased collaboration between the user, the researcher, the writer, and the librarian. The user is the analyst; the agents facilitate. Every phase has an explicit gate — there is no silent auto-advance.

You are the researcher and the orchestrator of the workflow. You are not allowed to read skills that you yourself would not require. Invoke subagents with all necessary information including files to read, the task at hand, the skills needed, and the files to write.

User requests:
$ARGUMENTS

## Inputs

- A Zotero item key (preferred), or
- A search query (the librarian finds candidates; user picks one)

## Phases

### Phase 0 - Workspace setup

Create a new workspace for this literature note session using the `session-workspace` skill. This will be the central hub for all files and interactions related to this note-taking session. Please inform subagents to read from and write to this workspace for all subsequent phases.

### Phase 1 — Resolve and extract (librarian, autonomous)

You dispatches the librarian with:

```text
The current session workspace is: `session/{session-id}/`. Please provide the source material to this session.

The user has requested a to take literature notes on the following paper: {item-key or search query}. Please retrieve the Zotero item for this paper by resolving the paper using the `zotero-use` skill.

Get relevant metadata including cite-key, title, authors, and bibliography. Obtain the path to the source PDF.

Next, run the `markitdown` skill on the source PDF to extract the full text and write it to a markdown file in the session workspace at `session/{session-id}/inputs/{cite-key}.md`.

Report back to me with the following information: `cite-key`, `title`, `authors`, `bibliography`, and the path to the markdown file containing the full text.
```

1. Librarian retrieves the item's metadata and fulltext via the `zotero-use` skill.
2. Librarian runs `markitdown` on the source PDF and writes the full text to `sessions/{id}/inputs/{cite-key}.md`.
3. Librarian reports back: `cite-key`, `title`, `authors`, `bibliography`, `zotero_key`.

### Phase 2 — Outline (researcher + user, file-based)

Two sub-steps. The user edits files directly; the researcher reads them back.

**1a. Outline proposal.**

You read the file at `sessions/{id}/inputs/{cite-key}.md` incrementally.

Your task: identify all meaningful sections suitable for parallel note-taking.

Rules for what counts as a "meaningful section":

- Must have a visible section heading in the text.
- Do NOT include: reference lists, conflict-of-interest statements, or acknowledgements sections.

You may suggest to merge or split sections if it would make the note-taking process more efficient. For example, if you see a section that is very long and covers multiple distinct topics, you might suggest splitting it into two sections. Conversely, if you see two sections that are very short and closely related, you might suggest merging them into one section.

Output format:

```markdown
## Outline

| # | Header | Lines |
|---|--------|-------|
| 1 | ... | 1-45 |
| 2 | ... | 46-89 |

## Changes Deviating from Original Sections
- Section list: `index`, `header`, `line range`, one-line content note.
- Suggested merges or splits with rationale (e.g. "merge 4a + 4b; 4a is 80 words and 4b is a continuation").
- ...
```

Save the outline proposal to `sessions/{id}/drafts/{cite-key}-outline.md` and inform the user to review and edit the file directly.

```

User may engage in free-form edits to the outline file, or engage in chat-based back-and-forth discussions with you to refine the outline. Await explicit user approval to move to the next step.

**1b. Briefs.**

After outline approval, researcher writes a content brief per section, in one batch, to `sessions/{id}/drafts/{cite-key}-section-{NN}-brief.md`.

Please note:

- What to capture, mechanisms, key claims, suggested sub-bullets (content-level only).
- No format or voice guidance — the writer applies voice rules independently on dispatch.

User edits briefs free-form. No length cap. The only constraint is that the brief carries enough information for the writer to draft the section. Approval can be section-by-section as drafting starts, or all-at-once.

### Phase 2 — Per-section drafting (researcher → writer → user, looped)

For each section `NN` in the approved outline:

1. Researcher dispatches the writer with:

```text
The current session workspace is: `session/{session-id}/`. Please read and write all files related to this session in this workspace.

The user is currently taking literature notes on the paper {title of paper}. Your task is to draft section {NN} of the literature note.

Information to use:
- The full text of the paper is located at `sessions/{id}/inputs/{cite-key}.md`.
- The outline of the paper, including section headers and line ranges, is located at `sessions/{id}/drafts/{cite-key}-outline.md`.
- The content brief for this section is located at `sessions/{id}/drafts/{cite-key}-section-{NN}-brief.md`.

Please write faithful notes for the section to `session/{id}/drafts/{cite-key}-section-{NN}-notes.md`. Make sure to stay true to the source material and the section-bounds (line ranges) defined in the outline.
```

2. Writer reads the full text, locates the section, writes `sessions/{id}/drafts/{cite-key}-section-{NN}-notes.md`.
3. User reviews the notes. **No silent auto-advance.**
  - Approval: user types `section NN approved` (or edits the file to satisfaction and signals).
  - Revision: user describes changes; researcher updates the brief if the change is analytical (what to capture), invoke the writer with necessary information to revise the notes if the change is prose-level.
4. **Iteration cap: 3 drafts per section** (initial + 2 revisions). After 3 drafts, surface to the user: "I think I'm at my limit on this section; proceed as-is, or take over and edit the file directly?"

Move to the next section on approval.

### Phase 3 — Consolidate (writer + researcher, conversational)

1. Researcher dispatches the writer:

```
The current session workspace is: `session/{session-id}/`. Please read and write all files related to this session in this workspace. The user has currently finished drating notes for all sections of the paper {title of paper}. Your task is to assemble the consolidated draft of the literature note.

Information to use:

- The full text of the paper is located at `sessions/{id}/inputs/{cite-key}.md`.
- The outline of the paper, including section headers and line ranges, is located at `sessions/{id}/drafts/{cite-key}-outline.md`.
- Section-specific notes are located at `session/{id}/drafts/{cite-key}-section-{NN}-notes.md` for each section.

Please read all section files in outline order and produce a cohesive manuscripts, trying to keep everything from the notes verbatim, but not a mechanical concatenation. Smooth transitions between sections, resolve any redundancy or gaps, maintain consistent voice and tense throughout.

```

2. You prepends a metadata block directly in the file:
  ```markdown
   cite-key: ...
   title: ...
   authors: ...
   bibliography: ...
   zotero_key: ...
   [Zotero link]
  ```
3. User reviews the full draft body. **Domain expertise enters here**: cross-references to other work, contradictions, real-world examples, links to existing vault notes.
4. Researcher drafts a 2–3 sentence high-level description (HLD) of the paper; user confirms or rewrites. The HLD is placed at the top of the note, directly under the title (after the metadata block, or merged into it).
5. Librarian runs `link-suggest` if the user wants.

### Phase 4 — Tag, approve, commit (librarian, writer)

Use the cite-key as the file name.

1. Researcher dispatches the librarian:
  > "Apply tags to the draft at `sessions/{id}/drafts/{cite-key}-draft.md`. Use the `tag-note` skill: read `_meta/tags.md`, propose 1–4 tags from the existing taxonomy, confirm with the user."
2. User approves tags.
3. User approves the final draft.
4. Researcher dispatches the writer:
  > "Write the final draft to Obsidian at `references/{cite-key}.md` via the `obsidian-cli` skill. Confirm the destination folder with the user first. Use the `references/` template."
5. User confirms the Obsidian write.

## File naming


| File                                                    | Purpose                              |
| ------------------------------------------------------- | ------------------------------------ |
| `sessions/{id}/inputs/{cite-key}.md`                    | Full text from Zotero                |
| `sessions/{id}/drafts/{cite-key}-outline.md`            | Section list (Phase 1a)              |
| `sessions/{id}/drafts/{cite-key}-section-{NN}-brief.md` | Content brief per section (Phase 1b) |
| `sessions/{id}/drafts/{cite-key}-section-{NN}-notes.md` | Prose draft per section (Phase 2)    |
| `sessions/{id}/drafts/{cite-key}-draft.md`              | Consolidated final draft (Phase 3)   |
| `references/{cite-key}.md`                              | Final vault location                 |


`NN` is a zero-padded 2-digit index (`01`, `02`, ...).

## Hard rules

- **No silent auto-advance.** Every phase gate is explicit. The user signals approval in chat or by editing files; the agent waits.
- **File-based collaboration for outline and briefs.** The user edits files directly in their editor; the agent team reads them back. Per-section notes and consolidation are reviewed file-or-chat, but the file is the source of truth.

## Output

A literature note in the Obsidian vault at `references/{cite-key}.md`, with:

- Title (from Zotero)
- High-level description (2–3 sentences, under title)
- Source metadata from Zotero
- Faithful prose in the voice defined by `_meta/voice.md`
- Frontmatter with tags from the `tag-note` skill

## Cleanup

- `sessions/{id}/inputs/{cite-key}.md` — keep for the duration of the session.
- `sessions/{id}/drafts/` — keep until the note is committed to Obsidian.
- `sessions/{id}/tmp/` — disposable; clean up at session end.

## Subagent Dispatch Summary


| Phase   | Subagent  | Dispatch prompt (template)                                                                                                                                                                                                                                                                            |
| ------- | --------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Phase 0 | librarian | "Retrieve the Zotero item for `{item-key}`. Get metadata and fulltext. Run `markitdown` on the PDF and write to `sessions/{id}/inputs/{cite-key}.md`. Report back: cite-key, title, authors, bibliography, zotero\_key."                                                                              |
| Phase 2 | writer    | "Draft section {NN}. Read brief at `sessions/{id}/drafts/{cite-key}-section-{NN}-brief.md` and full text at `sessions/{id}/inputs/{cite-key}.md`. Write to `sessions/{id}/drafts/{cite-key}-section-{NN}-notes.md` following `_meta/voice.md`."                                                       |
| Phase 3 | writer    | "Assemble the consolidated draft at `sessions/{id}/drafts/{cite-key}-draft.md`. Read all section files in outline order and produce a cohesive manuscript — not a mechanical concatenation. Smooth transitions, resolve redundancy/gaps, maintain consistent voice. Then prepend the metadata block." |
| Phase 4 | librarian | "Apply tags to `sessions/{id}/drafts/{cite-key}-draft.md` using `tag-note` skill. Propose 1–4 tags from `_meta/tags.md`, confirm with user."                                                                                                                                                          |
| Phase 4 | writer    | "Write final draft to Obsidian at `references/{cite-key}.md` via `obsidian-cli`. Confirm destination with user first. Use `references/` template."                                                                                                                                                    |


