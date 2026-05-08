---
type: project-control
project: Live Dashboard + Emails
area: personal
status: active
priority: high
canonical: true
last_updated: "2026-05-08 16:45"
next_action: "Wire digest emails to pull weight/macros from http://164.152.111.208:5000/api/today instead of Google Sheet. Then configure Health Auto Export webhook to POST to same URL."
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
- Calorie/nutrition data from Lose It! via Google Sheet (migrating to SQLite)
- Weight loss goal tracker (290 lbs by Sept 4, 2026)
- Manual body weight logging — dashboard input POSTs to Apps Script → writes to `weights` tab in Google Sheet
- Body measurements widget (placeholder — diagram + cards, data entry coming)
- Workout history tracking — sessions saved to localStorage, PR tracking, volume chart
- Work project status via Obsidian GitHub sync
- Notes tab — quick capture with project filter tags, localStorage
- Weather

Complemented by two AI-generated daily digest emails (Morning 5 AM, Evening 9:30 PM) built on Google Apps Script + Claude Haiku API.

**New stack live:** Python + SQLite + Flask running on Oracle Cloud free tier VM (always-on, 24/7). Health data pipeline via Apple Health → Health Auto Export → webhook → SQLite.

---

## Current Status

| Component | Version | Status |
|---|---|---|
| Dashboard HTML | v5.4 | **Live** on GitHub Pages |
| Morning Digest Email | v3 | **Live** — still reading from Google Sheet |
| Evening Digest Email | v3 | **Live** — still reading from Google Sheet |
| Lose It! → Sheet bridge | Script 1 | Live, triggers 7:00 AM |
| Mersen Shipping Summary | Script 2 | Live, triggers 6:30 AM |
| Post-Workout Emailer | WorkoutEmailer script | Live — handles workout email + weight log routing |
| Weight log → Sheet | WorkoutEmailer doPost | Live — type=weight POSTs write to `weights` tab |
| Weights tab CSV | hardcoded in HTML | Live — all 3 devices read same URL |
| sam-vault GitHub sync | — | Live — public repo, Obsidian Git auto-push every 5 min |
| SQLite database (sam.db) | — | **LIVE on Oracle Cloud** — 38 days health data imported |
| Flask API server | — | **LIVE on Oracle Cloud** — gunicorn systemd service, auto-restart |
| Oracle Cloud VM | AMD E2.1.Micro | **LIVE** — Ubuntu 20.04, Chicago region, always-on |
| Health Auto Export webhook | — | **Not yet configured** — manual CSV import working |

---

## Open Threads

- [ ] **Wire digest emails to SQLite API** — update Apps Script to call `http://164.152.111.208:5000/api/today` for weight/macros instead of Google Sheet
- [ ] **Wire dashboard to SQLite API** — replace Sheet CSV fetch with API call in dashboard HTML
- [ ] **Configure Health Auto Export webhook** — point to `http://164.152.111.208:5000/health`, 15-min cadence
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
- [ ] **Buy CanaKit Pi 4 2GB ($134.99)** — decided 2026-05-08, Amazon listing confirmed
- [x] ~~**Set up Oracle Cloud free tier**~~ — done 2026-05-08, AMD E2.1.Micro, Chicago
- [x] ~~**Install Python + run setup_db.py**~~ — done 2026-05-08, sam.db created
- [x] ~~**Import health CSV into SQLite**~~ — done 2026-05-08, 38 rows imported
- [x] ~~**Run SQL queries to verify data**~~ — done 2026-05-08, all columns verified
- [x] ~~**Deploy Flask server to Oracle Cloud**~~ — done 2026-05-08, gunicorn systemd service
- [x] ~~**Deploy digest_v3.js to Apps Script**~~ — done 2026-05-07/08
- [x] ~~**Deploy dashboard v5.4**~~ — done 2026-05-07
- [x] ~~**Fix weight POST sending workout email**~~ — done 2026-05-07
- [x] ~~**Add weights tab to Google Sheet**~~ — done 2026-05-07
- [x] ~~**Make `sam-vault` repo public**~~ — done 2026-05-07

---

## Key Architecture Decisions (Do Not Re-Debate)

### Current Stack (Apps Script — still live)
- **Single HTML file** — no framework, no build step. localStorage for config persistence.
- **Google Sheet as data bridge** — Lose It! email → Apps Script parser → Sheet → dashboard reads published CSV
- **Weight is manual only** — Dashboard input → POST → WorkoutEmailer doPost → `weights` tab in Sheet.
- **Weights CSV URL is hardcoded** in the HTML. All devices read same Sheet without per-device Setup.
- **GitHub Pages** for dashboard hosting (free, always-on, accessible from phone)
- **sam-vault is public** — required for raw GitHub URLs to work in browser fetch()
- **vault branch is `master`** (not `main`)
- **Claude Haiku** for digest emails — ~$0.02/email
- **Open-Meteo API** for weather — free, no API key. Fields: `weather_code`, `wind_speed_10m`
- **Single DOMContentLoaded listener** — all nav/tab wiring inside one listener

### New Stack (Python + SQLite — LIVE on Oracle Cloud)
- **SQLite** (`sam.db`) — single file database, 4 tables: daily_log, workouts, lawn_jobs, project_notes
- **Flask + Gunicorn** — webhook at `/health`, API at `/api/today`, `/api/week`, `/api/weight`, `/api/stats`, `/ping`
- **Systemd service** — `sam-server.service`, auto-restarts on crash/reboot
- **Oracle Cloud AMD E2.1.Micro** — free tier, always-on, public IP `164.152.111.208`, port 5000
- **Health Auto Export** — iOS app, REST API automation, posts Apple Health JSON to Flask webhook
- **Apple Health as data bridge** — Lose It syncs to Apple Health → Health Auto Export fires → Flask → SQLite
- **Parallel operation** — Google Sheet still live; SQLite is new source of truth. Cutover happens when digest + dashboard are rewired.
- **Platform path:** Oracle Cloud (now, always-on) → Raspberry Pi (physical layer, LEDs/display, later)
- **Pi chosen:** CanaKit Pi 4 2GB complete kit ($134.99, Amazon)
- **import_health.py** — manual CSV import script, run as needed until webhook is live
- **ARM capacity unavailable** in Chicago — using AMD E2.1.Micro instead, sufficient for this workload

---

## Version History

| Version | Date | Key Changes |
|---|---|---|
| Oracle Cloud + SQLite | 2026-05-08 | Full backend live: VM, Python, SQLite, Flask, gunicorn, systemd |
| Digest v3 | 2026-05-07 | Weather API field fix, weight from Sheet `weights` tab |
| v5.4 | 2026-05-07 | Rest timers, body measurements widget, Notes tab, DOMContentLoaded bug fix |
| v5.3 | 2026-05-07 | Hardcoded weights CSV URL, weight card reads Sheet |
| v5.2 | 2026-05-07 | Manual weight log, body weight page, workout history page |
| v5.1 | 2026-05-07 | Weight pipeline, workout email fix |
| v4.x | 2026-05-06 | Initial GitHub Pages deploy, bug fixes |

---

## Session Notes (Historical — Do Not Load Unless Needed)

Located in `Claude Sessions/Personal/Live Dashboard + Emails/`:
- `2026-05-08 16:45 oracle-cloud-sqlite-flask-live.md` — Oracle Cloud VM setup, SQLite import, Flask deployed, gunicorn systemd service
- `2026-05-08 11:30 python-sqlite-stack-homelab.md` — New stack design: Python+SQLite+Flask, Pi hardware decision, Apple Health pipeline
- `2026-05-07 19:30 dashboard-v5-2-build.md` — v5.2 build + weight pipeline fix
- `2026-05-07 21:45 dashboard-v5-3-v5-4-build.md` — v5.3 + v5.4 build
- `2026-05-08 00:00 digest-v3-weather-weight-fix.md` — digest v3: weather API fix + weight from Sheet
