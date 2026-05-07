---
type: project-state
project: Live Dashboard + Emails
canonical: true
last_updated: '"2026-05-07 11:15"'
---

# Live Dashboard + Emails — Current State

**Canonical hub:** [[Personal/Live Dashboard + Emails/00_Project_Control]] | [[INDEX]]
**Related:** [[Personal/Live Dashboard + Emails/01_Current_State]] | [[Personal/Live Dashboard + Emails/04_Links_And_Paths]]

---

## Dashboard Versions

| Version | Status | Notes |
|---|---|---|
| v3 | Superseded | Initial build |
| v4 | Superseded | Bug fixes: label search, parser, weather retry, CSV parsing |
| v4.1 | Superseded | Deployed to GitHub Pages |
| v4.2 | **Current live** | calGoal parser fix, correct fallback value (2190) |
| v5 | Built, not deployed | Projects redesign, frontmatter fetch, multi-file cards |

---

## Google Apps Scripts

| # | Script | Trigger | Status |
|---|---|---|---|
| 1 | Lose It! → Sheet writer (`writeLoseItToSheet`) | Time-based 7:00 AM | Live |
| 2 | Mersen Shipping Summary | Time-based 6:30 AM | Live |
| 3 | Post-Workout Emailer | HTTP POST from dashboard | Live |
| 4 | Morning Digest | Time-based 5:00 AM | Live |
| 5 | Evening Digest | Time-based 9:30 PM | Live |

---

## Google Sheet Schema

Sheet name: Lose It! Data Bridge
`date | calories | calGoal | protein | proteinGoal | carbs | fat | weight | steps`
- Row 1 = headers
- Row 2 = latest day (overwritten daily by Script 1)
- Rows 3+ = history log (appended)

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

## Morning Digest Email

- Send time: 5:00 AM daily
- Sections: Header + Weather, Goal Countdown, Workout of Day, Today's Schedule, Coach's Take
- Weather source: Open-Meteo API (free, no key)
- Accent color: Blue (standard)

## Evening Digest Email

- Send time: 9:30 PM daily
- Sections: Header, Weather (tonight + tomorrow), Lose It Yesterday Summary, Coach's Take, Tomorrow Schedule, Night Routine Checklist
- Accent color: Purple `#8b5cf6`

---

## Dashboard Projects Tab

- Fetches `.md` files directly by filename from GitHub raw URLs
- Base URL: `https://raw.githubusercontent.com/Eskroll/sam-vault/master/Work/Projects/`
- Files fetched: `Varian%20Shipping%20Report.md`, `PostgreSQL%20Learning.md`, `Run%20Cycle%20Standardization.md`, `General%20Work%20Tasks.md`
- Per-project cards: status pill, goal line, Next Action callout, priority badge, Drive link
- Waiting On panel: aggregates `## Waiting On` checklist items across all notes
- **Note: sam-vault must be public for fetch() to work**

---

## Obsidian Project Note Frontmatter Schema

Applied to all `Work/Projects/` files for dashboard consumption:

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

## Bug Fixes Log (v4 series)

| # | Bug | Fix |
|---|---|---|
| 1 | Lose It email not found | Script searches label first, falls back to global search |
| 2 | CSV parsing wrong (commas in food names) | Proper quoted-field `parseCSVRow()` parser |
| 3 | Weight/calGoal = 0 | Switched to `getPlainBody()`, line-by-line walker |
| 4 | Dashboard columns shifted by 1 | Quoted CSV fix in `fetchLoseItData()` |
| 5 | calGoal always 2350 | Fixed same-line + multi-line parser; default changed to 2190 |
