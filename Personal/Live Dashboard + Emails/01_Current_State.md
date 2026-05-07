---
type: project-state
project: Live Dashboard + Emails
canonical: true
last_updated: "2026-05-07 19:30"
---

# Live Dashboard + Emails — Current State

**Canonical hub:** [[Personal/Live Dashboard + Emails/00_Project_Control]] | [[INDEX]]
**Related:** [[Personal/Live Dashboard + Emails/04_Links_And_Paths]]

---

## Dashboard Versions

| Version | Status | Notes |
|---|---|---|
| v3 | Superseded | Initial build |
| v4 | Superseded | Bug fixes: label search, parser, weather retry, CSV parsing |
| v4.1 | Superseded | Deployed to GitHub Pages |
| v4.2 | Superseded | calGoal parser fix |
| v5.1 | Superseded | Manual weight log, 3-tab projects, workout email fix |
| v5.2 | **Current live** | Weight pipeline, body weight page, workout history page, MD null fix |

---

## Google Apps Scripts

| # | Script | Trigger | Status |
|---|---|---|---|
| 1 | Lose It! → Sheet writer (`writeLoseItToSheet`) | Time-based 7:00 AM | Live |
| 2 | Mersen Shipping Summary | Time-based 6:30 AM | Live |
| 3 | WorkoutEmailer (doPost) | HTTP POST from dashboard | Live — handles both workout email + weight log |
| 4 | Morning Digest | Time-based 5:00 AM | Live |
| 5 | Evening Digest | Time-based 9:30 PM | Live |

---

## WorkoutEmailer doPost Routing Logic

```javascript
doPost(e) {
  parse JSON → if (data.type === 'weight') → handleWeightLog() → return early
  else → handleWorkout() → send email
}
```

- Weight POST payload: `{ type: "weight", date: "YYYY-MM-DD", weight: 318.0 }`
- Workout POST payload: `{ day, streak, sets: [{name, totalSets, topSet, pr}] }`
- SHEET_ID stored in Script Properties (not hardcoded)

---

## Google Sheet Schema

**Sheet name:** Morning Digest Information (or equivalent)

**Sheet1 tab** — Calorie data (written by Script 1 daily):
`date | calories | calGoal | protein | proteinGoal | carbs | fat | steps`
- Row 1 = headers
- Row 2 = latest day (overwritten daily)
- Rows 3+ = history (appended)
- Note: `weight` column removed from Sheet1 — weight now tracked in `weights` tab

**weights tab** — Manual weight log (written by WorkoutEmailer doPost):
`date | weight`
- Row 1 = headers
- Each subsequent row = one logged entry (appended, never overwritten)

---

## Apps Script Constants

```javascript
const START_WEIGHT = 318;
const GOAL_WEIGHT = 290;
const GOAL_DATE = new Date('2026-09-04');
const START_DATE = new Date('2026-05-04');
const RECIPIENT_EMAIL = 'samblakeley71@gmail.com';
const MODEL = 'claude-haiku-4-5-20251001';
const MAX_TOKENS = 8000;
```

---

## Dashboard localStorage Keys

| Key | Contents |
|---|---|
| `samhq_sheetUrl` | Google Sheet published CSV URL |
| `samhq_scriptUrl` | WorkoutEmailer Web App URL |
| `samhq_ghUrl` | Obsidian vault INDEX.md raw GitHub URL |
| `samhq_goals` | Strength + habit goal numbers |
| `samhq_streak` | Current workout streak |
| `samhq_weight_log` | Array of `{date, weight}` — manual weight entries |
| `samhq_wkt_history` | Array of completed workout sessions |

---

## Morning Digest Email

- Send time: 5:00 AM daily
- Sections: Header + Weather, Goal Countdown, Workout of Day, Today's Schedule, Coach's Take
- Weather source: Open-Meteo API (free, no key)

## Evening Digest Email

- Send time: 9:30 PM daily
- Sections: Header, Weather, Lose It Yesterday Summary, Coach's Take, Tomorrow Schedule, Night Routine

---

## Dashboard Projects Tab — GitHub Sync

- Fetches INDEX.md from `https://raw.githubusercontent.com/Eskroll/sam-vault/master/INDEX.md`
- Parses `## Dashboard Links` section for standard markdown links `[name](url)`
- Each link renders as a clickable card in the left panel
- Clicking a card fetches and renders that file's markdown in the right panel
- **sam-vault must be public** for browser fetch() to work

---

## Obsidian Project Note Frontmatter Schema

```yaml
---
name: Project Name
status: in-progress        # in-progress | waiting | completed | paused
goal: One line goal statement
next_action: Single next action
priority: high             # high | medium | low
drive_url:
last_updated: YYYY-MM-DD HH:MM
---
```

---

## Workout Schedule (PPL)

| Day | Session | Exercises |
|---|---|---|
| Monday | PUSH | Bench 4x5, Shoulder Press 3x8, Incline 3x8, Lateral Raise 3x12, Tricep Pushdown 3x10 |
| Wednesday | PULL | Neg Pullups 2x10, Barbell Rows 4x8, Lat Pulldown 3x10, Rear Delt Flyes 3x10, Curls 3x10 |
| Friday | LEGS | Squat 4x5, RDL 3x8, Calf Raises 3x15, Cable Ab Crunch 3x12 |

---

## Weight Loss Goal

| Field | Value |
|---|---|
| Start Date | 2026-05-04 |
| Start Weight | 318 lbs |
| Goal Weight | 290 lbs |
| Deadline | 2026-09-04 |
| Duration | 18 weeks |
| Required rate | 1.6 lbs/week |

---

## Google Calendar (recurring events)

- Wake Up: 5 AM weekdays, 7 AM weekends
- Wind Down: 10:30 PM
- Log Dinner: 10 PM
- SAE Chassis: Mon 7:30 PM
- SAE Team: Sat 10 AM
- Workouts: PUSH Mon, PULL Wed, LEGS Fri at 9 PM

---

## API Cost

- ~$0.02/email at Haiku pricing
- ~$1.20–1.50/month for full stack
- $20 starting balance ≈ 10–11 months runtime

---

## Bug Fixes Log

| # | Bug | Fix | Version |
|---|---|---|---|
| 1 | Lose It email not found | Script searches label first, falls back to global search | v4 |
| 2 | CSV parsing wrong | Proper quoted-field `parseCSVRow()` parser | v4 |
| 3 | Weight/calGoal = 0 | Switched to `getPlainBody()`, line-by-line walker | v4 |
| 4 | Dashboard columns shifted | Quoted CSV fix in `fetchLoseItData()` | v4 |
| 5 | calGoal always 2350 | Fixed parser; default changed to 2190 | v4.2 |
| 6 | Weight POST sending workout email | doPost parsed JSON before routing; type=weight returns early | v5.2 |
| 7 | `innerHTML` null error on GitHub MD load | Null checks on all `getElementById` before `.innerHTML` | v5.2 |
