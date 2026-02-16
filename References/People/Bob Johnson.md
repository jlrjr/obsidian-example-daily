---
type: person
name: Bob Johnson
company: "[[Acme Corp]]"
department: Engineering
role: Tech Lead
relationship: partner
tags:
  - person
aliases:
  -
---

# Bob Johnson

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

## Meetings
*Meetings involving this person (full context):*

```dataviewjs
const currentPage = dv.current().file.name;
const currentAliases = dv.current().aliases || [];
const searchTerms = [currentPage, ...currentAliases];

const pages = dv.pages('"Journals/Daily"').sort(p => p.file.name, 'desc');

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
        dv.header(4, page.file.link);
        for (let meeting of meetingsFound) {
            dv.paragraph(meeting);
        }
        dv.paragraph("---");
    }
}
```

---

## All Mentions
*Other references to this person:*

```dataview
TABLE WITHOUT ID
  file.link AS "Source",
  L.text AS "Context"
FROM "Journals"
FLATTEN file.lists AS L
WHERE contains(L.text, this.file.name)
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
