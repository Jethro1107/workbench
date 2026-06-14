---
name: zotero-use
description: Used for search/query/retrieve references from the user's Zotero library using Pyzotero CLI (`zot`), and brainstorm from Zotero evidence.
---
# Zotero Use

## Rules

Use Pyzotero CLI (zot) for library search/browse/metadata because it is structured. 
Treat Zotero library create/update/delete as secondary; do not modify the Zotero library unless the user explicitly asks.
Avoid direct local API wrappers and raw SQLite in this skill; use Pyzotero CLI instead.

## Work Pattern

- Search broadly, then narrow by title, creator, collection, tag, item type, or year.
- Inspect metadata for shortlisted parent bibliographic items; avoid citing attachment keys.
- For a single selected item, check children/attachments and report whether a PDF is available.
- For brainstorming/review, use the abstract plus PDF/full text when available; if only metadata/abstract was reviewed, say so.
- For Word work, edit text and add live Zotero citation fields with narrow OOXML changes; use structural validation by default.
- Report Zotero item keys with conclusions and citation-placement suggestions.
- Distinguish Zotero evidence from external knowledge or inference.

## PyZotero CLI Usage

The installed command is `zot`. Use the `--help` flag to understand the cli.

### Local Read-Only Mode

Use --local before the command group:

```bash
zot --local --library-id 0 --library-type user items list --limit 5 --output table
zot --local --library-id 0 --library-type user items list --query "glaucoma" --qmode everything --limit 5 --output table
zot --local --library-id 0 --library-type user items get QUDEVEEJ --output table
zot --local --library-id 0 --library-type user items children QUDEVEEJ --output table
zot --local --library-id 0 --library-type user collections list --top --output table
zot --local --library-id 0 --library-type user util item-types
```
Local mode is read-only. Use it for GET-style operations.

### Common Commands

```bash
zot --local --library-id 0 --library-type user items list --filter-item-type journalArticle --sort dateModified --direction desc --limit 10 --output table
zot --local --library-id 0 --library-type user collections list --top --output table
zot --local --library-id 0 --library-type user tags list --output table
zot --local --library-id 0 --library-type user items children ITEM_KEY --output table
zot --local --library-id 0 --library-type user files download ATTACHMENT_KEY -o ./paper.pdf
zot --local --library-id 0 --library-type user fulltext get ATTACHMENT_KEY
zot --local --library-id 0 --library-type user items bib ITEM_KEY --output bibtex
zot --local --library-id 0 --library-type user items citation ITEM_KEY --output csljson
Use table output for human-readable summaries and JSON for machine processing.
```

Do not paste large JSON or full text unless the user asks for it.

## References and Workflows
Search and Synthesize from Zotero (`references/search-and-synthesize-from-zotero.md`) - structured workflow for searching, retrieving and synthesizing information from zotero evidence.