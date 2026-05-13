---
type: project-state
project: Live Dashboard + Emails
canonical: true
last_updated: "2026-05-12 14:30"
---

# Live Dashboard + Emails — Current State

**Canonical hub:** [[Personal/Live Dashboard + Emails/00_Project_Control]] | [[INDEX]]
**Related:** [[Personal/Live Dashboard + Emails/04_Links_And_Paths]]

---

## Dashboard Versions

| Version | Status | Notes |
|---|---|---|
| v3 | Superseded | Initial build |
| v4.x | Superseded | Bug fixes: label search, parser, weather retry, CSV parsing, GitHub Pages |
| v5.1 | Superseded | Manual weight log, 3-tab projects, workout email fix |
| v5.2 | Superseded | Weight pipeline, body weight page, workout history page, MD null fix |
| v5.3 | Superseded | Hardcoded weights CSV, weight card reads Sheet, all-device weight log |
| v5.4 | Superseded | SQLite API primary, Sheet fallback. CORS fixed. Cloudflare HTTPS confirmed. |
| Digest v4 | **Current live** | getLoseItFromAPI() hits SQLite /api/today, falls back /api/week. Sheet path removed. |
| v5.5 | **Current live** | Body Weight page: SQLite API primary for reads + writes. Chart two-tab. 30-day history. May 4 filter. |

---

## Google Apps Scripts

| # | Script | Trigger | Status |
|---|---|---|---|
| 1 | Lose It! → Sheet writer (`writeLoseItToSheet`) | Time-based 7:00 AM | **Deprecated** — digest reads SQLite directly, trigger can be deleted |
| 2 | Mersen Shipping Summary | Time-based 6:30 AM | Live |
| 3 | WorkoutEmailer (doPost) | HTTP POST from dashboard | Live — handles workout email + weight log Sheet backup |
| 4 | Morning Digest | Time-based 5:00 AM | Live — **v4** (reads SQLite API via Cloudflare tunnel) |
| 5 | Evening Digest | Time-based 9:30 PM | Live — **v4** (reads SQLite API via Cloudflare tunnel) |

---

## Oracle Cloud Server — LIVE

| Field | Value |
|---|---|
| Provider | Oracle Cloud Free Tier |
| Region | US Midwest (Chicago) — us-chicago-1 |
| Shape | VM.Standard.E2.1.Micro (AMD) |
| OS | Ubuntu 20.04 LTS |
| Public IP | 164.152.111.208 |
| Port | 5000 |
| Service | sam-server.service (gunicorn + systemd) |
| Auto-restart | Yes — systemd Restart=always |
| Survives reboot | Yes — systemd enabled |
| Files location | /home/ubuntu/sam-db/ |

### Files on Server
```
/home/ubuntu/sam-db/
    sam.db              ← SQLite database
    server.py           ← Flask + Gunicorn API server (flask-cors enabled)
    setup_db.py         ← Run once to create schema (already done)
    import_health.py    ← Manual CSV importer
```

### Systemd Services
```
/etc/systemd/system/sam-server.service
ExecStart: /home/ubuntu/.local/bin/gunicorn --workers 1 --bind 0.0.0.0:5000 server:app

/etc/systemd/system/cloudflared.service
ExecStart: /home/ubuntu/cloudflared tunnel --url http://localhost:5000 --no-autoupdate
```

### To SSH into server
```powershell
ssh -i "C:\Users\sambl\Documents\ssh-key-2026-05-08.key" ubuntu@164.152.111.208
```

### To restart services
```bash
sudo systemctl restart sam-server
sudo systemctl restart cloudflared
sudo systemctl status sam-server
sudo systemctl status cloudflared
```

### To manually import fresh health data
```bash
scp -i "C:\Users\sambl\Documents\ssh-key-2026-05-08.key" export.csv ubuntu@164.152.111.208:~/sam-db/
cd ~/sam-db && python3 import_health.py export.csv
```

---

## Cloudflare Tunnel

| Field | Value |
|---|---|
| Binary | /home/ubuntu/cloudflared |
| Service | cloudflared.service (systemd, enabled) |
| URL | https://api.samhq.dev (permanent — named tunnel) |
| CORS | flask-cors installed, CORS(app) in server.py |

---

## WorkoutEmailer doPost Routing Logic

```javascript
doPost(e) {
  parse JSON → if (data.type === 'weight') → handleWeightLog() → return early
  else → handleWorkout() → send email
}
```

- Weight POST payload: `{ type: "weight", date: "YYYY-MM-DD", weight: 316.3 }`
- Workout POST payload: `{ day, streak, sets: [{name, totalSets, topSet, pr}] }`
- SHEET_ID stored in Script Properties (not hardcoded)

---

## Google Sheet Schema

**Sheet1 tab** — Calorie data (written by Script 1 daily):
`date | calories | calGoal | protein | proteinGoal | carbs | fat | steps`

**weights tab** — Weight log backup (written by WorkoutEmailer doPost):
`date | weight`
- Fallback only — SQLite is source of truth for weight

---

## New Stack — Python + SQLite (LIVE on Oracle Cloud)

### SQLite Database (sam.db) — Schema

```sql
daily_log:     date (PK), weight_lbs, calories_consumed, calorie_budget,
               protein_g, carbs_g, fat_g, fiber_g, sugar_g, sodium_mg,
               steps, active_calories, resting_calories, walking_miles, updated_at

workouts:      id, date, type, completed, notes, logged_at

lawn_jobs:     id, date, address, service, duration_minutes, amount_charged, paid, notes

project_notes: id, date, project, status, notes, logged_at
```

### Flask Server Endpoints

| Endpoint | Method | Purpose |
|---|---|---|
| `/health` | POST | Receives Health Auto Export JSON, writes to daily_log |
| `/api/today` | GET | Returns today's row from daily_log |
| `/api/week` | GET | Returns last 7 days |
| `/api/weight` | GET | Returns weight history array |
| `/api/weight` | POST | **Pending** — needs `{ date, weight }` → upsert `daily_log.weight_lbs` |
| `/api/stats` | GET | Returns week averages |
| `/ping` | GET | Health check |

> ⚠️ `/api/weight` POST not yet implemented. Dashboard fires it; server may return 405. Sheet backup catches it. Add to Flask: accept `{ date, weight }`, upsert into `daily_log` where `date = ?`.

### Health Auto Export → SQLite Metric Map

| Health Auto Export column | SQLite column |
|---|---|
| `Weight (lbs)` | `weight_lbs` |
| `Dietary Energy (kcal)` | `calories_consumed` |
| `Protein (g)` | `protein_g` |
| `Carbohydrates (g)` | `carbs_g` |
| `Total Fat (g)` | `fat_g` |
| `Step Count (steps)` | `steps` |
| `Active Energy (kcal)` | `active_calories` |
| `Resting Energy (kcal)` | `resting_calories` |
| `Walking + Running Distance (mi)` | `walking_miles` |
| `Date/Time` | `date` (YYYY-MM-DD) |

### Oracle Cloud Networking
- NSG: ig-quick-action-NSG — Ingress TCP 22 + 5000 open
- Ubuntu iptables: port 5000 + 80 open via ACCEPT rules

### Platform Migration Path

```
Stage 1 (NOW):   Flask + SQLite on Oracle Cloud AMD E2.1.Micro — always-on, free
Stage 2 (later): Raspberry Pi CanaKit 4 2GB — physical layer (LEDs/display/GPIO)
```

---

## Dashboard HTML Architecture (v5.5)

### Pages / Nav Tabs

| Tab | ID | Data Source |
|---|---|---|
| Dashboard | page-dashboard | SQLite `/api/today` → Sheet1 CSV fallback |
| Workout | page-workout | localStorage (WORKOUTS state) |
| Body Weight | page-weight | SQLite `/api/weight` → weights tab CSV fallback |
| Wkt History | page-wkt-history | localStorage (samhq_wkt_history) |
| Morning | page-routine | in-memory checklist state |
| Goals | page-goals | localStorage (samhq_goals) |
| Projects | page-projects | GitHub raw URL (sam-vault INDEX.md) |
| Notes | page-notes | localStorage (samhq_notes) |
| Setup | page-setup | — (writes to localStorage) |

### Body Weight Page — Data Flow (v5.5)

```
READ:  fetchWeightSheetData()
         → GET /api/weight (SQLite, primary)
         → filter >= 2026-05-04
         → dedupe by date
         → if 0 results: fallback to WEIGHTS_CSV_URL (Google Sheet)

WRITE: submitWeightLog() / logWeightFromDash()
         → POST /api/weight { date, weight } (primary — may 405 until Flask route added)
         → POST scriptUrl { type: "weight", date, weight } no-cors (Sheet backup, silent)

CHART: renderWeightChart(log)
         → _wtChartMode = 'full' → all entries May4→today
         → _wtChartMode = '2w'  → last 14 days
         → tab toggle via switchWeightChart(mode), log cached in _wtChartLog

HISTORY: renderWeightHistory(log)
         → rolling 30-day window from today
         → newest first, entry count badge (#wt-hist-count)
         → fallback to last 14 entries if window empty
```

### Key JS Constants
```javascript
var WEIGHTS_CSV_URL = 'https://docs.google.com/spreadsheets/d/e/2PACX-1v.../pub?gid=796153409&single=true&output=csv';
var API_URL = 'https://api.samhq.dev'; // permanent
```

### localStorage Keys
| Key | Contents |
|---|---|
| `samhq_sheetUrl` | Sheet1 CSV URL (calories) — set in Setup |
| `samhq_scriptUrl` | WorkoutEmailer Web App URL — set in Setup |
| `samhq_ghUrl` | Obsidian vault INDEX.md raw GitHub URL |
| `samhq_goals` | Strength + habit goal numbers |
| `samhq_streak` | Current workout streak |
| `samhq_wkt_history` | Array of completed workout sessions |
| `samhq_notes` | Array of quick-capture notes with project tags |

### Critical Architecture Rules
- **Single `DOMContentLoaded` listener** — all nav/tab wiring must live inside one block
- **May 4 2026 is the cut start** — all weight UI filters `date >= '2026-05-04'`
- **`_wtChartMode` and `_wtChartLog`** are module-level globals — do not rename

---

## Apps Script Constants

```javascript
const START_WEIGHT = 318;
const GOAL_WEIGHT = 290;
const GOAL_DATE = new Date('2026-09-04');
const START_DATE = new Date('2026-05-04');
const RECIPIENT_EMAIL = 'samblakeley71@gmail.com';
const MODEL = 'claude-haiku-4-5-20251001';
```

---

## Morning / Evening Digest Emails

- Morning: 5:00 AM — Header + Weather, Goal Countdown, Workout of Day, Today's Schedule, Lose It macros, Coach's Take
- Evening: 9:30 PM — Header, Weather, Weight + macros, Coach's Take, Tomorrow Schedule, Night Routine
- Weather: Open-Meteo API, Rochester Hills MI. **Fields: `weather_code`, `wind_speed_10m`**

---

## Workout Schedule (PPL)

| Day | Session |
|---|---|
| Monday | PUSH |
| Wednesday | PULL |
| Friday | LEGS |
| Tue/Thu/Sun | REST or AB circuit |

---

## Weight Loss Goal

| Field | Value |
|---|---|
| Start Date | 2026-05-04 |
| Start Weight | 318 lbs |
| Goal Weight | 290 lbs |
| Deadline | 2026-09-04 |
| Required rate | 1.6 lbs/week |

---

## Bug Fixes Log

| # | Bug | Fix | Version |
|---|---|---|---|
| 1 | Lose It email not found | Label search + global fallback | v4 |
| 2 | CSV parsing wrong | Quoted-field `parseCSVRow()` | v4 |
| 3 | Weight/calGoal = 0 | `getPlainBody()` line-by-line walker | v4 |
| 4 | Dashboard columns shifted | Quoted CSV fix | v4 |
| 5 | calGoal always 2350 | Parser fix; default → 2190 | v4.2 |
| 6 | Weight POST sending workout email | doPost JSON routing; type=weight returns early | v5.2 |
| 7 | `innerHTML` null error on GitHub MD load | Null checks on all `getElementById` | v5.2 |
| 9 | Open-Meteo field name change | Use `weather_code` + `wind_speed_10m` | Digest v3 |
| 10 | import_health.py 0 rows imported | Column map wrong — Health Auto Export uses display names | 2026-05-08 |
| 11 | Mixed Content block (HTTP API from HTTPS page) | Cloudflare tunnel provides HTTPS | 2026-05-08 |
| 12 | CORS block from GitHub Pages | flask-cors installed, CORS(app) added | 2026-05-08 |
| 13 | fetchAndRenderDashWeight missing declaration | Moved setTimeout into init(), added async declaration | 2026-05-08 |
| 14 | sam.db wiped (0 bytes) | DB_PATH hardcoded, setup_db.py rerun, CSV reimported | 2026-05-08 |
| 15 | Body Weight page showing pre-cut data | Filter `date >= '2026-05-04'` in fetchWeightSheetData() both paths | v5.5 |
| 16 | Weight page read from Sheet only (stale) | fetchWeightSheetData() hits /api/weight first, Sheet fallback | v5.5 |
