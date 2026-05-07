---
type: project-control
project: Live Dashboard + Emails
area: personal
status: active
priority: high
canonical: true
last_updated: "2026-05-07 19:30"
next_action: "Commit + push Obsidian Git after this update, then verify weights tab in Sheet populates on next dashboard log"
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
- Weight loss goal tracker (290 lbs by Sept 4, 2026)
- Manual body weight logging — direct input on dashboard, POSTs to Apps Script → writes to `weights` tab in Google Sheet
- Workout history tracking — sessions saved to localStorage, PR tracking, volume chart
- Work project status via Obsidian GitHub sync
- Weather

Complemented by two AI-generated daily digest emails (Morning 5 AM, Evening 9:30 PM) built on Google Apps Script + Claude Haiku API.

---

## Current Status

| Component | Version | Status |
|---|---|---|
| Dashboard HTML | v5.2 | **Current live** — built this session |
| Morning Digest Email | — | Live, sending 5 AM daily |
| Evening Digest Email | — | Live, sending 9:30 PM daily |
| Lose It! → Sheet bridge | Script 1 | Live, triggers 7:00 AM |
| Mersen Shipping Summary | Script 2 | Live, triggers 6:30 AM |
| Post-Workout Emailer | WorkoutEmailer script | Live — deployed Access: Anyone, weight routing fixed |
| Weight log → Sheet | WorkoutEmailer doPost | Live — type=weight POSTs write to `weights` tab |
| sam-vault GitHub sync | — | Live — public repo, Obsidian Git auto-push every 5 min |
| INDEX.md Dashboard Links | — | Added this session — projects load as clickable cards |
| Google Sheet weights tab | — | Created this session — `date | weight` schema |

---

## Open Threads

- [ ] **Commit + push Obsidian Git** — INDEX.md and canonical files updated this session
- [ ] **Verify weights tab** — log weight from dashboard, confirm row appears in Sheet weights tab
- [ ] **Verify calGoal in Sheet** — after 8:30 AM trigger, confirm row 2 col C = 2190
- [ ] **Renew GitHub PAT** — expires ~Aug 2026, set calendar reminder
- [ ] **Delete empty file `Claude Sessions/Personal/Live Dashboard + Emails/2026-05-07 11`** — naming artifact, do manually
- [ ] **Build weekly weight trend report** — Apps Script (future)
- [ ] **Build Mersen Friday work summary** — Apps Script (future)
- [ ] **Build gym progression tracker** — Apps Script (future)
- [ ] **Steps** — will auto-populate when fitness tracker connected to Lose It
- [x] ~~**Deploy dashboard v5.2**~~ — done 2026-05-07 (weight log, body weight chart, workout history page, GitHub MD null fix)
- [x] ~~**Fix weight POST sending workout email**~~ — done 2026-05-07 (doPost routing fixed, SHEET_ID added to Script Properties, redeployed)
- [x] ~~**Add SHEET_ID to WorkoutEmailer Script Properties**~~ — done 2026-05-07
- [x] ~~**Add weights tab to Google Sheet**~~ — done 2026-05-07 (`date | weight` headers)
- [x] ~~**Add Dashboard Links to INDEX.md**~~ — done 2026-05-07 (Claude wrote directly via Obsidian MCP)
- [x] ~~**Fix Post-Workout Emailer**~~ — done prior session (Access: Anyone)
- [x] ~~**Make `sam-vault` repo public**~~ — done 2026-05-07
- [x] ~~**Obsidian Git auto-push configured**~~ — done 2026-05-07
- [x] ~~**Build dashboard v5.1**~~ — done 2026-05-07

---

## Key Architecture Decisions (Do Not Re-Debate)

- **Single HTML file** — no framework, no build step. localStorage for config persistence.
- **Google Sheet as data bridge** — Lose It! email → Apps Script parser → Sheet → dashboard reads published CSV
- **Weight is now manual** — no longer parsed from Lose It! email. Dashboard input → POST → WorkoutEmailer doPost → `weights` tab in Sheet. Old `weight` column in Sheet1 can be deleted.
- **WorkoutEmailer doPost routing** — parses JSON first, checks `type === 'weight'` to route to weight handler, else falls through to workout email. Weight handler returns early — never sends email.
- **GitHub Pages** for dashboard hosting (free, always-on, accessible from phone)
- **sam-vault is public** — required for raw GitHub URLs to work in browser fetch()
- **vault branch is `master`** (not `main`)
- **Claude Haiku** for digest emails — ~$0.02/email, ~$1.20–1.50/month
- **Open-Meteo API** for weather — free, no API key required
- **Post-Workout Emailer must be deployed with Access: Anyone**
- **Obsidian** is project notes source of truth; **GitHub** is code source of truth
- **Claude has Obsidian MCP access** — can read/write vault files directly, no manual copy-paste needed

---

## v5.2 Feature Summary (built 2026-05-07)

1. **Manual weight log card** — dashboard weight card replaced with input form. Logs to localStorage + POSTs to WorkoutEmailer as `{type:"weight", date, weight}`. Shows "LOGGED" badge once entered for today.
2. **Body Weight page** — new nav item ⚖️. Start/current/goal/remaining stat cards, progress bar, canvas trend chart, pace analysis (on pace for Sept 4?), full history table, log form.
3. **Workout History page** — new nav item 📊. Total sessions/sets/this week stats, sets-per-session bar chart, exercise PR tracker, session log (last 20). Auto-saves on "Finish & Send Email".
4. **GitHub markdown null error fixed** — all `getElementById` calls before `.innerHTML` now null-checked. `marked.parse` wrapped with fallback.
5. **INDEX.md Dashboard Links** — 4 project links added so Projects left panel populates with clickable cards.

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
- `2026-05-07-dashboard-v5-1-build.md` — v5.1 build + GitHub sync troubleshooting
- `2026-05-07 19:30 dashboard-v5-2-build.md` — v5.2 build + weight pipeline fix (this session)
