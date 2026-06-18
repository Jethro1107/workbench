# Knowledge Work Workbench

A focused workbench for clinical research note-taking: read sources, take notes, integrate into a Zettelkasten.

## Layout

- Workspace root: `~/workbench`
- Obsidian vault: `~/Desktop/Obsidian` (vault name: `Obsidian`)
- Zotero: local instance, PDFs in Google Drive

## Agents

| Agent | File | Primary for |
|---|---|---|
| `researcher` | `.opencode/agents/researcher.md` | `literature-note`, `synthesis`, `integrate` |
| `writer` | `.opencode/agents/writer.md` | prose production (delegated) |
| `librarian` | `.opencode/agents/librarian.md` | `query`, `review`; retrieval and tagging |

Permissions, tools, and frontmatter are in each agent's file.

## Commands (slash commands)

| Command | Primary | Purpose |
|---|---|---|
| `/literature-note` | researcher | Given a Zotero key, produce a literature note in Obsidian |
| `/synthesis` | researcher | Given a topic, produce a permanent-note synthesis |
| `/integrate` | researcher | Given a fleeting/lit note, integrate into permanent notes |
| `/query` | librarian | Search zotero and obsidian; unified shortlist |
| `/review` | librarian | Vault audit: orphans, broken links, stale TODOs |

Workflows: `.opencode/commands/{name}.md`. Read the command file at the start of a workflow.

## Skills

| Skill | File | Purpose |
|---|---|---|
| `obsidian-cli` | `.opencode/skills/obsidian-cli/SKILL.md` | Vault traversal via the `obsidian` CLI |
| `zotero-use` | `.opencode/skills/zotero-use/SKILL.md` | Zotero search/retrieval via the `zot` CLI |
| `markitdown` | `.opencode/skills/markitdown/SKILL.md` | File-to-markdown conversion |
| `session-workspace` | `.opencode/skills/session-workspace/SKILL.md` | Session folder creation |
| `tag-note` | `.opencode/skills/tag-note/SKILL.md` | Apply tag taxonomy |
| `link-suggest` | `.opencode/skills/link-suggest/SKILL.md` | Propose `[[wikilinks]]` |
| `decompose` | `.opencode/skills/decompose/SKILL.md` | Split long notes into atomic notes |
| `provenance` | `.opencode/skills/provenance/SKILL.md` | Trace claims to Zotero items |
| `consistency-check` | `.opencode/skills/consistency-check/SKILL.md` | Audit the harness itself |

## Meta files (`_meta/`)

Cross-session, non-ephemeral knowledge. Read on demand.

| File | Read when |
|---|---|
| `_meta/user.md` | Session start (orientation) |
| `_meta/field.md` | Content-heavy work (domain context, learning goals) |
| `_meta/focus.md` | When prioritising; user-curated current focus |
| `_meta/vault.md` | Before any vault operation; confirms current structure |
| `_meta/tags.md` | Before applying tags |
| `_meta/voice.md` | Before composing any prose |
| `_meta/zotero.md` | Before any zotero operation; library config |

## Obsidian

- Vault name: `Obsidian` — always pass as the first argument to the `obsidian` CLI.
- Obsidian Desktop must be running for the CLI to work.
- Current vault state (folders, hubs, templates, note types): `_meta/vault.md`. Re-query `obsidian "Obsidian" folders` and `obsidian "Obsidian" templates` before relying on any path.
- For list values in frontmatter, use `obsidian eval` rather than `property:set`.

## Zotero

- Local zotero accessed via the `zot` CLI (Pyzotero).
- Common patterns: `_meta/zotero.md` and `.opencode/skills/zotero-use/SKILL.md`.
- Read-only by default.

## Session workspace

- Path: `sessions/{id}/` where id is `YYYY-MM-DD_HHMMSS`.
- Subfolders: `inputs/`, `drafts/`, `queries/`, `tmp/`.
- Gitignored (or to be added to `.gitignore` when the workspace becomes a repo).
- Do not delete mid-session.
- Do not write to a top-level `tmp/` at the workspace root.

## Hard rules

- **Never write to the Obsidian vault without user approval.** Compose locally in `sessions/{id}/drafts/` → researcher inspects → writer writes.
- **Never modify the Zotero library** unless the user explicitly asks.
- **Do not hard-code vault structure.** Read `_meta/vault.md` and re-query the vault.
- **Do not duplicate content across files.** If a fact lives in `_meta/`, the agent/prompt/skill file references it; it does not restate it.

## Single-source rule

Each piece of information lives in exactly one file:

- Role + permissions + workflow boundaries → agent file (`.opencode/agents/*.md`)
- Procedures for one capability → skill file (`.opencode/skills/<name>/SKILL.md`)
- Workflow orchestration → command file (`.opencode/commands/*.md`)
- Declarative knowledge about the world → meta file (`_meta/*.md`)
- High-level orientation + pointers → this file

Use inline prose references ("see `_meta/voice.md`") or the `## References (read on demand)` section in agent, prompt, and skill files. The `consistency-check` skill audits this rule and reports violations.

## VSCode workflow

The harness runs inside VS Code Copilot. The session workspace is visible in the editor; drafts can be edited in place; the agent reads them back via file tools. To collaborate on a fleeting note during a workflow, edit the file in the session's `drafts/` or `inputs/` folder directly.

## Run a `consistency-check`

To audit the harness itself (duplicated facts, broken references, unresolved skill names in prompt bodies), invoke the `consistency-check` skill. Read-only; does not touch zotero or the Obsidian vault.
