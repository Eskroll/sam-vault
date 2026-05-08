---
tags:
  - claude
  - meta
last_updated: '"2026-05-07 12:00"'
---
   
# Claude Sessions

**Vault hub:** [[INDEX]]

This folder contains saved summaries from Claude chat sessions. These are **historical records** — they are not current project state. For current project state, read the canonical files in `Personal/` or `Work/`.

---

## Folder Structure

```
Claude Sessions/
  Personal/
    Live Dashboard + Emails/    ← dashboard, automation, productivity sessions
  Work/                         ← Mersen projects, reporting, tools
  School/                       ← course-specific sessions by class
  README.md                     ← this file
```

---

## Naming Convention

```
YYYY-MM-DD HH:MM topic-keyword.md
```

Example: `2026-05-07 10:45 vault-restructure.md`

The time is 24-hour format. This disambiguates multiple sessions on the same day.

---

## When to Read Session Notes

**Do not read these by default.** Claude should read canonical project files (`00_Project_Control.md`, etc.) first.

Read session notes only when:
- Canonical files are missing context Claude needs
- Sam explicitly asks for history
- There is a conflict that needs investigation

---

## End-of-Chat Prompt

> Copy and paste this block at the end of any chat to hand off context or keep a record.

---

```
Update canonical files for this project: 00_Project_Control.md (open threads, next action, last_updated in YYYY-MM-DD HH:MM). Update 01_Current_State.md if system details changed. Update 04_Links_And_Paths.md if URLs/paths changed.

If session had significant decisions, builds, or changes: save a session note in the matching Claude Sessions/ subfolder. Filename: YYYY-MM-DD HH:MM topic-keyword.md. Frontmatter: type: session-note, project, area, date, status: archived, canonical: false, superseded_by: 00_Project_Control.md. Skip if session was minor.
```


---

## Beginning-of-Chat Prompt

> Pick the block for your task. Copy. Paste at the start of a new Claude chat.

---

**🟦 Dashboard / Email**
```
Load: INDEX.md → Personal/Live Dashboard + Emails/00_Project_Control.md → 04_Links_And_Paths.md → 01_Current_State.md. Confirm next action in one line. Don't read session notes unless a canonical file is missing context.
```

---

**🟧 Work Project**
```
Load: INDEX.md → Work/Overview.md → Work/Projects/PROJECTNAME.md. Confirm next action in one line. Don't read session notes unless a canonical file is missing context.
```
*(swap PROJECTNAME before pasting)*

---

**🟩 School**
```
Load: INDEX.md → School/Summer 2026/COURSE/Syllabus.md → relevant Notes/ or Assignments/ files. Flag upcoming due dates. Confirm task in one line. Don't read session notes unless a canonical file is missing context.
```
*(swap COURSE before pasting)*

---

**⬜ Tasks / Not Sure**
```
Load: INDEX.md → Work/Tasks/00 - Task Dashboard.md. Confirm next action in one line. Don't read session notes unless a canonical file is missing context.
```

---

**🔁 Vault / Obsidian**
```
Load: INDEX.md → Claude Sessions/README.md. Confirm next action in one line. Don't read session notes unless a canonical file is missing context.
```
