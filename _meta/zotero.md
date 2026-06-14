# Zotero

Local zotero configuration. PDFs are stored in a Google Drive folder synced to the local machine.

## Library

- CLI: `zot` (Pyzotero)
- Local config: `--local --library-id 0 --library-type user`
- PDF base directory: [Google Drive path]

## Cite-key conventions

[How cite-keys are formatted, e.g. `authorYearShortTitle` from BetterBibTeX]

## Usage rules

- Do not modify the library unless the user explicitly asks.
- Use the `zot` CLI via the `zotero-use` skill.

## Common operations

```bash
zot --local --library-id 0 --library-type user items list --query "..." --qmode everything --limit 10 --output table
zot --local --library-id 0 --library-type user items get ITEM_KEY --output table
zot --local --library-id 0 --library-type user items children ITEM_KEY --output table
zot --local --library-id 0 --library-type user files download ATTACHMENT_KEY -o ./paper.pdf
zot --local --library-id 0 --library-type user fulltext get ATTACHMENT_KEY
zot --local --library-id 0 --library-type user items bib ITEM_KEY --output bibtex
zot --local --library-id 0 --library-type user items citation ITEM_KEY --output csljson
```

Use `--output table` for human-readable summaries, `--output json` for machine processing.
