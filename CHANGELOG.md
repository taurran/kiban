# Changelog

All notable changes to this project are documented here.
The format follows [Keep a Changelog](https://keepachangelog.com/), and the
project adheres to [Semantic Versioning](https://semver.org/) per the operator's
convention (major = new phase / breaking change, minor = feature, patch = fix).

> **Provenance note.** This project was developed under the name **PokeVault**
> through v1.1.1 and renamed **Kiban** (åŸºç›¤, "foundation") at v2.0.0 via an
> in-place repo adoption (not a fork). The PokeVault identity survives as the
> tags `v1.0.0` / `v1.1.0` / `v1.1.1` and the frozen `anchor/pokevault` branch â€”
> a single continuous history is the provenance trail. "PokeVault" therefore
> still appears in this changelog and other provenance records by design.

## [2.0.0] â€” Unreleased (tag applied at go-live, backlog KB-08)

Kiban v2.0.0 merges the two vault projects â€” the old **kiban** design project and
the **PokeVault** kit â€” into one continuously developed repository named **Kiban**,
losing nothing from either side.

### Changed
- **Rebrand in place: PokeVault â†’ Kiban.** Full rebrand sweep across README,
  INSTALL, bootstraps, vault engine files (`AGENTS.md`, `CLAUDE.md`,
  `.cursorrules`, `.vault/config.yaml`), and all skills. The `pokevault-update`
  skill is now `kiban-update`; the `pokevault_version` config key is now
  `kiban_version`. Every transform is recorded in `.planning/RENAME-MAP-2.md`.
  Provenance/history references (this changelog, genesis notes, spec/plan docs,
  git history/tags, `anchor/pokevault`) are intentionally left unrenamed.
- **New default install path:** `$env:USERPROFILE\Kiban\Vault` on Windows and
  `~/Kiban/Vault` on macOS/Linux (was `C:\PokeVault` / `~/PokeVault`).

### Added
- **Salvage import of the old kiban project** (from `kiban-archive`): the
  model-agnostic `configs/universal/` set, the cognitive-twin design spec, the
  naming guide, and the `design/` design system. (Internal architecture/decision
  records live in the private maintainer archive, not this tree.)
- **New `professional/` wiki zone** (employer-independent career, growth, goals),
  with 7 starter subdirectories â€” `goals/`, `areas/`, `people/` (+ nested
  `people/mentorship/`), `events/`, `growth/`, `portfolio/` (+ nested
  `portfolio/brand/`), and `insights/` â€” following the hybrid-zone pattern, where
  `insights/` is the machine-written synthesis landing zone (`kiban-managed`).
- **New operational zones** `archive/` and `attachments/`, completing the PARA
  arc: `archive/` is the retirement home for completed projects and superseded
  notes (agents archive, never delete); `attachments/` is the per-zone media home
  for audio/image/binary artifacts.
- **Agent-folder governance contract** (birth-certificate `_index.md` rule, two
  creation tiers, kebab-case + depth-cap shape rules) shipped as documented
  contract, config keys, and `_index.md` scaffolds. Dormant, propose-on-relevance
  folder types `opportunities/` and `ventures/` are registered but not scaffolded.
- **Post-merge backlog** at `.planning/BACKLOG.md` (spec Â§8): GAP-DEBRIEF,
  KB-01â€¦KB-09, KV-01, KG-01.

### Provenance
- Continuous history preserved via tags `v1.0.0` / `v1.1.0` / `v1.1.1` plus the
  frozen `anchor/pokevault` branch. GitHub repo rename, `v2.0.0` tag, and the
  anchor/archive pushes are deferred to go-live (KB-08).

## [1.1.1] â€” 2026-06-10

### Added
- Skill category taxonomy: skills organized into `<NN-category>` folders
  (e.g. `01-foundations/`, `02-research/`, `08-knowledge/`) with version +
  category frontmatter; bootstraps recurse category dirs and flatten to
  `.claude/skills/<name>/SKILL.md`.
- `vault/toolkit/agent-sops` tier for agent standard-operating-procedure content.

### Changed
- README tree, zone tables, skills table, AGENTS.md skill paths, and the vault
  blueprint (Â§3.5) aligned with the category-aware layout.

## [1.1.0] â€” 2026-06-10

### Changed
- Flattened the vault layout.
- Default Windows install path set to `C:\PokeVault`.
- Install docs lead with the ZIP download and de-emphasize `git clone`.

### Fixed
- Enforce LF line endings on shell scripts via `.gitattributes` (CRLF broke the
  shebang on macOS/Linux).

## [1.0.0] â€” 2026-06-09

### Added
- Initial release: PokeVault, a portable Obsidian "Second Brain + LLM Wiki" kit.
