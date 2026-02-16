---
date: <% tp.date.now("YYYY-MM") %>
type: monthly
tags:
  - monthly
---

# <% tp.date.now("MMMM YYYY") %>

[[<% tp.date.now("YYYY-MM", "P-1M") %>|← Previous Month]] | [[<% tp.date.now("YYYY-MM", "P1M") %>|Next Month →]]

---

## Summary
*Write a brief summary of this month at the end of the month.*



---

## Wins
*Items tagged #win from this month's daily notes:*

```dataview
TABLE WITHOUT ID
  file.link AS "Date",
  L.text AS "Win"
FROM "Journals/Daily"
WHERE file.name >= "<% tp.date.now("YYYY-MM") %>-01" AND file.name <= "<% tp.date.now("YYYY-MM") %>-31"
FLATTEN file.lists AS L
WHERE contains(L.text, "#win")
SORT file.name ASC
```

---

## Highlights
*Items tagged #highlight from this month's daily notes:*

```dataview
TABLE WITHOUT ID
  file.link AS "Date",
  L.text AS "Highlight"
FROM "Journals/Daily"
WHERE file.name >= "<% tp.date.now("YYYY-MM") %>-01" AND file.name <= "<% tp.date.now("YYYY-MM") %>-31"
FLATTEN file.lists AS L
WHERE contains(L.text, "#highlight")
SORT file.name ASC
```

---

## Key Decisions
*Items tagged #decision this month:*

```dataview
TABLE WITHOUT ID
  file.link AS "Date",
  L.text AS "Decision"
FROM "Journals/Daily"
WHERE file.name >= "<% tp.date.now("YYYY-MM") %>-01" AND file.name <= "<% tp.date.now("YYYY-MM") %>-31"
FLATTEN file.lists AS L
WHERE contains(L.text, "#decision")
SORT file.name ASC
```

---

## Lessons Learned
*Items tagged #learning this month:*

```dataview
TABLE WITHOUT ID
  file.link AS "Date",
  L.text AS "Learning"
FROM "Journals/Daily"
WHERE file.name >= "<% tp.date.now("YYYY-MM") %>-01" AND file.name <= "<% tp.date.now("YYYY-MM") %>-31"
FLATTEN file.lists AS L
WHERE contains(L.text, "#learning")
SORT file.name ASC
```

---

## Open Tasks
*Incomplete tasks still remaining from this month:*

```dataview
TABLE WITHOUT ID
  file.link AS "Date",
  L.text AS "Task"
FROM "Journals/Daily"
WHERE file.name >= "<% tp.date.now("YYYY-MM") %>-01" AND file.name <= "<% tp.date.now("YYYY-MM") %>-31"
FLATTEN file.lists AS L
WHERE L.task AND !L.completed
SORT file.name DESC
```

---

## Daily Notes Index
*All daily notes from this month:*

```dataview
TABLE WITHOUT ID
  file.link AS "Date"
FROM "Journals/Daily"
WHERE file.name >= "<% tp.date.now("YYYY-MM") %>-01" AND file.name <= "<% tp.date.now("YYYY-MM") %>-31"
SORT file.name ASC
```

---

## Reflections
*End of month thoughts: What went well? What could improve?*


