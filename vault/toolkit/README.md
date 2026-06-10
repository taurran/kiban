# Toolkit

Your personal AI additions — the skills, agents, and context files you author or customize.
This zone has **no wiki**; it is not ingested.

## Structure

- `skills/` — your custom skills (a folder with a `SKILL.md`, or a single `.md` file, per skill)
- `agents/` — your custom scheduled or event-driven agents
- `context/` — your custom always-on context files

## Relationship to the shipped kit

The PokeVault kit ships reference skills in the project's top-level `skills/` folder
(`vault-init`, `wiki-ingest`, `wiki-lint`, `daily-note`, `obsidian-setup`, `profile-build`).
Copy any of them here to customize — your copies live in `toolkit/` and are never overwritten by
a PokeVault update.

## Distribution

`toolkit/` is private to you. It is not shared and not touched by updates.
