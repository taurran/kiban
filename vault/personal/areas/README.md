# Areas

**PARA Areas** — ongoing life domains with no end date (vs. goals, which complete). One file per
area, created from `../_templates/area.md`.

Common areas (create the ones that fit your life): `health`, `finances`, `hobbies`, `networking`,
`family`, `learning`, `home`, `career`. Don't pre-create them all — add an area when you actually
want to maintain it.

## Active areas

```dataview
TABLE status AS "Status", review AS "Review"
FROM "personal/areas"
WHERE type = "area"
SORT status ASC, file.name ASC
```

_(Requires the Dataview plugin; otherwise browse the folder.)_
