---
date: 2026-05-06
topic: obsidian-git-setup-dashboard-v5-plan
tags:
  - claude-session
  - dashboard
  - obsidian-git
  - github
  - projects
status: in-progress
canonical: "false"
superseded_by: '"Personal/Live Dashboard + Emails/04_Links_And_Paths.md"'
---

# 2026-05-06 — Obsidian Git Setup + Dashboard v5 Plan

> **Historical record.** Current state lives in [[Personal/Live Dashboard + Emails/00_Project_Control]] and [[INDEX]].

## Summary
Configured Obsidian Git to sync vault to private GitHub repo (`Eskroll/sam-vault`). Confirmed Projects tab in dashboard is pulling live from GitHub. Planned full dashboard v5 redesign.

---

## Key Decisions

### Obsidian Git — Configured ✅
- Vault location: `C:\Users\sambl\OneDrive\Obsidian Vault`
- GitHub repo: `https://github.com/Eskroll/sam-vault` (private, branch: `master`)
- Token: Classic PAT, `repo` scope, 90-day expiration (~Aug 2026 — set a reminder to renew)
- Auto backup interval: 5 minutes
- Push confirmed working

### Raw GitHub URL Pattern
```
https://raw.githubusercontent.com/Eskroll/sam-vault/master/FOLDER/FILE.md
```
Spaces encoded as `%20`.

### Projects URL (updated for new vault structure)
```
https://raw.githubusercontent.com/Eskroll/sam-vault/master/Work/Projects/
```
Previously pointed at `Mersen%20-%20ToDo/Projects/` — update dashboard Setup tab after restructure.

---

## Open Threads
- [ ] **Renew GitHub PAT** — expires ~Aug 2026. Set calendar reminder.
- [ ] **Update dashboard Projects tab URL** to point at `Work/Projects/` (new path after vault restructure)
- [ ] **Make sam-vault public** — required for browser fetch() in Projects tab
- [ ] **Email digest updates** — morning snapshot + evening close-out (after v5 confirmed live)
