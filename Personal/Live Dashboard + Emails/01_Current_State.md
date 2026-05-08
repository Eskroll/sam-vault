---
type: project-state
project: Live Dashboard + Emails
canonical: true
last_updated: "2026-05-08 11:30"
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

## New Stack — Python + SQLite (In Progress)

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
| `/api/weight` | GET | Returns weight history + 7-day rolling average |
| `/api/stats` | GET | Returns week averages for all metrics |
| `/ping` | GET | Health check |

### Health Auto Export → SQLite Metric Map

| Apple Health metric name | SQLite column |
|---|---|
| `weight_body_mass` | `weight_lbs` |
| `dietary_energy` | `calories_consumed` |
| `protein` | `protein_g` |
| `carbohydrates` | `carbs_g` |
| `total_fat` | `fat_g` |
| `fiber` | `fiber_g` |
| `dietary_sugar` | `sugar_g` |
| `sodium` | `sodium_mg` |
| `step_count` | `steps` |
| `active_energy` | `active_calories` |
| `basal_energy_burned` | `resting_calories` |
| `walking_running_distance` | `walking_miles` |

### Health Export — Data Available (31 days, Apr 7 – May 7 2026)

| Metric | Days with data | Notes |
|---|---|---|
| Steps | 31/31 | Continuous from iPhone |
| Active calories | 31/31 | Continuous |
| Resting calories | 31/31 | Continuous |
| Walking distance | 31/31 | Continuous |
| Weight | 8/31 | Apr 14/16/17/18, May 4/5/6/7 |
| Dietary calories | 5/31 | Only since May 4 — Lose It logging became consistent |
| Protein | 5/31 | Same |
| Carbs, fat, fiber, sugar, sodium | 5/31 | Same |

### Platform Migration Path

```
Stage 1 (now):   Python scripts on Windows PC + Windows Task Scheduler
Stage 2 (summer): Move to Oracle Cloud free tier (always-on, same code)
Stage 3 (later):  Raspberry Pi for physical layer (display + LEDs + GPIO)
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
- Weight: `getLatestWeight()` reads most recent row from `weights` tab. `getGoalStats(currentWeight)` uses real weight, falls back to linear estimate if not logged.

---

## Workout Schedule (PPL)

| Day | Session |
|---|---|
| Monday | PULL |
| Wednesday | LEGS |
| Friday/Saturday | PUSH |
| Tue/Thu/Sun | REST |

*wktMap in JS = {1:PULL, 2:LEGS, 3:PUSH, 4:PULL, 5:LEGS, 6:PUSH, 0:REST}*

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
