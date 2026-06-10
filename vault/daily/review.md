---
type: journal-review
title: "Daily Journal — Reclassification Queue"
tags: [daily, review]
---

# Daily Journal — Reclassification Queue

When a captured fragment is genuinely ambiguous, your assistant **routes it to the single best-guess
zone anyway** (nothing is ever stranded or lost) and logs the decision here so you can move it in one
step if it landed wrong. This is the *route-by-default + never-silently-misroute* contract from
`AGENTS.md` §8.

> This is the **journal's** triage list (where did today's captures go?). It is distinct from each
> wiki zone's `review.md`, which handles wiki-internal issues (contradictions, near-duplicates,
> consolidation).

## How to use it
- Each line is a defaulted routing decision. Check it off once you've confirmed or corrected it.
- To reclassify: move the page/record to the right zone (Obsidian updates the links), then check the box.

## Queue

<!-- Format:  - [ ] YYYY-MM-DD: routed "<fragment>" → <zone> (low confidence) — reclassify? -->

_No items yet._

## Live view (unresolved reclassifications)

```dataview
TASK
FROM "daily"
WHERE !completed AND contains(text, "reclassify")
```
