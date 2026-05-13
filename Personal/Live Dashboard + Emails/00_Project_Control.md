---
type: project-control
project: Live Dashboard + Emails
area: personal
status: active
priority: high
canonical: true
last_updated: "2026-05-12 14:30"
next_action: "Port morning/evening digest triggers to Python cron on Oracle VM — remove Google Apps Script dependency"
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
- Calorie/nutrition data from Lose It! via SQLite API (Sheet fallback)
- Weight loss goal tracker (290 lbs by Sept 4, 2026)
- Body weight logging — dashboard input POSTs to SQLite API + Apps Script Sheet backup
- Body weight page: SQLite API primary, Sheet fallback. Chart with Full Cut / 2-Week tab toggle. History = rolling 30-day window. Data filtered to May 4 2026 (cut start) onward.
- Body measurements widget (placeholder — diagram + cards, data entry coming)
- Workout history tracking — sessions saved to localStorage, PR tracking, volume chart
- Work project status via Obsidian GitHub sync
- Notes tab — quick capture with project filter tags, localStorage
- Weather

Complemented by two AI-generated daily digest emails (Morning 5 AM, Evening 9:30 PM) built on Google Apps Script + Claude Haiku API.

**New stack live:** Python + SQLite + Flask running on Oracle Cloud free tier VM (always-on, 24/7). HTTPS via Cloudflare tunnel (systemd service). Health data pipeline via Apple Health → Health Auto Export → webhook → SQLite.

---

## Current Status

| Component | Version | Status |
|---|---|---|
| Dashboard HTML | v5.5 | **Live** on GitHub Pages — SQLite API primary, Sheet fallback |
| Morning Digest Email | v4 | **Live** — reads SQLite API via Cloudflare tunnel |
| Evening Digest Email | v4 | **Live** — reads SQLite API via Cloudflare tunnel |
| Lose It! → Sheet bridge | Script 1 | **Deprecated** — digest now reads SQLite API directly |
| Mersen Shipping Summary | Script 2 | Live, triggers 6:30 AM |
| Post-Workout Emailer | WorkoutEmailer script | Live — handles workout email + weight log Sheet backup |
| Weight log → SQLite API | `/api/weight` POST | **Live** — primary write path from dashboard |
| Weight log → Sheet | WorkoutEmailer doPost | Live — Sheet backup path (type=weight POSTs) |
| Weights tab CSV | hardcoded in HTML | Live — fallback only if SQLite unreachable |
| sam-vault GitHub sync | — | Live — public repo, Obsidian Git auto-push every 5 min |
| SQLite database (sam.db) | — | **LIVE on Oracle Cloud** |
| Flask API server | — | **LIVE on Oracle Cloud** — gunicorn systemd service, auto-restart |
| flask-cors | — | **LIVE** — CORS(app) in server.py |
| Cloudflare tunnel | cloudflared.service | **LIVE** — systemd service, permanent URL api.samhq.dev |
| Oracle Cloud VM | AMD E2.1.Micro | **LIVE** — Ubuntu 20.04, Chicago region, always-on |
| Health Auto Export webhook | — | **Live** — 15-min cadence, writing to SQLite |

---

## Open Threads

- [x] ~~**Wire digest emails to SQLite API**~~ — done 2026-05-08
- [x] ~~**Configure Health Auto Export webhook**~~ — done 2026-05-09
- [x] ~~**Get stable Cloudflare tunnel URL**~~ — done 2026-05-08, api.samhq.dev permanent
- [x] ~~**Wire dashboard to SQLite API**~~ — done 2026-05-08
- [x] ~~**Fix CORS on Flask server**~~ — done 2026-05-08
- [x] ~~**Body Weight page reads SQLite API**~~ — done 2026-05-12, v5.5. fetchWeightSheetData() hits /api/weight first, Sheet fallback. Log form POSTs to API + Sheet. Dashboard weight card also hits API.
- [x] ~~**Weight chart and history cleanup**~~ — done 2026-05-12, v5.5. Data filtered to May 4+ only. History = rolling 30-day window. Chart = Full Cut tab + 2-Week zoom tab.
- [ ] **Wire body measurements data entry** — placeholder UI; needs log form + Sheet tab
- [ ] **Renew GitHub PAT** — expires ~Aug 2026, set calendar reminder
- [ ] **Delete empty file `Claude Sessions/Personal/Live Dashboard + Emails/2026-05-07 11`** — naming artifact, do manually
- [ ] **Port digest email triggers to Python cron on Oracle VM** — remove Apps Script dependency entirely
- [ ] **Add /api/weight POST endpoint to Flask server** — currently GET only; dashboard POSTs to it but may 405. Needs: accept `{ date, weight }`, upsert into `daily_log.weight_lbs`.
- [ ] **Build weekly weight trend report** — Apps Script (future)
- [ ] **Build Mersen Friday work summary** — Apps Script (future)
- [ ] **Build gym progression tracker** — Apps Script (future)
- [ ] **Buy CanaKit Pi 4 2GB ($134.99)** — decided 2026-05-08, Amazon listing confirmed

---

## Key Architecture Decisions (Do Not Re-Debate)

### Current Stack (Apps Script — still live)
- **Single HTML file** — no framework, no build step. localStorage for config persistence.
- **Google Sheet as fallback** — Sheet still published; dashboard reads it only if SQLite API fails.
- **Weight is dual-write** — Dashboard POSTs to SQLite API (primary) AND WorkoutEmailer doPost (Sheet backup).
- **Weights CSV URL is hardcoded** in the HTML. Fallback only.
- **GitHub Pages** for dashboard hosting (free, always-on, accessible from phone)
- **sam-vault is public** — required for raw GitHub URLs to work in browser fetch()
- **vault branch is `master`** (not `main`)
- **Claude Haiku** for digest emails — ~$0.02/email
- **Open-Meteo API** for weather — free, no API key. Fields: `weather_code`, `wind_speed_10m`
- **Single DOMContentLoaded listener** — all nav/tab wiring inside one listener

### New Stack (Python + SQLite — LIVE on Oracle Cloud)
- **SQLite** (`sam.db`) — single file database, 4 tables: daily_log, workouts, lawn_jobs, project_notes
- **Flask + Gunicorn** — webhook at `/health`, API at `/api/today`, `/api/week`, `/api/weight`, `/api/stats`, `/ping`
- **flask-cors** — CORS(app) enables cross-origin requests from GitHub Pages
- **Systemd service** — `sam-server.service`, auto-restarts on crash/reboot
- **Cloudflare tunnel** — `cloudflared.service`, systemd, provides HTTPS. Permanent URL via named tunnel.
- **Oracle Cloud AMD E2.1.Micro** — free tier, always-on, public IP `164.152.111.208`, port 5000
- **Health Auto Export** — iOS app, REST API automation, posts Apple Health JSON to Flask webhook
- **Apple Health as data bridge** — Lose It syncs to Apple Health → Health Auto Export fires → Flask → SQLite
- **Parallel operation** — Google Sheet still live as fallback; SQLite is source of truth.
- **Platform path:** Oracle Cloud (now, always-on) → Raspberry Pi (physical layer, LEDs/display, later)
- **Pi chosen:** CanaKit Pi 4 2GB complete kit ($134.99, Amazon)
- **Weight data filter:** All UI displays filter to `>= 2026-05-04` (cut start date). Pre-cut data in DB but not shown.

### Body Weight Page Design (v5.5)
- **Chart tabs:** "Full Cut" = all entries May 4→today; "2 Weeks" = last 14 days zoom. Tab state stored in `_wtChartMode`, log cached in `_wtChartLog`.
- **History list:** Rolling 30-day window, newest first, entry count badge. Fallback to last 14 if window empty.
- **Log form:** POSTs `{ date, weight }` to `/api/weight` (primary), fires Script 3 Sheet POST as silent backup. Status message reports which path succeeded.
- **Dashboard weight card:** Still populated by `fetchLoseItData()` → `renderDashWeightFromApi()` from `/api/today`.

---

## Version History

| Version | Date | Key Changes |
|---|---|---|
| v5.5 | 2026-05-12 | Body Weight page: SQLite API primary for reads + writes. Chart two-tab (Full Cut / 2-Week). History rolling 30-day. May 4 cutoff filter. Dual-write log (API + Sheet backup). |
| Digest v4 | 2026-05-08 | getLoseItFromAPI() replaces Sheet path; hits /api/today, /api/week fallback |
| Dashboard API wire + CORS + Cloudflare | 2026-05-08 | Dashboard reads SQLite API over HTTPS, Sheet fallback, Cloudflare tunnel systemd |
| Oracle Cloud + SQLite | 2026-05-08 | Full backend live: VM, Python, SQLite, Flask, gunicorn, systemd |
| Digest v3 | 2026-05-07 | Weather API field fix, weight from Sheet weights tab |
| v5.4 | 2026-05-07 | Rest timers, body measurements widget, Notes tab, DOMContentLoaded bug fix |
| v5.3 | 2026-05-07 | Hardcoded weights CSV URL, weight card reads Sheet |
| v5.2 | 2026-05-07 | Manual weight log, body weight page, workout history page |
| v5.1 | 2026-05-07 | Weight pipeline, workout email fix |
| v4.x | 2026-05-06 | Initial GitHub Pages deploy, bug fixes |

---

## Session Notes (Historical — Do Not Load Unless Needed)

Located in `Claude Sessions/Personal/Live Dashboard + Emails/`:
- `2026-05-12 14:30 weight-page-sqlite-api-chart-tabs.md` — v5.5: Body Weight page reads SQLite API, dual-write log, two-tab chart, 30-day history, May4 filter
- `2026-05-08 18:30 digest-v4-sqlite-api.md` — Digest v4: getLoseItFromAPI() replaces Sheet path
- `2026-05-08 17:45 dashboard-api-cors-cloudflare.md` — Dashboard wired to SQLite API, CORS fixed, Cloudflare systemd
- `2026-05-08 16:45 oracle-cloud-sqlite-flask-live.md` — Oracle Cloud VM setup, SQLite import, Flask deployed
- `2026-05-08 11:30 python-sqlite-stack-homelab.md` — New stack design: Python+SQLite+Flask, Pi hardware decision
- `2026-05-07 19:30 dashboard-v5-2-build.md` — v5.2 build + weight pipeline fix
- `2026-05-07 21:45 dashboard-v5-3-v5-4-build.md` — v5.3 + v5.4 build
- `2026-05-08 00:00 digest-v3-weather-weight-fix.md` — digest v3: weather API fix + weight from Sheet
