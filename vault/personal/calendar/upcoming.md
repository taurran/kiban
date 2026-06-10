---
type: dashboard
tags:
  - calendar
---

# Upcoming

> Live dashboard (requires the **Dataview** plugin). Tune the windows to taste.

## 🎂 Birthdays (from your CRM)

```dataview
TABLE birthday AS "Birthday", relationship AS "Relationship"
FROM "personal/people"
WHERE birthday
SORT birthday ASC
```

## 📅 Upcoming events

```dataview
TABLE date AS "Date", recurring AS "Recurring"
FROM "personal/calendar"
WHERE type = "event" AND date >= date(today)
SORT date ASC
```

## ⏰ Recurring (any year)

```dataview
TABLE date AS "Date", remind_before AS "Notice (days)"
FROM "personal/calendar"
WHERE type = "event" AND recurring != "none"
SORT date ASC
```

_Tip: a "next 30 days" birthday view needs month/day math; the reminder/Periodic-Notes plugins
handle that well. The lists above are the dependency-free baseline._
