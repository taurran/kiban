# LLM Wiki, Synthesis & Daily Notes

> The companion to `01-vault-blueprint.md`. The blueprint defines the *structure*; this document
> explains the *thinking* — how the LLM wiki works, the philosophy behind synthesis, and the design
> for daily/periodic notes. Some of what's here (daily-note automation, wiki-ingest automation) is
> **designed but not yet built as turnkey skills** — it's packaged now so the whole picture ships
> together and can be implemented in one pass later.

---

## Part 1 — The LLM Wiki

### 1.1 The core idea

Most "chat with your documents" setups use **RAG**: every question re-retrieves chunks from a pile
of raw documents and re-derives an answer on the spot. The pile never gets smarter; you pay the
same derivation cost forever, and cross-document insight is rebuilt from scratch each time.

The **LLM Wiki pattern** (popularized by Andrej Karpathy in 2026) inverts this. An LLM **compiles** raw
sources *once* into a structured, interlinked Markdown wiki — writing the articles, creating
backlinks, categorizing entities and concepts, flagging contradictions. From then on you query the
*compiled wiki*. When new sources arrive, the wiki is **updated**, not rebuilt: cross-references
are revised, contradictions surfaced, knowledge **compounds**.

> "Instead of re-deriving answers from raw documents on every query, let the LLM incrementally
> compile those documents into a persistent, interlinked Markdown wiki. Knowledge compounds.
> Cross-references are built once." — paraphrase of the pattern (see §Research Appendix).

Why this fits a personal/work knowledge base:
- **It compounds.** Page 50 is more valuable than page 1 because everything links.
- **It's inspectable.** Plain Markdown you can read, edit, diff, and trust.
- **It's portable.** No vector DB, no server. Obsidian renders the graph for free.
- **It degrades gracefully.** If automation disappears, you still have a clean human wiki.

### 1.2 How Kiban implements it

| Pattern element | Kiban realization |
|---|---|
| Raw drop directory | `*/wiki/raw/{inbox,meetings,notes,media}/` (immutable) |
| Processed marker (dedup-by-absence) | `*/wiki/raw/processed/` — sources move here after ingest |
| Compiled wiki | `*/wiki/pages/{sources,entities,concepts,synthesis,references}/` |
| The compiler's instructions | `vault/AGENTS.md` — the schema every LLM reads |
| Catalog / history / attention | `index.md` / `log.md` / `review.md` |
| Journal pillar (pattern detection) | `daily/index.md` (chronological one-liners) + `daily/review.md` (reclassification queue) |
| Dedup + queue state | `.vault/state/<zone>/{manifest.json,pending.json}` |
| Graph + backlinks UI | Obsidian (wikilinks, graph view, Dataview) |

The **five page buckets** are the routing target. Each holds a different *shape* of knowledge:
`sources/` (one summary per input), `entities/` (named things), `concepts/` (ideas/methods),
`synthesis/` (multi-source analysis), `references/` (tables/matrices). Routing is done by the LLM
during ingest, guided by AGENTS.md §2 Step 4 — *"when in doubt, synthesis/."*

### 1.3 Three patterns we adopt (attribution)

The ingest design borrows three **patterns** (concepts, not code) from the open-source project
**OB1 / Open Brain** by Nate B. Jones (https://github.com/NateBJones-projects/OB1). OB1 is licensed
FSL-1.1-MIT. **We implement the patterns from scratch; no OB1 code is included.** If you ship a
commercial derivative, review OB1's license terms first.

1. **Content-fingerprint dedup.** SHA-256 of normalized source content gates ingest, so the same
   article dropped twice never creates duplicate pages. (`manifest.json`, AGENTS.md §2 Step 2.)
   After ingest the source is **moved to `raw/processed/`**, so absence from the live buckets is a
   second, filesystem-level dedup signal — the periodic ingest never rescans `processed/`.
2. **Auto-capture.** At the end of a meaningful session (chat, voice, meeting), capture decisions,
   action items, open questions, and context into a structured summary file that enters the normal
   ingest flow. (Daily-note "Capture → wiki" section is the manual seed of this.)
3. **Schema-aware routing.** The LLM decides each fragment's destination zone using the rules in
   AGENTS.md, rather than brittle pattern-matching or manual filing.

All three generalize cleanly to a file-based, Obsidian-native wiki — no external infrastructure.

### 1.4 The four ingest paths

| Path | How it works | Status |
|---|---|---|
| **Manual drop** | Put a file in `raw/inbox/`; ask the assistant to "process the inbox." | Works today (assistant-driven). |
| **Voice** | Save a voice transcript to `raw/notes/` (frontmatter `source: voice`); ingest. | Works today (manual save). |
| **Chat / session capture** | At session end, write a structured summary to `raw/notes/`; ingest. | Works today (manual); auto-capture skill = future. |
| **Web clippings** | Obsidian Web Clipper → `second-brain/wiki/raw/inbox/`; ingest. | Works today (configure clipper once). |

Future automation (documented, not built): transcript polling, session-end hooks, email/Slack
auto-ingest, and semantic (vector) search to replace grep. See §4.

### 1.5 Query workflow

1. Read the zone's `index.md` (or search) to find candidate pages.
2. Read the candidates.
3. **Verify** specific facts against the raw source listed in `sources:` — the wiki is a map.
4. Answer with citations: `(from [[page-name]])`.
5. **Grow the wiki from the question:** synthesize a non-trivial answer into a new/updated page,
   append to `log.md`, and update `index.md` (query-driven growth — AGENTS.md §6). Skip for trivial
   lookups.
6. If nothing matches, search `raw/` and offer to ingest.

### 1.6 Workshop → library (research + promotion)

Not all knowledge starts as a dropped source. Sometimes you investigate a topic over days — reading,
note-taking, testing hypotheses — before anything is ready for the wiki. That's what the
**workshop** layer is for.

**The model:** `research/` (visible, one folder per investigation) and `projects/` (Obsidian-ignored
code workspaces) are the workshop — readable and agent-accessible but **not part of the compiled
wiki**. The wiki (`*/wiki/pages/`) is the library. A one-way **promotion** gate moves finished
workshop artifacts into the library; nothing travels the other direction.

**There is no separate "research wiki."** Promoted findings land in the same zone wikis described in
§1.2 — routed to `entities/`, `concepts/`, `synthesis/`, or `references/` using the normal ingest
rules.

**Promotion eligibility (frontmatter-driven):**

- The file carries `status: ready` **or** a `vault:promote` tag in its `tags:` list.
- The `sources:` field is **non-empty** (provenance is mandatory — the wiki never accepts
  unsourced claims).

**Two triggers:**

1. **On request** — you ask an external harness or your assistant to promote a specific finding.
2. **Proactive offer** — when a research loop completes (all questions answered, `findings.md`
   written), the harness offers to promote eligible pages into the wiki.

`projects/` items follow the same path: tag a file `vault:promote`, ensure it has `sources:`, and
it becomes promotable on the next pass.

**The frontmatter contract** governing research project files, promotion fields, and status
transitions is defined in `AGENTS.md` §5 (schemas) and §8 (cross-zone flow). Refer there for the
full field-level spec; the invariant from the user's perspective is simple: mark it ready, ensure
it has sources, and it graduates.

---

## Part 2 — Synthesis philosophy

Synthesis is where a pile of notes becomes *knowledge*. The hard part isn't writing pages — it's
writing them so they stay trustworthy as they grow. Six principles:

### 2.1 Append, don't rewrite
New information becomes a dated `### Update [YYYY-MM-DD]` section. Existing text is preserved. This
is the single most important anti-forgetting rule: it means the wiki can never silently lose what
it knew yesterday. The cost is that long-lived pages accumulate update logs — which is what
*consolidation* (2.5) addresses, carefully.

### 2.2 Map, not territory
Pages are synthesis; raw is truth. Any exact quote, number, date, or commitment must be verified
against (and cite) the raw source. Synthesis is for *understanding*; raw is for *evidence*.

### 2.3 Confidence is a function of corroboration
`confidence: low` (1 source), `medium` (2), `high` (3+). A claim isn't "true" because it's written
down — it's as strong as the number of independent sources behind it. Lint nudges you to corroborate
aging low-confidence pages.

### 2.4 Contradictions are surfaced, never silently resolved
When a new source disagrees with a page, the assistant adds a `⚠️ Contradiction` block with both
claims and their sources, and routes a note to `review.md`. **You** decide. Auto-resolution is how
knowledge bases quietly become wrong.

### 2.5 Consolidation — the one careful exception
When a page reaches 5+ update sections it's an append-log, not synthesis. Consolidation rewrites it
as one coherent page — but only with: (a) the prior version archived, (b) your approval, (c) all
sources preserved, and (d) a **mandatory diff log** listing what was preserved vs. dropped. The diff
log is the safety rail; without it, consolidation *is* catastrophic forgetting wearing a helpful mask.

### 2.6 Provenance and canonicality
Every page records its `sources:`. Every entity has exactly one canonical page; other zones link to
it rather than copying. This keeps a single source of truth per thing and makes the graph honest.

### Why this compounds rather than rots
RAG piles don't rot because they don't change; they also don't improve. Naive "let the AI keep
editing my notes" rots fast because edits overwrite and errors propagate. The append + provenance +
contradiction-flag + bounded-consolidation discipline is the middle path: the wiki *changes* (so it
improves) but never *loses* (so it stays trustworthy).

---

## Part 3 — Daily & Periodic Notes (design — wiring shipped, automation future)

A **single daily note** is the low-friction capture surface for *both* work and personal. The vault
ships it **wired** (Obsidian core Daily Notes is enabled and points at the neutral vault-root
`daily/` with a seeded template). The user captures freely and never pre-sorts; **synthesis routes
each fragment** to the right zone. The automated flow is a documented design you currently drive by
hand (the assistant does the routing on request).

### 3.1 The model

```
ONE daily note  (daily/YYYY-MM-DD.md)   ← work + personal, captured freely
   │  on ingest, synthesis reads it and SPLITS by content:
   ├── work items ...........→ work/wiki/
   ├── people / networking ..→ personal/people/   (CRM)
   ├── areas / goals / dates → personal/areas | goals | calendar/
   ├── personal-life synth ..→ personal/wiki/
   ├── durable knowledge ....→ second-brain/wiki/
   └── reflections ..........→ second-brain/profile/patterns.md
   ▼
daily note stays in daily/ as the dated record (never deleted)
```

This is the **3-pillar wiring** (Wiki + CRM + Journal): the daily note is the **Journal**,
`personal/people/` is the **CRM**, and `*/wiki/` is the **Wiki**. The journal keeps its own pointer —
**`daily/index.md`** (one dated line per note, for pattern detection). Ambiguous fragments are
**default-routed to the best-guess zone and logged in `daily/review.md`** for one-tap reclassification
— never stranded, never silently misrouted. The daily note is a *source*, not a wiki page — synthesis
routes from it; the note itself is the chronological record.

### 3.2 The shipped daily template

`daily/_templates/daily.md` (applied automatically by core Daily Notes) has five sections:
**Focus** (top 1–3), **Tasks**, **Log** (timestamped), **Capture** (ingest candidates, with optional
non-forced `#work` / `#personal` cues), and **Reflection** (feeds `patterns.md`). Frontmatter:
`type: daily`, `date`, `tags: [daily]`.

### 3.3 Periodic notes (weekly/monthly/quarterly)

Layer the community **Periodic Notes** plugin on top of core Daily Notes. Recommended structure
(public best practice — see appendix):

| Cadence | Filename format | Folder | Purpose |
|---|---|---|---|
| Daily | `YYYY-MM-DD` | `daily/` | Capture + log (work + personal) |
| Weekly | `gggg-[W]ww` (e.g., `2026-W24`) | `daily/weekly/` | Roll up the week; pull open tasks; pick next-week focus |
| Monthly | `YYYY-MM` | `daily/monthly/` | Themes, wins, what to change; trigger cognitive extraction |
| Quarterly | `YYYY-[Q]Q` | `daily/quarterly/` | Goal review vs `profile/goals.md` + `personal/goals/` |

Weekly/monthly notes are **review surfaces**, not capture surfaces. With Dataview you can
auto-aggregate — e.g., list every daily note's unfinished `- [ ]` tasks into the weekly note, or
surface pages created this week:

```dataview
TASK FROM "daily" WHERE !completed AND file.day >= date(today) - dur(7 days)
```

```dataview
LIST FROM "work/wiki/pages" WHERE file.cday >= date(today) - dur(7 days) SORT file.cday DESC
```

### 3.4 Daily → wiki automation (future skill)

The `daily-note` skill (in `skills/`) currently documents the manual flow. The future automated
version: a scheduled assistant reads the day's "Capture → wiki" section, writes keepers to
`raw/notes/` as structured sources (auto-capture pattern), and runs ingest — leaving the daily note
intact as the dated record. This is intentionally deferred until the ingest skills are hardened, so
automation never runs ahead of the trust rails.

---

## Part 4 — Future directions (documented, not built)

| Area | Idea | Why deferred |
|---|---|---|
| **Semantic search** | Replace grep with local hybrid BM25+vector (e.g., a local index) and LLM re-rank. | Adds a dependency; grep + Obsidian search suffice at small scale. |
| **Auto-capture** | Session-end hooks (chat/voice/coding) write summaries automatically. | Needs runtime hooks the portable kit can't assume. |
| **Transcript polling** | Watch a voice-app folder and auto-queue transcripts. | Background daemon = install complexity; manual save works now. |
| **Shared zone** | Add a `team/` zone synced to a shared drive, with a publish guardrail. | Out of scope for a single-user portable kit; the guardrail is pre-described in AGENTS.md §8 so it slots in cleanly. |
| **Eval harness** | Measure routing/synthesis quality automatically. | Ship prompt-engineered defaults first; tune from real use. |

When any of these is built, it follows the **non-destructive update contract** (see the project's
update mechanism): new capability is added *around* your data without touching existing pages, raw,
profile, config, or state.

---

## Research Appendix — public grounding

This design deliberately aligns with public, professional best practice so it stays recognizable
and maintainable by anyone, not tied to any one organization's internal conventions.

**LLM Wiki pattern (the core idea)**
- Andrej Karpathy's original gist describing the pattern (raw sources → incrementally compiled,
  interlinked Markdown wiki; query the wiki, not the pile).
- Independent write-ups confirming the pattern: "Beyond RAG: how the LLM Wiki builds knowledge that
  compounds"; "Building an LLM Wiki with Claude Code and Obsidian"; community reference
  implementations. Consensus points we adopted: compile-once, update-don't-rebuild, flag
  contradictions, interlink, keep it Markdown.

**Knowledge-base patterns (ingest mechanics)**
- OB1 / Open Brain (Nate B. Jones) — fingerprint dedup, auto-capture, schema-aware routing. Patterns
  borrowed conceptually; license FSL-1.1-MIT; no code vendored.

**Obsidian vault organization**
- PARA (Tiago Forte: Projects / Areas / Resources / Archive) and Zettelkasten influence the
  separation of *actionable* (work `initiatives/`, `deliverables/`) from *reference* (the wiki) and
  *archive* (`_archive/`). Kiban uses zones + a compiled wiki rather than strict PARA, but the
  actionability ladder is the same instinct.
- Community guidance on folders-vs-tags-vs-links: prefer links + properties for relationships, use
  folders for coarse separation, avoid deep nesting. Reflected in the shallow zone/wiki layout and
  the reliance on wikilinks + frontmatter for structure.

**Daily / periodic notes**
- Obsidian core **Daily Notes** (built-in) + community **Periodic Notes** (Liam Cain) for
  weekly/monthly/quarterly. Filename formats (`YYYY-MM-DD`, `gggg-[W]ww`, `YYYY-MM`) follow the
  plugin defaults and community templates so the vault interoperates with existing setups.
- Community journaling workflows informed the daily template's capture-then-review rhythm and the
  weekly/monthly roll-up via Dataview.

> All external references are public, professional sources. None of this design depends on any
> proprietary or organization-internal system.
