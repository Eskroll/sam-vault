---
date: 2026-05-06
topic: personal-dashboard
tags:
  - claude-session
  - dashboard
  - html
  - google-apps-script
  - obsidian
  - workout
  - lose-it
status: live-partial
canonical: "false"
superseded_by: '"Personal/Live Dashboard + Emails/00_Project_Control.md"'
---

# 2026-05-06 — Personal Dashboard Build

> **Historical record.** Current state lives in [[Personal/Live Dashboard + Emails/00_Project_Control]] and [[INDEX]].

## Summary
Multi-session build of a self-hosted personal OS dashboard in plain HTML/JS. Covers workout logging, morning routine checklist, goal tracking, Lose It! calorie data, and Obsidian project notes — all in a single file deployable via GitHub Pages.

---

## Key Decisions

### Architecture
- **Single HTML file** — no framework, no build step. Deployable to GitHub Pages for phone + PC access via one URL.
- **Lose It! data bridge** — Google Script parses Lose It! email → writes to Google Sheet → dashboard reads Sheet's published CSV URL.
- **localStorage** for all config persistence (Sheet URL, Script 3 URL, manual goals). Set once, works forever.

### Google Apps Scripts
| # | Script | Trigger | Status |
|---|--------|---------|--------|
| 1 | Daily Calorie Report | Time-based, 7:00 AM | ✅ Already exists |
| 2 | Mersen Shipping Summary | Time-based, 6:30 AM | ✅ Already exists |
| 3 | Post-Workout Emailer | HTTP POST from dashboard | ✅ Deployed |

### Weight Loss System
| Field | Value |
|---|---|
| Start Date | May 4, 2026 |
| Start Weight | 318 lbs |
| Goal Weight | 290 lbs |
| Deadline | September 4, 2026 |
| Duration | 18 weeks |
| Required rate | 1.6 lbs/week |

### Workout Schedule (PPL)
| Day | Session |
|---|---|
| Monday | PUSH — Bench 4x5, Shoulder Press 3x8, Incline 3x8, Lateral Raise 3x12, Tricep Pushdown 3x10 |
| Wednesday | PULL — Neg Pullups 2x10, Barbell Rows 4x8, Lat Pulldown 3x10, Rear Delt Flyes 3x10, Curls 3x10 |
| Friday | LEGS — Squat 4x5, RDL 3x8, Calf Raises 3x15, Cable Ab Crunch 3x12 |

### Key Apps Script Constants
```javascript
const START_WEIGHT = 318;
const GOAL_WEIGHT = 290;
const GOAL_DATE = new Date('2026-09-04');
const START_DATE = new Date('2026-05-04');
const RECIPIENT_EMAIL = 'samblakeley71@gmail.com';
// Model: claude-haiku-4-5-20251001
// max_tokens: 8000
```

### Google Sheet Schema (FINAL)
`date | calories | calGoal | protein | proteinGoal | carbs | fat | weight | steps`
Row 1 = headers, Row 2 = latest day (overwritten daily), Rows 3+ = history log.

---

## Dashboard Versions

| Version | Status | Key Changes |
|---|---|---|
| v3 | Superseded | Initial build — workout log, morning routine, goals, projects |
| v4 | Superseded | Bug fixes: label search, parser, weather retry, CSV parsing |
| v4.1 | Superseded | Deployed to GitHub Pages |
| v4.2 | Current | calGoal parser fix, correct fallback value |

---

## Bug Fixes Log (Sessions 2–4)

| # | Bug | Fix |
|---|-----|-----|
| 1 | Lose It email not found | Script now searches label first, falls back to global search |
| 2 | CSV parsing wrong (commas in food names) | Proper quoted-field `parseCSVRow()` parser in both script and dashboard |
| 3 | Weight/calGoal = 0 | Switched to `getPlainBody()`, line-by-line walker instead of regex |
| 4 | Dashboard columns shifted by 1 | Same quoted CSV fix in `fetchLoseItData()` |
| 5 | calGoal always 2350 | Fixed same-line + multi-line parser; changed default to 2190 |

---

## Open Threads
- [ ] Push `dashboard_v4_2.html` to GitHub Pages as `index.html`
- [ ] Verify calGoal in sheet after next 8:30 AM writeLoseItToSheet trigger
- [ ] Manual goals — bench/OHP/squat/streak/lawn revenue in Setup tab
- [ ] Steps — will auto-populate when fitness tracker is connected to Lose It
