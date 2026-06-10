---
type: journal-index
title: "Daily Journal — Index"
tags: [daily, index]
---

# Daily Journal — Index

The chronological pointer to your daily notes. Your assistant reads this **first** to scan the
journal for patterns ("you keep revisiting X", recurring blockers, open commitments) before pulling
individual notes. One dated line per daily note — newest at the top. The assistant refreshes the
entry for a day when it processes that day's note (`AGENTS.md` §2 Step 10).

> The daily note is the **front door** (the inbox). This index is how the journal stays scannable as
> it grows. The notes themselves stay in `daily/` as the dated record.

## Recent (assistant-maintained, newest first)

<!-- Format:  - [[YYYY-MM-DD]] — one-line summary of the day (themes, decisions, open loops) -->

_No entries yet. Your assistant adds one line per processed daily note._

## Live view (Dataview)

Recent daily notes, newest first:

```dataview
LIST
FROM "daily"
WHERE type = "daily"
SORT file.day DESC
LIMIT 30
```

Open loops across the last two weeks (unfinished tasks in daily notes):

```dataview
TASK
FROM "daily"
WHERE type = "daily" AND !completed AND file.day >= date(today) - dur(14 days)
```
