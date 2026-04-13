# CLAUDE.md — System Prompt & Schema Rules

> This file is the complete behavioral specification for the LLM operating this Wiki. The LLM must read this file before executing any command.

---

## 1. Core Philosophy

### Source 1: Karpathy LLM Wiki Design Principles
- **Raw layer is immutable**: All files in `raw/` are read-only — never edit or delete them
- **Knowledge layer evolves continuously**: `wiki/` is the only place the LLM is allowed to write and update
- **Every operation must be logged**: Any ingest, update, or query must append a timestamped entry to `log.md`
- **index.md is the front door**: After every update, the relevant sections of `index.md` must be kept in sync
- **Prefer internal links**: Wiki pages should cross-reference each other using `[[relative-path]]` or `[text](relative-path)` wherever possible

### Source 2: gigaai.studio Two-Layer Separation + Closed-Loop Pipeline
- **L0 Raw Layer** (`raw/`): Entry point for all external materials — categorize only, never modify
- **L1 Knowledge Layer** (`wiki/`): Structured knowledge base where the LLM actively transforms content and builds relationships
- **Closed-loop principle**: The knowledge base strengthens with use — every valuable insight from a Query must be written back into the wiki

---

## 2. Five-Stage Pipeline (run in full for every Ingest)

```
Capture → Compile → Link → Query → Feedback → Lint
```

| Stage | Action | Output Location |
|-------|--------|-----------------|
| **Capture** | Place material in `raw/` without categorizing or modifying | `raw/` |
| **Compile** | LLM analyzes material and generates structured pages | `wiki/concepts/`, `wiki/references/`, `wiki/howto/` |
| **Link** | Establish bidirectional links, update See Also on related pages | All wiki pages |
| **Query** | Cross-reference all notes to answer questions | `wiki/logs/YYYY-MM-DD-qna-XXX.md` |
| **Feedback** | Write valuable findings back into the wiki | Relevant wiki pages |
| **Lint** | Check for broken links, contradictions, and stale content | Lint section of `log.md` |

---

## 3. Folder Structure & Purpose

```
RESEARCH-WIKI/
├── raw/
│   ├── done/              ← Processed source files moved here (move only, never modify)
│   └── google-slides/     ← Weekly exported Google Slides (YYYY-MM-DD-vX.md)
├── wiki/
│   ├── concepts/          ← Core concept pages (one .md file per concept)
│   ├── references/        ← Paper and citation pages (one .md file per paper)
│   ├── howto/             ← Research methods and operational workflows
│   ├── evolution/         ← Research evolution section (two fixed core files)
│   ├── logs/              ← Q&A records and operation logs
│   └── artifacts/         ← Auto-generated outputs: slides, summaries, derivatives
├── index.md               ← Full site index + quick navigation (must always be current)
├── log.md                 ← Chronological operation log (append only, never delete)
└── CLAUDE.md              ← This file
```

---

## 4. File Schema Specifications

### concepts/ page format
```markdown
---
title: Concept Name
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: [tag1, tag2]
source: raw/google-slides/XXXX.md
---

# Concept Name

## Definition
...

## Role in the Research
...

## Evolution Context
(How this concept has changed as the research has progressed)

## See Also
- [[related concept]]
- [[related paper]]
```

### references/ page format
```markdown
---
title: Paper Title
authors: [Author1, Author2]
year: YYYY
venue: Conference/Journal Name
created: YYYY-MM-DD
tags: [tag1, tag2]
---

# Paper Title

## Core Contribution
...

## Relevance to This Research
...

## Key Methods
...

## See Also
- [[related concept]]
- [[related method]]
```

### howto/ page format
```markdown
---
title: Method Name
created: YYYY-MM-DD
updated: YYYY-MM-DD
---

# Method Name

## Applicable Scenarios
...

## Steps
1. ...
2. ...

## Notes & Caveats
...

## See Also
```

### evolution/ update format (append to end of file)
```markdown
### YYYY-MM-DD Update
- Research question shifted from "..." → "..."
- Research method shifted from "..." → "..."
- New concept added: [[concept name]]
- Source: [raw/google-slides/XXXX.md](../raw/google-slides/XXXX.md)
```

---

## 5. Command Set

### `Ingest new source: <path>`
Run the full five-stage pipeline:
1. Read the specified file under `raw/`
2. Compile: generate or update pages in `wiki/concepts/`, `wiki/references/`, `wiki/howto/`
3. Link: update bidirectional links on related pages
4. Update `wiki/evolution/research-question-evolution.md`
5. Update `wiki/evolution/research-method-evolution.md`
6. Update `index.md`
7. Write an entry to `log.md`
8. Move the processed source file to `raw/done/` (if appropriate)

### `Query: <question>`
1. Cross-reference all relevant pages in `wiki/`
2. Generate an answer
3. Save the Q&A to `wiki/logs/YYYY-MM-DD-qna-001.md`
4. If the answer contains new insights, write them back to relevant wiki pages

### `Add paper: <path or DOI>`
1. Create a paper page in `wiki/references/`
2. Auto-assign tags
3. Link to related concepts/howto pages
4. Update the References section of `index.md`
5. Write an entry to `log.md`

### `Lint`
1. Scan all internal wiki links and report broken ones
2. Check the `evolution/` timeline for gaps
3. Verify `index.md` is in sync with actual files
4. Write a Lint report to `log.md`

---

## 6. log.md Entry Format

Every operation must append the following to `log.md`:

```markdown
## YYYY-MM-DD HH:MM — <Operation Type>

- **Operation**: Ingest / Query / Add paper / Lint
- **Source**: `raw/...` (if applicable)
- **Output**: List of new or updated wiki pages
- **Summary**: One sentence describing the core change from this operation
```

---

## 7. LLM Behavioral Rules

1. **Cite, don't speculate**: All wiki content must include a source link pointing to the original material in `raw/`
2. **Link, don't duplicate**: Each concept gets exactly one page; all other references use links
3. **Append, never overwrite**: `log.md` and `evolution/` are append-only — never delete old content
4. **Updates must sync**: After updating any wiki page, always sync `index.md` and `log.md`
5. **Stay focused**: Each page covers a single topic — avoid cramming everything into one page
