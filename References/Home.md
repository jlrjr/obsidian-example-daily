# Tips
*can add some reminder tips here*


# Dashboard

## Unresolved Links
*Links to pages that don't exist yet. Review periodically and create as needed.*

```dataview
TABLE WITHOUT ID
  key AS "Missing Page",
  rows.file.link AS "Referenced In"
FROM "Journals"
FLATTEN file.outlinks AS outlink
WHERE !outlink.file
GROUP BY outlink AS key
SORT length(rows) DESC
```

---

## Recently Modified
*Your most recently updated notes.*

```dataview
TABLE WITHOUT ID
  file.link AS "Note",
  file.folder AS "Folder",
  dateformat(file.mtime, "yyyy-MM-dd HH:mm") AS "Modified"
FROM ""
WHERE file.name != "dashboard"
SORT file.mtime DESC
LIMIT 10
```

---

## Open Tasks (All)
*All incomplete tasks across daily notes.*

```dataview
TABLE WITHOUT ID
  file.link AS "Date",
  L.text AS "Task"
FROM "Journals/Daily"
FLATTEN file.lists AS L
WHERE L.task AND !L.completed
SORT file.name DESC
LIMIT 20
```

---

## High Priority Tasks
*Tasks tagged #high.*

```dataview
TABLE WITHOUT ID
  file.link AS "Date",
  L.text AS "Task"
FROM "Journals/Daily"
FLATTEN file.lists AS L
WHERE L.task AND !L.completed AND contains(L.text, "#high")
SORT file.name DESC
```

---

## Waiting On
*Tasks tagged #waiting.*

```dataview
TABLE WITHOUT ID
  file.link AS "Date",
  L.text AS "Task"
FROM "Journals/Daily"
FLATTEN file.lists AS L
WHERE L.task AND !L.completed AND contains(L.text, "#waiting")
SORT file.name DESC
```

---

## Recent Wins
*Last 10 items tagged #win.*

```dataview
TABLE WITHOUT ID
  file.link AS "Date",
  L.text AS "Win"
FROM "Journals/Daily"
FLATTEN file.lists AS L
WHERE contains(L.text, "#win")
SORT file.name DESC
LIMIT 10
```

---

## Active Projects

```dataview
TABLE WITHOUT ID
  file.link AS "Project",
  status AS "Status",
  companies AS "Companies"
FROM "References/Projects"
WHERE status = "active"
SORT file.name ASC
```
