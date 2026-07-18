# Knowledge Vault

This is a portable knowledge vault (Kiban) — a "Second Brain + LLM Wiki." Read `AGENTS.md`
in this directory for the complete schema governing all operations (ingest, query, lint, maintenance).

## Quick Reference

- Drop sources in `second-brain/wiki/raw/inbox/`, `work/wiki/raw/inbox/`, or `personal/wiki/raw/inbox/`.
- Wiki pages live in `*/wiki/pages/{sources,entities,concepts,synthesis,references}/`.
- Never modify files in `*/wiki/raw/` — those are immutable sources.
- Always update `*/wiki/index.md` and append to `*/wiki/log.md` after changes.
- Check `*/wiki/review.md` for items needing human judgment.
- `personal/` is **hybrid**: operational CRM (`people/`, `areas/`, `goals/`, `calendar/`) **+ a `wiki/`** for personal-life synthesis.
- One shared **`daily/`** note at the vault root captures work + personal; synthesis routes it.
- Other non-wiki folders: `profile/` (second-brain), `initiatives/`, `deliverables/`, `records/`,
  `artifacts/`. `projects/` (code) and `scratch/` (transient) sit at the root, outside the zones.

## Zones

| Zone | Path | Privacy |
|---|---|---|
| Second Brain | `second-brain/` | Private |
| Work | `work/` | Private |
| Personal | `personal/` | Private — hybrid: CRM (people/areas/goals/calendar) + `wiki/` |
| Toolkit | `toolkit/` | Private (your own skills/agents/context) |

## The 7 Anti-Forgetting Rules (AGENTS.md §4 — non-negotiable)

1. Append, don't rewrite
2. Wiki is map, not territory (verify against raw)
3. Search before create (no duplicates)
4. Flag contradictions, don't resolve silently
5. Provenance on every claim (`sources:` required)
6. One canonical location per entity
7. Raw is immutable
