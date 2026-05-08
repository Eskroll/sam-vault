---
type: project-state
project: Live Dashboard + Emails
canonical: true
last_updated: "2026-05-08 16:45"
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
| Digest v3 | **Current live** | Weather API field names fixed; weight from `weights` tab |
| v5.4 | **Current live** | Rest timers, body measurements widget, Notes tab, DOMContentLoaded bug fix |

---

## Google Apps Scripts

| # | Script | Trigger | Status |
|---|---|---|---|
| 1 | Lose It! → Sheet writer (`writeLoseItToSheet`) | Time-based 7:00 AM | Live |
| 2 | Mersen Shipping Summary | Time-based 6:30 AM | Live |
| 3 | WorkoutEmailer (doPost) | HTTP POST from dashboard | Live — handles workout email + weight log |
| 4 | Morning Digest | Time-based 5:00 AM | Live — **v3** (weather fix + weight from Sheet) |
| 5 | Evening Digest | Time-based 9:30 PM | Live — **v3** (weather fix + weight from Sheet) |

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
    sam.db          ← SQLite database, 38 days health data
    server.py       ← Flask + Gunicorn API server
    setup_db.py     ← Run once to create schema (already done)
    import_health.py ← Manual CSV importer
```

### Systemd Service
```
/etc/systemd/system/sam-server.service
ExecStart: /home/ubuntu/.local/bin/gunicorn --workers 1 --bind 0.0.0.0:5000 server:app
```

### To SSH into server
```powershell
ssh -i "C:\Users\sambl\Documents\ssh-key-2026-05-08.key" ubuntu@164.152.111.208
```

### To manually import fresh health data
```bash
# On PC: export CSV from Health Auto Export app, transfer to server
scp -i "C:\Users\sambl\Documents\ssh-key-2026-05-08.key" export.csv ubuntu@164.152.111.208:~/sam-db/
# On server:
cd ~/sam-db && python3 import_health.py export.csv
```

### To restart the service
```bash
sudo systemctl restart sam-server
sudo systemctl status sam-server
```

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
- Row 1 = headers, Row 2 = latest day, Rows 3+ = history
- Note: `weight` column removed from Sheet1 — weight tracked in `weights` tab only

**weights tab** — Manual weight log (written by WorkoutEmailer doPost):
`date | weight`
- Row 1 = headers
- Each entry appended — never overwritten
- Published as separate CSV — URL hardcoded in dashboard HTML

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

### Current Data (as of 2026-05-08 import)

| Metric | Days with data | Notes |
|---|---|---|
| Steps | 38/38 | Continuous from iPhone |
| Active calories | 38/38 | Continuous |
| Resting calories | 38/38 | Continuous |
| Walking distance | 38/38 | Continuous |
| Weight | ~8 days | Apr 14/16/17/18, May 4/5/6/7/8 |
| Dietary calories | ~5 days | Since May 4 — Lose It logging consistent |
| Protein/Carbs/Fat | ~5 days | Same |

### Flask Server Endpoints

| Endpoint | Method | Purpose |
|---|---|---|
| `/health` | POST | Receives Health Auto Export JSON, writes to daily_log |
| `/api/today` | GET | Returns today's row from daily_log |
| `/api/week` | GET | Returns last 7 days |
| `/api/weight` | GET | Returns weight history |
| `/api/stats` | GET | Returns week averages |
| `/ping` | GET | Health check |

### Health Auto Export → SQLite Metric Map (actual CSV column names)

| Health Auto Export column | SQLite column |
|---|---|
| `Weight (lbs)` | `weight_lbs` |
| `Dietary Energy (kcal)` | `calories_consumed` |
| `Protein (g)` | `protein_g` |
| `Carbohydrates (g)` | `carbs_g` |
| `Total Fat (g)` | `fat_g` |
| `Fiber (g)` | `fiber_g` |
| `Sugar (g)` | `sugar_g` |
| `Sodium (mg)` | `sodium_mg` |
| `Step Count (steps)` | `steps` |
| `Active Energy (kcal)` | `active_calories` |
| `Resting Energy (kcal)` | `resting_calories` |
| `Walking + Running Distance (mi)` | `walking_miles` |
| `Date/Time` | `date` (YYYY-MM-DD) |

### Oracle Cloud Networking
- VCN: sam-vcn (10.0.0.0/16)
- Subnet: sam-subnet (10.0.0.0/24)
- NSG: ig-quick-action-NSG — Ingress TCP 22 + 5000 open
- Security List: Default — Ingress TCP 22 + 5000 open
- Internet Gateway: attached via quick-connect action
- Ubuntu iptables: port 5000 + 80 open via ACCEPT rules

### Platform Migration Path

```
Stage 1 (NOW):   Flask + SQLite on Oracle Cloud AMD E2.1.Micro — always-on, free
Stage 2 (later): Raspberry Pi CanaKit 4 2GB — physical layer (LEDs/display/GPIO)
```

---

## Dashboard HTML Architecture (v5.4)

### Pages / Nav Tabs
| Tab | ID | Data Source |
|---|---|---|
| Dashboard | page-dashboard | Sheet1 CSV (calories) + weights CSV (weight card) |
| Workout | page-workout | localStorage (WORKOUTS state) |
| Body Weight | page-weight | weights tab CSV (hardcoded URL) |
| Wkt History | page-wkt-history | localStorage (samhq_wkt_history) |
| Morning | page-routine | in-memory checklist state |
| Goals | page-goals | localStorage (samhq_goals) |
| Projects | page-projects | GitHub raw URL (sam-vault INDEX.md) |
| Notes | page-notes | localStorage (samhq_notes) |
| Setup | page-setup | — (writes to localStorage) |

### Key JS Constants
```javascript
var WEIGHTS_CSV_URL = 'https://docs.google.com/spreadsheets/d/e/2PACX-1v.../pub?gid=796153409&single=true&output=csv';
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

### Critical Architecture Rule
**Single `DOMContentLoaded` listener.** All nav link wiring, tab wiring, notes filter wiring must live inside one `document.addEventListener('DOMContentLoaded', ...)` block.

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
- Weight: currently from Google Sheet `weights` tab — **to be migrated to SQLite API**

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
| 10 | import_health.py 0 rows imported | Column map was wrong — Health Auto Export uses display names not snake_case | 2026-05-08 |
