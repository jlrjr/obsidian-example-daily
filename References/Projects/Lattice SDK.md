---
type: project
name: Lattice SDK
status: active
summary: Developer SDK for the Lattice platform enabling partner integrations and custom applications.
companies:
  - "[[Anduril]]"
  - "[[Acme Corp]]"
people:
  - "[[plugin examples]]"
tags:
  - project
aliases:
  - SDK
  - Lattice
---

# Lattice SDK

## Overview
| Field | Value |
|-------|-------|
| Status | `= this.status` |
| Companies | `= this.companies` |
| People | `= this.people` |

### Summary
`= this.summary`

---

## Notes
*Project notes, decisions, and updates.*

- Q1 goal: Beta release with core API endpoints
- Focus on OAuth2 authentication for partners

---

## Recent Meetings (Last 30 Days)

```dataviewjs
const currentPage = dv.current().file.name;
const currentAliases = dv.current().aliases || [];
const searchTerms = [currentPage, ...currentAliases];

// Calculate 30 days ago
const cutoffDate = new Date();
cutoffDate.setDate(cutoffDate.getDate() - 30);
const cutoffStr = cutoffDate.toISOString().split('T')[0];

const pages = dv.pages('"Journals/Daily"')
    .where(p => p.file.name >= cutoffStr)
    .sort(p => p.file.name, 'desc');

let found = false;

for (let page of pages) {
    const content = await dv.io.load(page.file.path);
    const lines = content.split('\n');
    
    let inMeetingsSection = false;
    let currentMeeting = [];
    let currentMeetingHasLink = false;
    let meetingsFound = [];
    
    for (let i = 0; i < lines.length; i++) {
        const line = lines[i];
        
        if (line.match(/^## Meetings/i)) {
            inMeetingsSection = true;
            continue;
        }
        
        if (inMeetingsSection && line.match(/^## /) && !line.match(/^## Meetings/i)) {
            if (currentMeeting.length > 0 && currentMeetingHasLink) {
                meetingsFound.push(currentMeeting.join('\n'));
            }
            inMeetingsSection = false;
            continue;
        }
        
        if (inMeetingsSection) {
            if (line.match(/^### /)) {
                if (currentMeeting.length > 0 && currentMeetingHasLink) {
                    meetingsFound.push(currentMeeting.join('\n'));
                }
                currentMeeting = [line];
                currentMeetingHasLink = searchTerms.some(term => 
                    line.includes(`[[${term}]]`) || line.includes(`[[${term}|`)
                );
            } else if (currentMeeting.length > 0) {
                currentMeeting.push(line);
                if (!currentMeetingHasLink) {
                    currentMeetingHasLink = searchTerms.some(term => 
                        line.includes(`[[${term}]]`) || line.includes(`[[${term}|`)
                    );
                }
            }
        }
    }
    
    if (currentMeeting.length > 0 && currentMeetingHasLink) {
        meetingsFound.push(currentMeeting.join('\n'));
    }
    
    if (meetingsFound.length > 0) {
        found = true;
        dv.header(4, page.file.link);
        for (let meeting of meetingsFound) {
            dv.paragraph(meeting);
        }
        dv.paragraph("---");
    }
}

if (!found) {
    dv.paragraph("*No meetings in the last 30 days.*");
}
```

---

## All Meetings

```dataview
TABLE WITHOUT ID
  file.link AS "Date",
  L.text AS "Meeting"
FROM "Journals/Daily"
FLATTEN file.lists AS L
WHERE contains(L.text, this.file.name) OR any(this.aliases, (a) => contains(L.text, a))
WHERE meta(L.section).subpath = "Meetings"
SORT file.name DESC
```

---

## Tasks

```dataview
TABLE WITHOUT ID
  file.link AS "Source",
  L.text AS "Task"
FROM "Journals/Daily"
FLATTEN file.lists AS L
WHERE contains(L.text, this.file.name)
WHERE L.task AND !L.completed
SORT file.name DESC
```

---

## All Mentions

```dataview
TABLE WITHOUT ID
  file.link AS "Source",
  L.text AS "Context"
FROM "Journals"
FLATTEN file.lists AS L
WHERE contains(L.text, this.file.name)
WHERE !contains(meta(L.section).subpath, "Meetings")
SORT file.name DESC
LIMIT 20
```

---

## Related People

```dataview
LIST
FROM "References/People"
WHERE contains(file.outlinks, this.file.link)
```

---

## Timeline
*Key dates and milestones.*

- 2026-01-05: Kickoff meeting
- 2026-01-06: Beta shipped internally
