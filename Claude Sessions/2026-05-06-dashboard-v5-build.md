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
---

# 2026-05-06 — Dashboard v5 Build

## Summary
Built dashboard v5 from v4.1 base. Added frontmatter to all 4 Obsidian project notes. Redesigned Projects tab with multi-file GitHub fetch, per-project cards, status pills, Waiting On aggregation, and quick capture. Fixed calGoal reading from Sheet. Home tab now shows real project mini-cards. Vault repo needs to be made public for fetch to work.

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

### Step 1 — Frontmatter Added to All 4 Project Notes
Applied to every file in `Mersen - ToDo/Projects/`:

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

Applied statuses:
- **Varian Shipping Report** → `in-progress` · priority: high
- **PostgreSQL Learning** → `waiting` · priority: medium
- **Run Cycle Standardization** → `in-progress` · priority: high
- **General Work Tasks** → `in-progress` · priority: low

### Step 2 — Dashboard v5 Built (dashboard_v5.html)

**Projects tab — full redesign:**
- Multi-file fetch: hits each `.md` file directly by filename, no index file needed
- Files fetched: `Varian%20Shipping%20Report.md`, `PostgreSQL%20Learning.md`, `Run%20Cycle%20Standardization.md`, `General%20Work%20Tasks.md`
- Frontmatter parsed client-side from each note
- Per-project cards: status pill with colored left-border, goal line, Next Action callout, priority badge, Drive link, last updated
- Count bar: X active · Y waiting · Z completed · Z paused
- Waiting On panel: aggregates all `## Waiting On` checklist items across all 4 notes
- Quick Capture: copies formatted task line to clipboard for paste into Obsidian inbox

**Home tab (Dashboard):**
- Projects strip now renders real mini-cards from frontmatter
- Shows status pill + next action per project, color-coded left border
- Replaces old empty placeholder

**calGoal fix:**
- Now correctly reads `calGoal` from Sheet column C (index 2) instead of hardcoding 2350

### Step 3 — GitHub Pages Deploy (PENDING)
- Upload `dashboard_v5.html` to `github.com/Eskroll/sam-hq` as `index.html`
- Easiest path: edit `index.html` in GitHub web UI → select all → paste v5 content → commit
- URL stays the same: `https://Eskroll.github.io/sam-hq/`

### Vault Repo — Must Be Made Public
- Projects tab fetch fails because `sam-vault` is private
- Fix: `github.com/Eskroll/sam-vault` → Settings → Change visibility → Public
- Then use this base URL in the Projects tab:
  ```
  https://raw.githubusercontent.com/Eskroll/sam-vault/master/Mersen%20-%20ToDo/Projects
  ```
- Token-authenticated URLs (from private repo) don't work in browser fetch calls

---

## v5.1 Planned Features

### Project Card Detail Panel (click-to-expand)
When a project card is clicked, show a structured breakdown panel:
- Completed tasks (checked items from `## Completed`)
- Open tasks (unchecked items from `## Open Tasks`)
- Notes / Context section
- Clear visual separation of done vs. to-do
- Template-driven so each project renders consistently regardless of note structure

### Goal Tracker Improvements
- Start building out the Goals tab with more meaningful data
- Specifics TBD next session

---

## Open Threads

- [ ] **Make `sam-vault` repo public** — required for Projects tab to fetch notes in the browser
- [ ] **Deploy v5 to GitHub Pages** — upload as `index.html` to `Eskroll/sam-hq`, commit via web UI
- [ ] **Verify calGoal** — after next 8:30 AM `writeLoseItToSheet` trigger, confirm Sheet row 2 col C = correct goal value
- [ ] **Build v5.1** — project card detail panel (click-to-expand with done/todo breakdown) + Goal Tracker improvements
- [ ] **Manual goals** — enter current bench/OHP/squat/streak/lawn revenue in Setup tab
- [ ] **Step 4** — update morning + evening email digests with project snapshot (after v5 confirmed live)
- [ ] **Renew GitHub PAT** — expires ~Aug 2026, set a calendar reminder

---

## Notes
- v5 output file: `dashboard_v5.html` — rename to `index.html` on deploy
- Vault branch is `master` (not `main`)
- Quick Capture uses clipboard copy for now — GitHub API write requires a PAT stored server-side, not yet implemented
- All 4 project note files have frontmatter as of today; vault will push to GitHub on next auto-backup (5-min interval)
