# People — Personal CRM

One file per person (named after them, e.g. `Jordan Rivera.md`), created from
`../_templates/person.md`. The body is an append-only interaction log; key facts live in frontmatter.

> Rule (from `../../AGENTS.md` §6): **one canonical home per person.** The wiki links to a person
> here as an entity; it never duplicates them.

## Directory

```dataview
TABLE relationship AS "Relationship", birthday AS "Birthday", location AS "Location"
FROM "personal/people"
WHERE type = "person"
SORT file.name ASC
```

_(Requires the Dataview community plugin. Without it, this is just a normal note — browse the folder.)_

## Add someone

Tell your assistant: *"Add &lt;name&gt; to my CRM — &lt;context, where you met, birthday…&gt;."*
It creates the file from the template, fills frontmatter, and logs the first interaction.
