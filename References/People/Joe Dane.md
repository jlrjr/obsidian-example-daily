---
type: person
name: Joe Dane
company: 
department: 
role: 
relationship: employee
tags:
  - person
aliases:
  - 
---

# Joe Dane

## Info
| Field | Value |
|-------|-------|
| Company | `= this.company` |
| Department | `= this.department` |
| Role | `= this.role` |
| Relationship | `= this.relationship` |

---

## Notes
*General notes about this person.*



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

```dataviewjs
const currentPage = dv.current().file.name;
const currentAliases = dv.current().aliases || [];
const searchTerms = [currentPage, ...currentAliases];

const rows = [];
const pages = dv.pages('"Journals/Daily"').sort(p => p.file.name, 'desc');

for (let page of pages) {
    const content = await dv.io.load(page.file.path);
    const lines = content.split('\n');
    
    let inMeetingsSection = false;
    
    for (let line of lines) {
        if (line.match(/^## Meetings/i)) {
            inMeetingsSection = true;
            continue;
        }
        if (inMeetingsSection && line.match(/^## /) && !line.match(/^## Meetings/i)) {
            inMeetingsSection = false;
            continue;
        }
        if (inMeetingsSection && line.match(/^### /)) {
            const hasLink = searchTerms.some(term => 
                line.includes(`[[${term}]]`) || line.includes(`[[${term}|`)
            );
            if (hasLink) {
                rows.push([page.file.link, line.replace(/^### /, '')]);
            }
        }
    }
}

if (rows.length > 0) {
    dv.table(["Date", "Meeting"], rows);
} else {
    dv.paragraph("*No meetings found.*");
}
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
LIMIT 15
```

---

## Related Projects

```dataview
LIST
FROM "References/Projects"
WHERE contains(people, this.file.link) OR contains(people, this.file.name)
```
