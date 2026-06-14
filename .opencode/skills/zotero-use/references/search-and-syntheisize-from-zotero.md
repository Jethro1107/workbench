### Search and Retrieve Workflow

- Load the `Zotero Use` skill to understand how to interact with the zotero library via the cli.

#### Workflow

1. Start with metadata for candidate parent bibliographic items. Search broadly, then narrow by title, creator, collection, tag, item type, or year.
2. Prefer parent bibliographic item keys over attachment keys for citation decisions.
3. For a single selected item, always check children/attachments and report PDF availability.
4. For brainstorming/review, use the abstract plus PDF/full text when available.
5. Fetch full text selectively; summarise or save output to session-workspace. Do not paste long full text.
6. Separate what the reference says from your inference.
7. Adapt brainstorming to the user's current question and preferred style.
8. Do not create, update, delete, tag, or otherwise modify Zotero library items unless the user explicitly asks for library modification.

#### Pyzotero CLI Commands

Search:

```bash
zot --local --library-id 0 --library-type user items list --query "artificial intelligence ophthalmology" --qmode everything --filter-item-type journalArticle --limit 10 --output table
```

Get metadata:

```bash
zot --local --library-id 0 --library-type user items get FJRYBQGQ --output table
zot --local --library-id 0 --library-type user items get FJRYBQGQ --output json
```

Get children and PDF availability:

```bash
zot --local --library-id 0 --library-type user items children FJRYBQGQ --output table
```
Download PDF, when needed:

```bash
zot --local --library-id 0 --library-type user files download ATTACHMENT_KEY -o ./paper.pdf
```

Full text, when available:

```bash
zot --local --library-id 0 --library-type user fulltext get ATTACHMENT_KEY --output raw_content
```

#### Brainstorming Output

Do not impose a fixed brainstorming framework. The user's current task determines the format.

For each cited/reviewed reference, include a compact identity line:

Year - First author - Title - Publication/Journal
Then, when available from the abstract or manuscript, present:

1. study type/design
2. where and when the study was done
3. study population
4. sample size
5. PDF availability

Then provide results/recommendations in the current context. Examples of context-dependent outputs:

- whether the item is worth citing at the current sentence
- what claim it supports
- how it compares with another candidate reference
- what limitation or caveat matters for this argument
- whether to search for a stronger or newer reference
- When summarizing evidence from a reference, use bullet points and support them with numbers from the abstract or manuscript when available, such as sample size, sensitivity, specificity, AUC, confidence intervals, dates, number of included studies, or effect sizes.

Do not overstate evidence from abstracts alone. Say whether the summary used metadata/abstract only or abstract plus PDF/full text.