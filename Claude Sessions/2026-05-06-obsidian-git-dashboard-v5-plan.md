---
date: 2026-05-06
topic: obsidian-git-setup-dashboard-v5-plan
tags:
  - claude-session
  - dashboard
  - obsidian-git
  - github
  - projects
  - email-digest
status: in-progress
---

# 2026-05-06 — Obsidian Git Setup + Dashboard v5 Plan

## Summary
Configured Obsidian Git to sync vault to a private GitHub repo (`Eskroll/sam-vault`). Confirmed Projects tab in dashboard is pulling live from GitHub. Planned full dashboard v5 redesign including Projects tab overhaul, home tab project strip, and email digest updates.

---

## Key Decisions

### Obsidian Git — Configured ✅
- Vault location: `C:\Users\sambl\OneDrive\Obsidian Vault`
- GitHub repo: `https://github.com/Eskroll/sam-vault` (private, branch: `master`)
- Token: Classic PAT, `repo` scope, 90-day expiration (~Aug 2026 — set a reminder to renew)
- Auto backup interval: 5 minutes set in Obsidian Git plugin settings
- Push confirmed working — all vault files visible on GitHub

### Raw GitHub URL Pattern
```
https://raw.githubusercontent.com/Eskroll/sam-vault/master/FOLDER/FILE.md
```
Spaces in folder/file names are encoded as `%20`.

### Active Projects URL (currently loaded in dashboard)
```
https://raw.githubusercontent.com/Eskroll/sam-vault/master/Mersen%20-%20ToDo/02%20-%20Active%20Projects.md
```

### Dashboard Projects Tab — Currently Working
All 4 projects rendering live from GitHub in the right panel:
- Varian Shipping Report
- PostgreSQL Learning
- Run Cycle Standardization
- General Work Tasks

Left panel (file list sidebar) shows "1 FILE" — not a bug, just a UI limitation of the current single-URL approach. Will be replaced in v5.

---

## Planned Build Order

| Step | Task | Status |
|------|------|--------|
| 1 | Add frontmatter to 4 Obsidian project notes | ⬜ Next |
| 2 | Build dashboard v5 (Projects redesign + home tab + v4.2 calGoal fix) | ⬜ Pending |
| 3 | Confirm v5 live on GitHub Pages | ⬜ Pending |
| 4 | Update morning + evening email digests with project snapshot | ⬜ Pending |

---

## Dashboard v5 — Planned Features

### Projects Tab Redesign
- Individual project cards (one per project) instead of raw markdown dump
- Status pill per card: In Progress / Waiting / Completed / Paused
- Goal line + Next Action visible on each card
- Project count bar at top: X active · Y waiting · Z completed
- Waiting On section pulled from `03 - Waiting On.md`
- Optional Google Drive doc link per project card
- Quick capture box → appends to `01 - Inbox Task Dump.md`
- Multi-file fetch — pulls each project note individually, parses frontmatter

### Dashboard Home Tab
- Projects strip becomes real cards (status + next action per project)
- No longer shows empty placeholder

### Email Digests
- Morning email: Projects Snapshot section appended (active projects, status, next action)
- Evening email: Project Close-Out footer (updated today vs. no movement)

---

## Obsidian Frontmatter Structure (to add to each project note)

```yaml
---
name: Project Name
status: in-progress        # in-progress | waiting | completed | paused
goal: One line goal statement
next_action: Single next action
priority: high             # high | medium | low
drive_url:                 # optional Google Drive link
last_updated: 2026-05-06
---
```

Apply to all 4 project notes in `Mersen - ToDo/Projects/`.

---

## Open Threads

- [ ] **Renew GitHub PAT** — expires ~Aug 2026. Set a calendar reminder.
- [ ] **Push `dashboard_v4_2.html` to GitHub Pages** — still pending, replaces current index.html
- [ ] **Add frontmatter to 4 project notes** — prerequisite for dashboard v5 build
- [ ] **Build dashboard v5** — Projects tab redesign + home tab cards + calGoal fix
- [ ] **Verify calGoal** — after tomorrow's 8:30 AM writeLoseItToSheet trigger, check Sheet row 2 col C = 2190
- [ ] **Manual goals** — enter current bench/OHP/squat/streak/lawn revenue in Setup tab
- [ ] **Steps** — will auto-populate when a fitness tracker is connected to Lose It
- [ ] **Email digest updates** — morning snapshot + evening close-out (after v5 confirmed)

---

## Notes
- Vault is on OneDrive AND GitHub — OneDrive is local backup, GitHub is the sync layer for the dashboard
- Branch is `master` (not `main`) — confirmed during push
- The `git remote set-url` command showed "remote origin already exists" which was fine — remote was already set from the initial session
