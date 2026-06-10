---
type: research-index
title: "Research — Index"
tags:
  - research
  - index
---

# Research — Index

Live dashboard for active research projects and findings ready for promotion into the wiki.

## Active research projects

```dataview
TABLE status, file.mtime AS updated FROM "research" WHERE type = "research-project" SORT file.mtime DESC
```

## Ready to promote

```dataview
TABLE project, status FROM "research" WHERE type = "research-finding" AND (status = "ready" OR contains(tags, "vault:promote")) AND status != "promoted"
```
