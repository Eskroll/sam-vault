---
date: 2026-05-07
topic: Vault Restructure + School Storage Strategy
tags:
  - claude-session
  - obsidian
  - vault
  - organization
  - school
status: complete
canonical: "false"
superseded_by: '"INDEX.md"'
---

# 2026-05-07 — Vault Restructure + School Storage Strategy

## Summary

> **Historical record.** Current state lives in [[Personal/Live Dashboard + Emails/00_Project_Control]] and [[INDEX]].
Full audit and live reorganization of the Obsidian vault. Designed and executed a clean future-state folder structure — collapsed duplicate Mersen folders into a unified `Work/`, reorganized `Claude Sessions/` by domain, added school subfolders, and deleted all old/duplicate files. Also decided on a OneNote vs. Obsidian split for school materials.

---

## Key Decisions

### Vault Restructure — Executed
Complete reorganization performed live via Obsidian MCP. Went from 42 entries / 11 dirs with duplicates → 49 entries / 18 dirs, zero duplicates.

**What was deleted:**
- All of `Mersen - ToDo/` (9 files) — content merged into `Work/`
- All of `Mersen/` (7 files) — content merged into `Work/`
- 5 flat Claude Sessions root files — moved to `Personal/` and `Work/` subfolders
- Old `Personal - Live Dashboard + Emails/` — moved into `Personal/Live Dashboard + Emails/`

**New structure:**
```
Work/
  Overview.md
  Tasks/  (00 Dashboard, 01 Inbox, 02 Active, 03 Waiting, 04 Completed)
  Projects/
    Varian Shipping Report.md   ← merged: rich reference + task frontmatter
    Run Cycle Standardization.md
    PostgreSQL Learning.md
    General Work Tasks.md
    AI CNC Research.md          ← status: completed
  Archive/

Claude Sessions/
  Personal/
    2026-05-06-claude-session-system.md
    2026-05-06-personal-dashboard.md
    2026-05-06-productivity-automation-system.md
    Live Dashboard + Emails/
      2026-05-07-obsidian-github-setup.md
  Work/
    2026-05-06-dashboard-v5-build.md
    2026-05-06-obsidian-git-dashboard-v5-plan.md
  School/   ← empty, ready for future sessions
  README.md (updated to document domain-based structure)

School/
  Archive/
  Summer 2026/
    ISE 4464/  (Syllabus, Notes/, Assignments/)
    MGT 1100/  (Syllabus, Notes/, Assignments/)

Templates/  ← unchanged
```

### Semester Lifecycle Pattern
- Active semester lives under `School/Summer 2026/`
- When semester ends → drag whole folder into `School/Archive/`
- Pre-create next semester folder empty so it's ready
- Same pattern for `Work/Archive/` — completed projects move there

### Project File Merge Strategy
- `Mersen - ToDo/Projects/` had rich frontmatter + task checklists → **kept as canonical**
- `Mersen/Projects/` had empty stubs → **deleted**
- `Mersen/AI CNC Research.md` and `Mersen/Varian Shipping Report.md` had detailed reference content → **merged into Work/Projects/ notes**
- Result: each project note now has both frontmatter (for dashboard) and full reference content

### Dashboard v5 Compatibility
- Structure works cleanly with v5 — URL paths are predictable and consistent
- **Action required:** Update Projects tab base URL from `Mersen%20-%20ToDo/Projects/` → `Work/Projects/`
- Raw URL: `https://raw.githubusercontent.com/Eskroll/sam-vault/master/Work/Projects/`

### OneNote vs. Obsidian for School Files
**Decision: Don't duplicate.**
- Professor slides, PowerPoints, annotatable PDFs → **OneNote** (annotation is the right tool)
- Personal synthesis, study guides, Claude session notes, assignment planning → **Obsidian**
- Rule: if it came from the professor and you're annotating it → OneNote. If you created or synthesized it → Obsidian.
- `Notes/` folder in each course is for self-created content only (summaries, formula sheets, study guides built with Claude)
- Planned: lightweight course index note per class with OneNote deep link + assignment tracker + personal notes

---

## Open Threads
- [ ] **Update dashboard v5 Projects tab URL** → change base path to `Work/Projects/` in Setup tab
- [ ] **Make `sam-vault` repo public** — still required for browser fetch() in dashboard Projects tab
- [ ] **Create course index note template** — OneNote link placeholder + assignment tracker + personal notes section (Claude can generate this)
- [ ] **Rename `Templates/Mersen Work Request.md`** → consider renaming to `Work Request.md` now that the folder is `Work/`
- [ ] **Renew GitHub PAT** — expires ~Aug 2026, set calendar reminder
- [ ] **Set Obsidian Git auto-sync to 5 minutes** on desktop if not already done
