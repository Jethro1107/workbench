---
name: session-workspace
description: Use at the start of every new session. Creates a timestamped folder under ./sessions/ (id format YYYY-MM-DD_HHMMSS) to hold all session artefacts — notes, inputs, drafts, queries, scratch — so work stays organised and traceable. Trigger on "new session", "start working on X", or any first message that begins substantive work.
allowed-tools: Bash Write
---

# Session Workspace

## Purpose

At the start of every new session, create a folder for this session's work, named with the wall-clock time. All artefacts (notes, inputs, drafts, queries, scratch files) go inside; the project root is left clean.

## Procedure

1. **Generate the session id**:
   - PowerShell: `(Get-Date -Format 'yyyy-MM-dd_HHmmss')`
   - Bash: `date +%Y-%m-%d_%H%M%S`
   - Result example: `2026-06-11_143052` (sortable, no spaces, no colons — Windows-safe).

2. **Create the session folder**:
   ```bash
   mkdir -p ./sessions/<id>
   ```

3. **Write a session header** at `./sessions/<id>/notes.md` with:
   - the session id and timestamp,
   - the user's opening prompt (verbatim or near-verbatim),
   - a one-line intent ("synthesis on X", "lit note for paper Y", etc.).

   Update `notes.md` as the session progresses — append milestones, decisions, blockers, and a final summary.

4. **Optional descriptive suffix**: if the user names the session (e.g. "synthesis on antidepressants"), append a slug after the id:
   `./sessions/2026-06-11_143052_synthesis-antidepressants/`
   The timestamp stays the unique id; the slug is a human label.

5. **Use subfolders** as needed:
   - `inputs/` — downloaded papers, external data
   - `drafts/` — intermediate outputs
   - `queries/` — saved query results
   - `tmp/` — disposable scratch

6. **Cleanup**: when the session's work is finalised (notes written, drafts committed), the session folder can be deleted. Do not delete it mid-session — agents and follow-up turns may still need to read from it.

## Conventions

- This skill is stateless. It runs the date command and creates a directory; it does not need to be re-run within a session.
- The `./sessions/` folder is gitignored by convention.
- The literature-note workflow (see `.opencode/commands/literature-note.md`) uses `sessions/{id}/inputs/{cite-key}.pdf` and `sessions/{id}/inputs/{cite-key}.md` for source artefacts and `sessions/{id}/drafts/{cite-key}-*.md` for the outline and draft. No top-level `tmp/` directory is used.
- One session = one folder. Don't spread a session's work across multiple timestamped folders.
