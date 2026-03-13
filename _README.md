	# Obsidian Vault Setup

## Quick Start

### 1. Install Community Plugins

Enable Community Plugins in Settings → Community Plugins → Turn on community plugins

**Core Plugins (Required)**

|Plugin|Purpose|
|---|---|
|**Periodic Notes**|Daily/monthly note creation|
|**Dataview**|Dynamic queries and backlink displays|
|**Templater**|Template engine for all pages|
|**Rollover Daily Todos**|Carries incomplete tasks forward|

**Recommended Plugins**

|Plugin|Purpose|
|---|---|
|**Calendar**|Visual calendar in sidebar. Click any date to open/create that daily note.|
|**QuickAdd**|Create people/projects/companies with one hotkey from anywhere.|
|**Various Complements**|Auto-complete for `[[links]]` as you type. Speeds up linking.|
|**Homepage**|Set dashboard as startup page. Opens to tasks and unresolved links.|

**Optional Plugins**

|Plugin|Purpose|
|---|---|
|**Natural Language Dates**|Type `@tomorrow` or `@next friday` for due dates.|
|**Linter**|Auto-formats markdown on save. Set once, forget it.|
https://github.com/mgmeyers/obsidian-list-callouts
https://github.com/scambier/obsidian-omnisearch

### 2. Plugin Configuration

#### Templater Settings

- Template folder location: `templates`
- Enable "Trigger Templater on new file creation"
- Enable "Enable folder templates" and set:
    - `Journals/Daily` → `templates/daily.md`
    - `Journals/Monthly` → `templates/monthly.md`
    - `References/People` → `templates/person.md`
    - `References/Projects` → `templates/project.md`
    - `References/Companies` → `templates/company.md`

#### Periodic Notes Settings

- Enable Daily Notes:
    - Format: `YYYY-MM-DD`
    - Daily Note Template: `templates/daily.md`
    - New File Location: `Journals/Daily`
- Enable Monthly Notes:
    - Format: `YYYY-MM`
    - Monthly Note Template: `templates/monthly.md`
    - New File Location: `Journals/Monthly`

#### Dataview Settings

- Enable JavaScript Queries (required for meeting block queries)
- Enable Inline Queries

#### Rollover Daily Todos Settings

- Enable automatic rollover
- Delete empty task lines after rollover: Yes
- Roll over children of incomplete tasks: Yes

#### Calendar Settings

- Show week numbers: Optional
- Start week on: Sunday
- Words per dot: 250 (shows activity dots on calendar)

#### Homepage Settings

- Homepage: `_references/dashboard`
- Opening method: Replace current tab
- Open on startup: Yes

#### QuickAdd Settings

See "QuickAdd Setup" section below for full configuration.

### 3. Keyboard Shortcuts to Remember

|Action|Suggested Shortcut|How to Set|
|---|---|---|
|Open today's daily note|`Cmd/Ctrl + D`|Settings → Hotkeys → "Periodic Notes: Open daily note"|
|Open monthly note|`Cmd/Ctrl + M`|Settings → Hotkeys → "Periodic Notes: Open monthly note"|
|Open dashboard|`Cmd/Ctrl + 0`|Settings → Hotkeys → "Homepage: Open homepage"|
|New Person|`Cmd/Ctrl + Shift + 1`|Settings → Hotkeys → "QuickAdd: New Person"|
|New Project|`Cmd/Ctrl + Shift + 2`|Settings → Hotkeys → "QuickAdd: New Project"|
|New Company|`Cmd/Ctrl + Shift + 3`|Settings → Hotkeys → "QuickAdd: New Company"|

---

## QuickAdd Setup

QuickAdd lets you create new people, projects, and companies from anywhere with a single keystroke. No need to navigate to folders or manually apply templates.

### Creating the Macros

Open Settings → QuickAdd

#### New Person Macro

1. Click "Add Choice" → name it `New Person` → select "Template"
2. Click the gear icon to configure:
    - Template Path: `templates/person.md`
    - File Name Format: `{{VALUE}}`
    - Create in folder: `References/People`
    - Open: Yes
3. Click the lightning bolt icon to add a hotkey (`Cmd+Shift+1`)

#### New Project Macro

1. Click "Add Choice" → name it `New Project` → select "Template"
2. Click the gear icon to configure:
    - Template Path: `templates/project.md`
    - File Name Format: `{{VALUE}}`
    - Create in folder: `References/Projects`
    - Open: Yes
3. Click the lightning bolt icon to add a hotkey (`Cmd+Shift+2`)

#### New Company Macro

1. Click "Add Choice" → name it `New Company` → select "Template"
2. Click the gear icon to configure:
    - Template Path: `templates/company.md`
    - File Name Format: `{{VALUE}}`
    - Create in folder: `References/Companies`
    - Open: Yes
3. Click the lightning bolt icon to add a hotkey (`Cmd+Shift+3`)

### Using QuickAdd

1. Press `Cmd+Shift+1` (New Person)
2. Type the person's name in the prompt
3. Press Enter
4. File is created with template applied, cursor in the new file
5. Fill in frontmatter, then navigate back to your daily note

This works from anywhere—you never need to leave your daily note to create a new person, project, or company.

---

## Dashboard

The dashboard (`_references/dashboard.md`) is your home base. Set it as your startup page with the Homepage plugin.

**What It Shows**

- **Unresolved Links** - pages you've referenced but haven't created yet
- **Recently Modified** - your latest activity
- **Open Tasks** - all incomplete tasks across daily notes
- **High Priority Tasks** - tasks tagged `#high`
- **Waiting On** - tasks tagged `#waiting`
- **Recent Wins** - your last 10 `#win` items
- **Active Projects** - projects with status: active

**Using It**

- Open with `Cmd+0` (if you set the hotkey)
- Check unresolved links periodically and create pages with QuickAdd
- Review open tasks at start of day
- Celebrate your wins

---

## Daily Workflow

### Morning (2 minutes)

1. `Cmd+D` to open today's note (auto-creates from template)
2. Review rolled-over tasks from yesterday
3. Add any known priorities for the day

### During the Day

- Quick capture thoughts at the top
- Log meetings under `## Meetings` with attendees linked
- Add tasks as they come up with priority tags (`#high`, `#low`)
- Tag wins with `#win` and highlights with `#highlight`

### End of Day (2 minutes)

- Move any quick capture items to proper sections
- Add brief reflection
- Incomplete tasks auto-roll to tomorrow

### Monthly (10 minutes, first of month)

1. `Cmd+M` to open monthly note
2. Review auto-populated highlights and wins
3. Add any reflections or lessons learned

---

## Tags Reference

### Priority Tags (for tasks)

- `#high` - must do today/this week
- `#low` - nice to have
- `#waiting` - blocked on someone else

### Content Tags

- `#win` - victories, successes (shows in monthly rollup)
- `#highlight` - important items to surface in monthly review
- `#feedback` - feedback received or given
- `#learning` - lessons learned, insights
- `#integration` - integration-related notes
- `#decision` - key decisions made

### Usage Examples

```markdown
- [ ] Review SDK documentation #high
- [ ] Follow up with vendor #waiting
- Had a great demo with the team #win
- Realized we need better error handling #learning #highlight
```

---

## Creating New Pages

### Quick Method (Recommended)

Use QuickAdd hotkeys from anywhere:

- `Cmd+Shift+1` → New Person
- `Cmd+Shift+2` → New Project
- `Cmd+Shift+3` → New Company

Type the name, press Enter, done. Template auto-applies.

### Manual Method

1. Navigate to the appropriate folder (`References/People`, `Projects`, or `Companies`)
2. Create new note with the name
3. Template auto-applies via Templater folder templates
4. Fill in frontmatter

### Placeholder Links

When writing in your daily note, you can type `[[New Person Name]]` as a placeholder. Later:

1. Check the Dashboard for unresolved links
2. Use QuickAdd to create the page with the same name
3. The link auto-resolves

---

## Linking Best Practices

### In Daily Notes

```markdown
## Meetings
### Lattice SDK Review [[Jane Smith]] [[Lattice SDK]]
Attendees: [[Jane Smith]], [[Bob Johnson]], [[Sarah Chen]]

Discussed timeline for Q1 release. #decision
```

### In Notes Section

```markdown
## Notes
- Call with [[Acme Corp]] about partnership #highlight
- [[Jane Smith]] mentioned concerns about timeline
```

The dataview queries on People, Project, and Company pages will automatically find and display these backlinks with context.

---

## File Structure

```
/
├── _references/
│   ├── dashboard.md            # Task lists, unresolved links, overview
│   └── daily-links.md          # Links to display on every daily note
├── templates/
│   ├── daily.md
│   ├── monthly.md
│   ├── person.md
│   ├── project.md
│   └── company.md
├── Journals/
│   ├── Daily/                  # Daily notes: 2026-01-06.md
│   └── Monthly/                # Monthly notes: 2026-01.md
├── References/
│   ├── People/                 # Person pages
│   ├── Projects/               # Project pages
│   └── Companies/              # Company pages
└── README.md
```

---
