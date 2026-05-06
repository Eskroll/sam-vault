---
date: 2026-05-06
topic: Productivity & Automation System Build
tags: [claude-session, automation, google-apps-script, weight-loss, calendar, email, mersen, SAE]
status: active
---

## Summary

Massive system build session. Went from scratch to a fully running automated productivity stack — Google Calendar restructure, Google Tasks migration, two daily AI-generated emails via Apps Script + Claude API, and a complete personal system across health, work, and academics.

---

## Key Decisions & Outputs

### Google Calendar
- Migrated fully from Outlook Calendar to Google Calendar as single source of truth
- Abandoned Microsoft To Do entirely — everything lives in Google Calendar now
- Recurring events set up: Wake Up (5 AM weekdays, 7 AM weekends), Wind Down (10:30 PM), Log Dinner (10 PM), Weekly Planning Claude Block (Sunday 7 PM)
- SAE Chassis Meeting: Monday 7:30–8:00 PM (Oakland Engineering Center Garage)
- SAE Team Meeting: Saturday 10 AM–12 PM (Oakland Engineering Center Garage)
- Workouts: PUSH Mon, PULL Wed, LEGS Fri at 9 PM — end date May 8 (set manually)
- Classes (PHY 1520, ISE 4487, EGR 2800) and Ascential end date May 8 (set manually)

### Google Tasks
- Built via Google Apps Script (script.google.com)
- Structure: Morning Routine (7 steps), Night Routine (5 steps), Workouts (PUSH/PULL/LEGS with descriptions), Academics, Work & Engineering, Personal & Errands
- Later consolidated: routines moved into Google Calendar descriptions, Tasks kept minimal

### Lose It Integration
- Daily Summary emails arrive from donotreply@loseit.com ~8 AM
- Lose It data is always YESTERDAY's data (one day lag)
- Start weight for new journey: 318 lbs (May 4, 2026)
- Goal weight: 290 lbs by September 4, 2026 (reward = new bag)
- Calorie budget in app: 2,459 cal/day (old app data — real budget recalculated)

### Morning Digest Email (Apps Script)
- Sends 5 AM daily via Google Apps Script trigger
- Calls Claude Haiku 4.5 API
- Sections: Header + Weather, Goal Countdown (Week X of 18), Workout of Day, Today's Schedule, Coach's Take, Today's One Focus
- Lose It section REMOVED from morning (data arrives too late at 8 AM)
- Weather: Open-Meteo API (free, no key needed)
- Emoji strip applied at two levels: `stripNonAscii()` on calendar data + regex in `cleanHtml()`

### Evening Digest Email (Apps Script)
- Sends 9:30 PM daily
- Sections: Header, Weather (Tonight + Tomorrow side by side), Lose It Yesterday Summary, Coach's Take (purple accent), Tomorrow Schedule, Night Routine Checklist
- Tomorrow weather uses Open-Meteo daily forecast endpoint (index 1)
- Purple (#8b5cf6) accent color distinguishes from morning amber

### Apps Script Configuration
- `max_tokens: 8000` (increased from 4000 to fix HTML truncation)
- Model: `claude-haiku-4-5-20251001`
- Nuclear emoji regex strip in `cleanHtml()`
- Two triggers: `sendMorningDigest` (5–6 AM), `sendEveningDigest` (9–10 PM)
- Cost: ~$0.12 for first 5–6 test runs. Projected ~$1.20–1.50/month for full stack

### API & Security
- Claude API key: separate from Claude Pro subscription (console.anthropic.com)
- Pay-as-you-go, ~$0.02 per email at Haiku pricing
- **Security incident**: API key accidentally shared in chat — key was revoked and replaced immediately
- $20 starting balance = ~10–11 months of runtime at current usage

---

## Weight Loss System

| Field | Value |
|---|---|
| Start Date | May 4, 2026 |
| Start Weight | 318 lbs |
| Goal Weight | 290 lbs |
| Deadline | September 4, 2026 |
| Duration | 18 weeks |
| Required rate | 1.6 lbs/week |
| Reward | New bag |

Goal countdown calculated in `getGoalStats()` function — updates automatically each week.

---

## Workout Schedule

| Day | Session |
|---|---|
| Monday | PUSH — Bench 4x5, Shoulder Press 3x8, Incline 3x8, Lateral Raise 3x12, Tricep Pushdown 3x10 |
| Tuesday | Ab Circuit — Cable Ab Crunch 4x12, Hanging Ab Crunch 3x10, Dead Bug 3x10 each |
| Wednesday | PULL — Neg Pullups 2x10, Barbell Rows 4x8, Lat Pulldown 3x10, Rear Delt Flyes 3x10, Curls 3x10 |
| Thursday | Ab Circuit (same as Tuesday) |
| Friday | LEGS — Squat 4x5, RDL 3x8, Calf Raises 3x15, Cable Ab Crunch 3x12, Hanging Ab Crunch 2x8 |
| Sat/Sun | Rest |

---

## Automation Stack Built

| Automation | Trigger | Status |
|---|---|---|
| Morning digest email | 5 AM daily | LIVE |
| Evening digest email | 9:30 PM daily | LIVE |
| Sunday weekly planning (manual) | Sunday 7 PM calendar block | LIVE |
| Weekly weight trend report | Sunday 6:30 AM | Planned |
| Mersen work summary | Friday 4 PM | Planned |
| Lawn care weekly schedule | Sunday 8 AM | Planned |
| Gym progression tracker | Sunday 7 AM | Planned |
| Excel skill of the week | Monday 6 AM | Later |
| ISE concept of the week | Monday 6 AM | Later |
| Finance weekly summary | Sunday | Later |

---

## Key Code — Apps Script Constants

```javascript
const START_WEIGHT = 318;
const GOAL_WEIGHT = 290;
const GOAL_DATE = new Date('2026-09-04');
const START_DATE = new Date('2026-05-04');
const RECIPIENT_EMAIL = 'samblakeley71@gmail.com';
// Model: claude-haiku-4-5-20251001
// max_tokens: 8000
```

---

## Context Notes

- Classes ended first week of May 2026 (PHY 1520, ISE 4487, EGR 2800 all done)
- Ascential internship ending — transitioning to Mersen full time
- Mersen situation: manager's replacement hire (Matt Macinerney) was frozen, Sam asked to fill expanded senior manufacturing/IE role this summer. 4 days on-site, 1 remote.
- Formula SAE chassis team still active — meetings Mon 7:30 PM and Sat 10 AM
- Lawn care side business active — season ramping up
- LDI capstone completed

---

## Open Threads

- [ ] Manually set end dates (May 8) on Ascential, classes, and workout events in Google Calendar — API couldn't set UNTIL dates
- [ ] Formally confirm Mersen expanded role scope, compensation, and start date
- [ ] Build and deploy weekly weight trend report script
- [ ] Build Mersen Friday work summary script
- [ ] Build lawn care client tracker script
- [ ] Build gym progression tracker script (log sets to Google Sheet after each session)
- [ ] Set Gmail filters to route digest emails to Obsidian folders (Claude - MorningDigest, Claude - EveningDigest)
- [ ] Start consistently logging food in Lose It (0 logged for most of tracked period)
- [ ] Sunday planning session at 7 PM weekly

---

## Gmail Filter Notes

- Outlook inbox rules don't support Gmail connected accounts
- Filters must be set at gmail.com → Settings → Filters and Blocked Addresses
- Morning: `from:me subject:"Sam's Morning Digest"`
- Evening: `from:me subject:"Evening Digest"`
- Folders already created in Outlook: Auto-Daily Emails > Claude - MorningDigest / Claude - EveningDigest
- Decision: Apply label WITHOUT skipping inbox, archive manually after reading (one swipe)
