# Research

The **workshop** — where investigation churns. Unlike the curated `*/wiki/` (the library),
research/ is active, messy, and project-scoped. Durable findings are **promoted** from workshop
to library when they're ready (see `AGENTS.md` §8 for the promotion rule).

## Layout

Each research project lives in its own folder:

```
research/<project-slug>/
  README.md      — project charter (type: research-project)
  raw/           — source material, clippings, PDFs, data
  notes/         — working notes (type: research-note)
  findings.md    — synthesized findings (type: research-finding)
```

## Special directories

- `_templates/` — starter templates for projects, notes, and findings.
- `_archive/` — completed or abandoned projects moved here to reduce noise.

## Workflow

1. Create a project folder from `_templates/research-project.md`.
2. Drop raw material in `raw/`, write working notes in `notes/`.
3. When a finding crystallizes, write it up as a `research-finding`.
4. Mark the finding `status: ready` (or tag `vault:promote`) to queue it for promotion.
5. The promotion process moves findings into the wiki as curated pages.
