---
type: project-control
project: Live Dashboard + Emails
area: personal
status: active
priority: high
canonical: true
last_updated: "2026-05-08 11:30"
next_action: "At work: install Python, run setup_db.py, import HealthAutoExport CSV into SQLite, run Phase 3 queries to verify data. At home: run server.py, configure Health Auto Export webhook to localhost:5000/health"
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

**In progress (new stack):** Python + SQLite + Flask backend to replace Apps Script + Google Sheets. Health data pipeline via Apple Health → Health Auto Export → webhook → SQLite. See session note 2026-05-08.

---

## Current Status

| Component | Version | Status |
|---|---|---|
| Dashboard HTML | v5.4 | **Current live** — built this session |
| Morning Digest Email | v3 | **Live** — weather fix + weight from Sheet (digest_v3.js, deployed 2026-05-07) |
| Evening Digest Email | v3 | **Live** — weather fix + weight from Sheet (digest_v3.js, deployed 2026-05-07) |
| Lose It! → Sheet bridge | Script 1 | Live, triggers 7:00 AM |
| Mersen Shipping Summary | Script 2 | Live, triggers 6:30 AM |
| Post-Workout Emailer | WorkoutEmailer script | Live — handles workout email + weight log routing |
| Weight log → Sheet | WorkoutEmailer doPost | Live — type=weight POSTs write to `weights` tab |
| Weights tab CSV | hardcoded in HTML | Live — all 3 devices read same URL, no Setup needed |
| sam-vault GitHub sync | — | Live — public repo, Obsidian Git auto-push every 5 min |
| SQLite database (sam.db) | — | **In progress** — schema designed, import script ready, not yet built |
| Flask webhook server | — | **In progress** — server.py written, not yet deployed |
| Health Auto Export webhook | — | **In progress** — app on phone, manual export done, automation not yet configured |

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
- [ ] **NEW — Phase 1: Install Python + run setup_db.py at work** — creates sam.db with 4 tables
- [ ] **NEW — Phase 2: Import health CSV at work** — run import_health.py with HealthAutoExport export
- [ ] **NEW — Phase 3: Run SQL queries at work** — verify data, learn the query patterns
- [ ] **NEW — Phase 4: Run server.py at home** — Flask server + test /api/today endpoint
- [ ] **NEW — Configure Health Auto Export webhook** — point to localhost:5000/health (home WiFi), 15-min cadence
- [ ] **NEW — Buy CanaKit Pi 4 2GB ($134.99)** — decided this session, Amazon listing confirmed
- [ ] **NEW — Set up Oracle Cloud free tier** — for always-on scheduler when ready (summer)
- [x] ~~**Deploy digest_v3.js to Apps Script**~~ — done 2026-05-07/08
- [x] ~~**Deploy dashboard v5.4**~~ — done 2026-05-07
- [x] ~~**Fix v5.4 duplicate DOMContentLoaded bug**~~ — done 2026-05-07
- [x] ~~**Deploy dashboard v5.3**~~ — done 2026-05-07
- [x] ~~**Deploy dashboard v5.2**~~ — done 2026-05-07
- [x] ~~**Fix weight POST sending workout email**~~ — done 2026-05-07
- [x] ~~**Add SHEET_ID to WorkoutEmailer Script Properties**~~ — done 2026-05-07
- [x] ~~**Add weights tab to Google Sheet**~~ — done 2026-05-07
- [x] ~~**Fix Post-Workout Emailer**~~ — done prior session
- [x] ~~**Make `sam-vault` repo public**~~ — done 2026-05-07

---

## Key Architecture Decisions (Do Not Re-Debate)

### Current Stack (Apps Script)
- **Single HTML file** — no framework, no build step. localStorage for config persistence.
- **Google Sheet as data bridge** — Lose It! email → Apps Script parser → Sheet → dashboard reads published CSV
- **Weight is manual only** — no longer parsed from Lose It! email. Dashboard input → POST → WorkoutEmailer doPost → `weights` tab in Sheet.
- **Weights CSV URL is hardcoded** in the HTML. All devices read same Sheet without per-device Setup.
- **WorkoutEmailer doPost routing** — parses JSON first, checks `type === 'weight'` to route to weight handler.
- **GitHub Pages** for dashboard hosting (free, always-on, accessible from phone)
- **sam-vault is public** — required for raw GitHub URLs to work in browser fetch()
- **vault branch is `master`** (not `main`)
- **Claude Haiku** for digest emails — ~$0.02/email
- **Open-Meteo API** for weather — free, no API key. Fields: `weather_code`, `wind_speed_10m`
- **Single DOMContentLoaded listener** — all nav/tab wiring inside one listener

### New Stack (Python + SQLite — being built)
- **SQLite** (`sam.db`) — single file database, 4 tables: daily_log, workouts, lawn_jobs, project_notes
- **Flask** — webhook receiver at `/health`, API endpoints at `/api/today`, `/api/week`, `/api/weight`, `/api/stats`
- **Health Auto Export** — iOS app, REST API automation, posts Apple Health JSON to Flask webhook
- **Apple Health as data bridge** — Lose It syncs to Apple Health in real time → Health Auto Export fires → Flask → SQLite. Eliminates 24-hr email delay.
- **Metric map** — Health Auto Export metric names → SQLite column names defined in server.py
- **Windows Task Scheduler** now → cron on Pi/Oracle later. Same Python scripts, zero changes.
- **Platform path:** Windows PC (learn) → Oracle Cloud free tier (always-on, summer) → Raspberry Pi (physical layer, LEDs/display)
- **Pi chosen:** CanaKit Pi 4 2GB complete kit ($134.99, Amazon) — ordered/to be ordered
- **Lose It is a dead end for APIs** — no public API, email parse is ceiling. Apple Health bridge is the correct long-term path.
- **Health Auto Export iOS limitation** — cannot guarantee 5-min cadence; iOS controls background timing. Realistic: hourly average. 15-min target cadence set in app.
- **Oracle Cloud free tier** — 2 ARM VMs permanently free, runs Ubuntu, same Python/cron as Pi. Use for scheduler before Pi arrives.

---

## Version History

| Version | Date | Key Changes |
|---|---|---|
| Digest v3 | 2026-05-07 | Weather API field fix, weight from Sheet `weights` tab |
| v5.4 | 2026-05-07 | Rest timers, body measurements widget, Notes tab, DOMContentLoaded bug fix |
| v5.3 | 2026-05-07 | Hardcoded weights CSV URL, weight card reads Sheet |
| v5.2 | 2026-05-07 | Manual weight log, body weight page, workout history page |
| v5.1 | 2026-05-07 | Weight pipeline, workout email fix |
| v4.x | 2026-05-06 | Initial GitHub Pages deploy, bug fixes |

---

## Session Notes (Historical — Do Not Load Unless Needed)

Located in `Claude Sessions/Personal/Live Dashboard + Emails/`:
- `2026-05-08 11:30 python-sqlite-stack-homelab.md` — New stack design: Python+SQLite+Flask, Pi hardware decision, Apple Health pipeline, homelab exploration
- `2026-05-07 19:30 dashboard-v5-2-build.md` — v5.2 build + weight pipeline fix
- `2026-05-07 21:45 dashboard-v5-3-v5-4-build.md` — v5.3 + v5.4 build
- `2026-05-08 00:00 digest-v3-weather-weight-fix.md` — digest v3: weather API fix + weight from Sheet
