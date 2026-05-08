---
type: project-control
project: Live Dashboard + Emails
area: personal
status: active
priority: high
canonical: true
last_updated: "2026-05-07 21:45"
next_action: "Push dashboard_v5_4.html to GitHub Pages (replace index.html in sam-hq repo), then verify weights tab populates on next log"
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
- Workout logging + PPL schedule + rest timers (1:15 set, 2:00 lift)
- Calorie/nutrition data from Lose It! via Google Sheet
- Weight loss goal tracker (290 lbs by Sept 4, 2026)
- Manual body weight logging — dashboard input POSTs to Apps Script → writes to `weights` tab in Google Sheet (hardcoded URL, all devices see same data)
- Body measurements widget (placeholder — diagram + cards, data entry coming)
- Workout history tracking — sessions saved to localStorage, PR tracking, volume chart
- Work project status via Obsidian GitHub sync
- Notes tab — quick capture with project filter tags, localStorage
- Weather

Complemented by two AI-generated daily digest emails (Morning 5 AM, Evening 9:30 PM) built on Google Apps Script + Claude Haiku API.

---

## Current Status

| Component | Version | Status |
|---|---|---|
| Dashboard HTML | v5.4 | **Current live** — built this session |
| Morning Digest Email | — | Live, sending 5 AM daily |
| Evening Digest Email | — | Live, sending 9:30 PM daily |
| Lose It! → Sheet bridge | Script 1 | Live, triggers 7:00 AM |
| Mersen Shipping Summary | Script 2 | Live, triggers 6:30 AM |
| Post-Workout Emailer | WorkoutEmailer script | Live — handles workout email + weight log routing |
| Weight log → Sheet | WorkoutEmailer doPost | Live — type=weight POSTs write to `weights` tab |
| Weights tab CSV | hardcoded in HTML | Live — all 3 devices read same URL, no Setup needed |
| sam-vault GitHub sync | — | Live — public repo, Obsidian Git auto-push every 5 min |

---

## Open Threads

- [ ] **Push dashboard_v5_4.html to GitHub Pages** — replace `index.html` in sam-hq repo
- [ ] **Verify weights tab** — log weight from dashboard on each device, confirm row appears in Sheet
- [ ] **Wire body measurements data entry** — currently placeholder UI; needs log form + Sheet tab
- [ ] **Verify calGoal in Sheet** — after 8:30 AM trigger, confirm row 2 col C = 2190
- [ ] **Renew GitHub PAT** — expires ~Aug 2026, set calendar reminder
- [ ] **Delete empty file `Claude Sessions/Personal/Live Dashboard + Emails/2026-05-07 11`** — naming artifact, do manually
- [ ] **Build weekly weight trend report** — Apps Script (future)
- [ ] **Build Mersen Friday work summary** — Apps Script (future)
- [ ] **Build gym progression tracker** — Apps Script (future)
- [ ] **Steps** — will auto-populate when fitness tracker connected to Lose It
- [x] ~~**Deploy dashboard v5.4**~~ — done 2026-05-07 (timers, body measurements widget, Notes tab, bug fix)
- [x] ~~**Fix v5.4 duplicate DOMContentLoaded bug**~~ — done 2026-05-07 (tabs broken, data not fetching — merged into single listener)
- [x] ~~**Deploy dashboard v5.3**~~ — done 2026-05-07 (hardcoded weights CSV, weight card reads Sheet, all-device weight log)
- [x] ~~**Deploy dashboard v5.2**~~ — done 2026-05-07 (weight log, body weight chart, workout history page, GitHub MD null fix)
- [x] ~~**Fix weight POST sending workout email**~~ — done 2026-05-07
- [x] ~~**Add SHEET_ID to WorkoutEmailer Script Properties**~~ — done 2026-05-07
- [x] ~~**Add weights tab to Google Sheet**~~ — done 2026-05-07
- [x] ~~**Fix Post-Workout Emailer**~~ — done prior session
- [x] ~~**Make `sam-vault` repo public**~~ — done 2026-05-07

---

## Key Architecture Decisions (Do Not Re-Debate)

- **Single HTML file** — no framework, no build step. localStorage for config persistence.
- **Google Sheet as data bridge** — Lose It! email → Apps Script parser → Sheet → dashboard reads published CSV
- **Weight is manual only** — no longer parsed from Lose It! email. Dashboard input → POST → WorkoutEmailer doPost → `weights` tab in Sheet.
- **Weights CSV URL is hardcoded** in the HTML (`WEIGHTS_CSV_URL` constant at top of script block). This means all devices read the same Sheet without any per-device Setup entry. Only the calorie Sheet1 URL still requires Setup.
- **WorkoutEmailer doPost routing** — parses JSON first, checks `type === 'weight'` to route to weight handler, else falls through to workout email.
- **GitHub Pages** for dashboard hosting (free, always-on, accessible from phone)
- **sam-vault is public** — required for raw GitHub URLs to work in browser fetch()
- **vault branch is `master`** (not `main`)
- **Claude Haiku** for digest emails — ~$0.02/email
- **Open-Meteo API** for weather — free, no API key
- **Post-Workout Emailer must be deployed with Access: Anyone**
- **Single DOMContentLoaded listener** — all nav wiring, tab wiring, and page-specific init must live inside one listener to avoid overwrite conflicts.
- **Obsidian** is project notes source of truth; **GitHub** is code source of truth
- **Claude has Obsidian MCP access** — can read/write vault files directly

---

## Version History

| Version | Date | Key Changes |
|---|---|---|
| v5.4 | 2026-05-07 | Rest timers (1:15 set, 2:00 lift), body measurements widget (placeholder), Notes tab with project filter, bug fix: duplicate DOMContentLoaded |
| v5.3 | 2026-05-07 | Hardcoded weights CSV URL, weight card reads from weights tab (all devices), separated weight from calorie Sheet |
| v5.2 | 2026-05-07 | Manual weight log, body weight page, workout history page, GitHub MD null fix |
| v5.1 | 2026-05-07 | Weight pipeline, workout email fix |
| v4.x | 2026-05-06 | Initial GitHub Pages deploy, bug fixes |

---

## Session Notes (Historical — Do Not Load Unless Needed)

Located in `Claude Sessions/Personal/Live Dashboard + Emails/`:
- `2026-05-07 19:30 dashboard-v5-2-build.md` — v5.2 build + weight pipeline fix
- `2026-05-07 21:45 dashboard-v5-3-v5-4-build.md` — v5.3 + v5.4 build (this session)
- (older sessions archived — see prior control file versions)
