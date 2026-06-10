# Goals

Goals & aspirations with horizons and milestones, created from `../_templates/goal.md`. This is the
**operational** layer; the short *identity-level* version of your goals lives in
`../../second-brain/profile/goals.md` (they cross-link).

## Active goals

```dataview
TABLE status AS "Status", horizon AS "Horizon", target_date AS "Target"
FROM "personal/goals"
WHERE type = "goal" AND status = "active"
SORT target_date ASC
```

_(Requires the Dataview plugin; otherwise browse the folder.)_
