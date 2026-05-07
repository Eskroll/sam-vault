---
type: project-control
project: Live Dashboard + Emails
area: personal
status: active
priority: high
canonical: true
last_updated: '"2026-05-07 14:30"'
next_action: '"Delete Templates/Mersen Work Request.md (replaced by Work Request.md)"'
context_load_order:
  - 00_Project_Control
  - 04_Links_And_Paths
  - 01_Current_State
archive: false
---

# Live Dashboard + Emails — Project Control

**Vault hub:** [[INDEX]]
**This project:** [[Personal/Live Dashboard + Emails/01_Current_State]] | [[Personal/Live Dashboard + Emails/04_Links_And_Paths]]

> **Canonical source of truth. Read this first.**

---

## What This Project Is

A self-hosted personal operating system. One HTML file deployed to GitHub Pages that gives Sam a single URL (phone + PC) for:
- Morning routine checklist
- Workout logging + PPL schedule
- Calorie/nutrition data from Lose It! via Google Sheet
- Weight loss goal tracker
- Work project status pulled live from Obsidian vault on GitHub
- Weather

Complemented by two AI-generated daily digest emails (Morning 5 AM, Evening 9:30 PM) built on Google Apps Script + Claude Haiku API.

---

## Current Status

| Component | Version | Status |
|---|---|---|
| Dashboard HTML | v4.2 | Live on GitHub Pages — calGoal fix applied |
| Dashboard HTML | v5 | Built, not yet deployed |
| Morning Digest Email | — | Live, sending 5 AM daily |
| Evening Digest Email | — | Live, sending 9:30 PM daily |
| Lose It! → Sheet bridge | Script 1 | Live, triggers 7:00 AM |
| Mersen Shipping Summary | Script 2 | Live, triggers 6:30 AM |
| Post-Workout Emailer | Script 3 | Live, HTTP POST from dashboard |

---

## Open Threads (Consolidated)

- [ ] **Make `sam-vault` repo public** — required for browser fetch() to work in Projects tab
- [ ] **Deploy dashboard v5** — upload as `index.html` to `Eskroll/sam-hq` GitHub Pages
- [ ] **Update Projects tab base URL** → `Work/Projects/` (was `Mersen - ToDo/Projects/`)
- [ ] **Verify calGoal in Sheet** — after 8:30 AM trigger, confirm row 2 col C = 2190
- [ ] **Build dashboard v5.1** — click-to-expand project cards + Goal Tracker improvements
- [ ] **Renew GitHub PAT** — expires ~Aug 2026, set calendar reminder
- [ ] **Build weekly weight trend report** — Apps Script
- [ ] **Build Mersen Friday work summary** — Apps Script
- [ ] **Build gym progression tracker** — Apps Script
- [ ] **Steps** — will auto-populate when fitness tracker connected to Lose It
- [ ] **Delete `Templates/Mersen Work Request.md`** — superseded by Work Request.md (do manually in Obsidian)
- [ ] **Delete empty file `Claude Sessions/Personal/Live Dashboard + Emails/2026-05-07 11`** — naming artifact, do manually
- [x] ~~**Add Beginning-of-Chat Prompt to Claude Sessions/README.md**~~ — done 2026-05-07
- [x] ~~**Add wikilinks to all vault files for graph connectivity**~~ — done 2026-05-07 (all templates, session notes, task files, READMEs, project files, syllabi)

---

## Key Architecture Decisions (Do Not Re-Debate)

- **Single HTML file** — no framework, no build step. localStorage for config persistence.
- **Google Sheet as data bridge** — Lose It! email → Apps Script parser → Sheet → dashboard reads published CSV
- **GitHub Pages** for dashboard hosting (free, always-on, accessible from phone)
- **sam-vault must be public** for raw GitHub URLs to work in browser fetch() without auth
- **vault branch is `master`** (not `main`)
- **Claude Haiku** for digest emails — ~$0.02/email, ~$1.20–1.50/month
- **Open-Meteo API** for weather — free, no API key required
- **Obsidian** is project notes source of truth; **GitHub** is code source of truth

---

## Session Notes (Historical — Do Not Load Unless Needed)

Located in `Claude Sessions/Personal/Live Dashboard + Emails/`:
- `2026-05-06-claude-session-system.md` — session system setup (superseded)
- `2026-05-06-personal-dashboard.md` — v4 build history
- `2026-05-06-obsidian-git-dashboard-v5-plan.md` — GitHub/Git setup
- `2026-05-06-dashboard-v5-build.md` — v5 build details
- `2026-05-06-productivity-automation-system.md` — Google Calendar + digest setup
- `2026-05-07-obsidian-github-setup.md` — desktop setup, PowerShell bridge
- `2026-05-07-vault-restructure.md` — vault reorganization record
- `2026-05-07 12:30 vault-graph-linking.md` — Beginning-of-Chat prompt + full graph wikilink pass
