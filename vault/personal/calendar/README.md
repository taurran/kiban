# Calendar — Key Dates & Orchestration

File-based date orchestration. Dates live in **frontmatter** (`birthday:` on people, `date:` on
events), and **Dataview** surfaces what's coming up. No external service required.

- `upcoming.md` — live dashboard of upcoming birthdays + events (open it to see what's near).
- `events.md` — log important / recurring dates, or create one note per event from
  `../_templates/event.md` (frontmatter `date`, `recurring`, `remind_before`).
- Birthdays are read from `personal/people/*` frontmatter — you don't duplicate them here.

**Reminders:** the daily note's synthesis can surface "today/this week" items into your daily and
weekly notes (Periodic Notes plugin). 

**Future (out of scope for the portable kit):** two-way sync with an external calendar
(Google/Outlook). That needs a live integration; the file-based model above works offline and
everywhere. See `docs/02-llm-wiki-synthesis-and-daily-notes.md`.
