# AGENTS.md

This is a **wiki repo** — not a code project. There is no build system, no tests, no linting, no CI. The agent's job is to maintain interlinked Markdown study pages.

## What this is

Obsidian wiki for the **Claude Certified Architect – Foundations (CCA-F)** exam. User curates raw material into `sources/`; the agent compiles it into `wiki/` following the schema in `CLAUDE.md`. A `derived/` folder holds build outputs (web app, slide decks).

## Core rule: CLAUDE.md is the schema

`CLAUDE.md` at the repo root is the authoritative source of truth for page structure, placement rules, workflows, and style. Read it before doing anything. This file (AGENTS.md) only supplements it with agent-specific gotchas.

## Hard invariants

- **Never write to `sources/`.** Read only. If a source needs cleanup, copy it elsewhere.
- **Always update `wiki/log.md`** after any wiki change (append-only, dated).
- **Always update `wiki/index.md`** if a new page is created.
- **Page placement is domain-first**, not type-first. See CLAUDE.md "Organizing principle" section.
- **Every page needs**: H1, blockquote one-liner, ≥ 2 outgoing `[[wiki-links]]`, `## Sources`, `## Continue reading`.
- **File names**: `kebab-case.md`. No spaces, no uppercase.

## Workflow: ingest

When a new source appears in `sources/`:

1. Read it. Create a `*.meta.md` sibling if missing.
2. Decide: extend existing page or create new one?
3. Place by domain folder (or `start-here/` for cross-cutting, or `reference/` for entities).
4. Write the page per CLAUDE.md templates.
5. Append to `wiki/log.md`.
6. Update `wiki/index.md` if new page created.

## Workflow: lint

Periodic hygiene — verify every page has required sections, no orphan pages, no dead `[[wiki-links]]`, correct folder placement. See CLAUDE.md "lint" workflow.

## Workflow: recompile

Treat `wiki/` as ephemeral. Delete and rebuild from `sources/` + `CLAUDE.md`.

## Derived outputs

`derived/claude-architect-foundations/` contains:
- `app/` — learner web app (static HTML/JS, served via `python3 -m http.server`)
- `slides/` — `.pptx` deck built by `build_deck.py` (requires `.venv/` with python-pptx)

Rebuild deck: `cd derived/claude-architect-foundations/slides && ./.venv/bin/python build_deck.py`

## Obsidian config

Graph is configured in `.obsidian/graph.json` with color groups by domain folder. `workspace.json` is gitignored (personal layout). Do not modify `.obsidian/` unless intentionally changing graph display.

## Gotchas

- Source PDFs are gitignored (`sources/*.pdf`). The `.meta.md` files are the citation layer.
- `CCA-F Notes.md` in the repo root is empty/stale — not a source file.
- The older `CCA-F Exam Notes.md` in `sources/` is a markdown export that some wiki pages still cite. The newer `CCA-F Notes.pdf` (with `.meta.md`) is the authoritative source — prefer citing it when updating pages.
- Wiki-links resolve by **filename**, not path. Moving a page between folders doesn't break links as long as the filename stays the same.
- Domain hubs are named identically to their folders (folder-note convention). E.g., `domain-1-agentic-architecture/domain-1-agentic-architecture.md`.
