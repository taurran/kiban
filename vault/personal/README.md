# Personal

Your **life-management** layer — the operational "run your life" zone. Private.

Where `second-brain/` is *your mind* (who you are + what you know) and `work/` is *your job*,
`personal/` is *your life*: the people you know, the areas you maintain, the goals you're chasing,
and the dates that matter.

> This zone is **hybrid**. The CRM folders (`people/ areas/ goals/ calendar/`) are **operational
> living documents** you (and your assistant) edit directly. Alongside them, **`personal/wiki/`** is a
> standard LLM wiki that compiles **synthesis about your personal life** (relationship patterns,
> life-area knowledge, recurring themes) from its own `raw/`. The wiki **links to** the CRM records (a
> person here is the one canonical entity the wiki references — `../AGENTS.md` §6) but **never ingests
> them as raw**; `personal/wiki/` compiles only from `personal/wiki/raw/`.

## Structure

- `people/` — **Personal CRM.** One file per person (named after them: `Jordan Rivera.md`).
  Frontmatter carries `birthday`, `relationship`, `location`, `met`; the body is an interaction log.
- `areas/` — **PARA Areas.** Ongoing life domains with no end date — health, finances, hobbies,
  networking, family, learning. One file per area.
- `goals/` — **Goals & aspirations.** Active tracking with horizons and milestones (the *operational*
  layer; your `second-brain/profile/goals.md` holds the short *identity-level* version).
- `calendar/` — **Key dates & orchestration.** Birthdays (auto-surfaced from `people/`), important
  and recurring dates, reminders. `upcoming.md` is a live dashboard (Dataview).
- `_templates/` — person / area / goal / event templates.
- `wiki/` — **Personal-life knowledge wiki.** Structurally identical to `second-brain/wiki/`
  (`raw/` → `pages/`). Compiles synthesis about your personal life from `wiki/raw/`; links to the CRM
  records above but never ingests them as raw.

## How it connects (the 3-pillar wiring)

This zone carries the **CRM + Areas** pillars **and its own slice of the Wiki pillar**
(`personal/wiki/`); the single vault-root `daily/` note is the **Journal** pillar; `second-brain/wiki`
(+ `work/wiki` + `personal/wiki`) is the **Wiki** pillar. Per `../AGENTS.md`, your assistant
**cross-checks the wiki, the journal, and `people/` before answering** — so a question about a person
or commitment is grounded in everything you've recorded.

## Privacy

Private by default. Nothing here is published or shared without your explicit action. Keep sensitive
personal context (relationships, health, finances) here, not in any shared zone.

## Quick start

- "Add Jordan Rivera to my CRM — met at the conference in March, birthday June 12." → creates
  `people/Jordan Rivera.md` from the template.
- "What birthdays are coming up?" → reads `calendar/upcoming.md` / `people/` frontmatter.
- Drop life notes in the daily note; synthesis routes the personal bits here automatically.
