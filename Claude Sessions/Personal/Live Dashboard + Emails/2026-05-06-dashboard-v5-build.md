---
date: 2026-05-06
topic: dashboard-v5-build
tags:
  - claude-session
  - dashboard
  - obsidian-git
  - github
  - projects
  - v5
status: in-progress
canonical: "false"
superseded_by: '"Personal/Live Dashboard + Emails/00_Project_Control.md"'
---

# 2026-05-06 — Dashboard v5 Build

> **Historical record.** Current state lives in [[Personal/Live Dashboard + Emails/00_Project_Control]] and [[INDEX]].

## Summary
Built dashboard v5 from v4.1 base. Added frontmatter to all 4 Obsidian project notes. Redesigned Projects tab with multi-file GitHub fetch, per-project cards, status pills, Waiting On aggregation, and quick capture. Fixed calGoal reading from Sheet.

---

## Build Order

| Step | Task | Status |
|------|------|--------|
| 1 | Add frontmatter to 4 Obsidian project notes | ✅ Done |
| 2 | Build dashboard v5 (Projects redesign + home tab + calGoal fix) | ✅ Done |
| 3 | Confirm v5 live on GitHub Pages | ⬜ Pending |
| 4 | Update morning + evening email digests with project snapshot | ⬜ Pending |

---

## Key Decisions

### Frontmatter Schema (applied to all project notes)
```yaml
---
name: Project Name
status: in-progress        # in-progress | waiting | completed | paused
goal: One line goal statement
next_action: Single next action
priority: high             # high | medium | low
drive_url:
last_updated: 2026-05-06
---
```

### Dashboard v5 — Projects Tab
- Multi-file fetch: hits each `.md` file directly by filename — no index file needed
- **New vault path after restructure:** `Work/Projects/` (was `Mersen - ToDo/Projects/`)
- Raw GitHub URL base: `https://raw.githubusercontent.com/Eskroll/sam-vault/master/Work/Projects/`
- Files fetched: `Varian%20Shipping%20Report.md`, `PostgreSQL%20Learning.md`, `Run%20Cycle%20Standardization.md`, `General%20Work%20Tasks.md`
- Per-project cards: status pill, goal line, Next Action callout, priority badge, Drive link
- Waiting On panel: aggregates `## Waiting On` checklist items across all notes
- Quick Capture: copies formatted task line to clipboard for paste into Obsidian inbox

### GitHub / Vault Config
- Vault repo: `https://github.com/Eskroll/sam-vault` (branch: `master`)
- Dashboard repo: `https://github.com/Eskroll/sam-hq` → deployed as GitHub Pages
- **Vault must be public** for browser fetch() to work (no-auth raw URLs)

---

## v5.1 Planned Features
- Project card detail panel (click-to-expand: completed tasks, open tasks, notes)
- Goal Tracker improvements

---

## Open Threads
- [ ] **Make `sam-vault` repo public** — required for Projects tab fetch
- [ ] **Deploy v5 to GitHub Pages** — upload as `index.html` to `Eskroll/sam-hq`
- [ ] **Update Projects tab base URL** to `Work/Projects/` after vault restructure
- [ ] **Verify calGoal** — after next 8:30 AM trigger, confirm Sheet row 2 col C = 2190
- [ ] **Build v5.1** — click-to-expand project cards + Goal Tracker
- [ ] **Renew GitHub PAT** — expires ~Aug 2026, set calendar reminder

---

## Notes
- Vault branch is `master` (not `main`)
- Quick Capture uses clipboard copy for now — GitHub API write requires PAT server-side
