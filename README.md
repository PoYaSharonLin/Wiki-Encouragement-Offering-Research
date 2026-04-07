# RESEARCH-WIKI

A two-layer LLM-assisted research knowledge base for managing and synthesizing research materials.

---

## How It Works

```
raw/   ←  drop your source files here (PDFs, slides, notes)
wiki/  ←  LLM auto-generates and maintains structured knowledge pages here
```

You interact with this repo by giving Claude Code commands (listed below). Claude reads your source files, builds wiki pages, links concepts together, and keeps everything in sync.

---

## Folder Structure

```
RESEARCH-WIKI/
├── raw/
│   ├── done/              ← processed source files (moved here after ingest)
│   └── google-slides/     ← exported Google Slides / meeting PDFs
├── wiki/
│   ├── concepts/          ← one page per core concept
│   ├── references/        ← one page per paper / citation
│   ├── howto/             ← research methods and operation procedures
│   ├── evolution/         ← timeline of how research questions and methods evolved
│   ├── logs/              ← Q&A records
│   └── artifacts/         ← auto-generated summaries, slides, etc.
├── index.md               ← full site directory (always up to date)
├── log.md                 ← append-only operation timeline
├── CLAUDE.md              ← LLM behavior rules (do not edit casually)
└── README.md              ← this file
```

---

## Commands

Open Claude Code in this directory and type any of the following:

### Add new source material
```
Ingest new source: raw/google-slides/your-file.pdf
```
Claude will read the file, generate wiki pages for all concepts and references found, link them together, update the evolution timeline, and sync `index.md` and `log.md`. The source file stays untouched in `raw/`.

### Ask a question across all your notes
```
Query: <your question>
```
Claude cross-searches all wiki pages to answer, saves the Q&A to `wiki/logs/`, and writes any new insights back into relevant wiki pages.

### Add a paper
```
Add paper: raw/your-paper.pdf
```
or
```
Add paper: 10.xxxx/xxxxx   ← DOI
```
Claude creates a structured page in `wiki/references/`, tags it, and links it to related concepts.

### Check for broken links and outdated content
```
Lint
```
Claude scans all internal links, checks the evolution timeline for gaps, and verifies `index.md` matches the actual files.

---

## Workflow for Adding New Material

1. Drop your PDF or notes file into `raw/` (or a subfolder)
2. Run `Ingest new source: raw/your-file.pdf` in Claude Code
3. Review the new/updated pages in `wiki/`
4. Check `index.md` for the full updated directory

For PDFs specifically, see [`wiki/howto/pdf-ingest-procedure.md`](wiki/howto/pdf-ingest-procedure.md) for how Claude handles large files.

---

## Rules

| Rule | Reason |
|------|--------|
| Never edit files in `raw/` | Source files are the ground truth; wiki pages are derived from them |
| Never delete entries in `log.md` or `evolution/` | These are append-only timelines |
| Each concept gets exactly one wiki page | Use links, not duplication |
| All wiki content must cite a `raw/` source | No unsourced claims |

---

## Navigation

- **Browse all pages**: [`index.md`](index.md)
- **See what changed**: [`log.md`](log.md)
- **Understand the research arc**: [`wiki/evolution/research-question-evolution.md`](wiki/evolution/research-question-evolution.md)
- **Full LLM behavior rules**: [`CLAUDE.md`](CLAUDE.md)
