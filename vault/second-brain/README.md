# Second Brain

Your cognitive fingerprint and personal knowledge base. Strictly private — never shared.

> **Engine:** all wiki operations here follow `../AGENTS.md`. Read it before ingesting or editing pages.

## Structure

- `profile/` — **WHO YOU ARE:** identity, voice, preferences, goals, relationships, projects, patterns.
  Agents read these to personalize responses in your voice and priorities.
- `wiki/` — **WHAT YOU KNOW:** a personal LLM wiki (Karpathy pattern).
  - `wiki/raw/inbox/` — drop articles, clippings, ideas here
  - `wiki/raw/notes/` — quick notes + voice transcripts (`source: note|voice`)
  - `wiki/raw/media/` — screenshots/whiteboards (+ a `.md` sidecar)
  - `wiki/pages/{sources,entities,concepts,synthesis,references}/` — synthesized pages
- `initiatives/` — lightweight personal project/goal tracking (links to work/, never duplicates).
- `artifacts/` — diagrams, exports, and other personal artifacts.

## Privacy

Nothing in this zone is auto-published or shared. To make something shareable, you move it
deliberately (and sanitize it first).

## Workflow

1. Drop sources in `wiki/raw/inbox/`.
2. Ask your assistant to "process my second-brain inbox" (uses `../AGENTS.md`).
3. Browse the graph view in Obsidian.
4. Keep `profile/` current as your role, voice, and priorities evolve.
