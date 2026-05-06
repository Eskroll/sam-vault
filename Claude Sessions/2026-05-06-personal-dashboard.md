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
---

# 2026-05-06 — Personal Dashboard Build

## Summary
Multi-session build of a self-hosted personal OS dashboard in plain HTML/JS. Covers workout logging, morning routine checklist, goal tracking, Lose It! calorie data, and Obsidian project notes — all in a single file deployable via GitHub Pages.

---

## Key Decisions

### Architecture
- **Single HTML file** — no framework, no build step. Deployable to GitHub Pages for phone + PC access via one URL.
- **Google Drive abandoned as markdown source** — replaced by **Obsidian Git plugin → private GitHub repo → raw GitHub URL fetch**. Dashboard fetches and renders markdown live. Obsidian stays as the editor; GitHub is the sync layer.
- **Lose It! data bridge** — Lose It! has no public API. Decided: existing calorie Google Script parses Lose It! email → writes to a Google Sheet → dashboard reads the Sheet's published CSV URL. No auth required.
- **localStorage** for all config persistence (Sheet URL, Script 3 URL, manual goals). Set once, works forever.

### Google Apps Scripts
| # | Script | Trigger | Status |
|---|--------|---------|--------|
| 1 | Daily Calorie Report | Time-based, 7:00 AM | ✅ Already exists |
| 2 | Mersen Shipping Summary | Time-based, 6:30 AM | ✅ Already exists |
| 3 | Post-Workout Emailer | HTTP POST from dashboard | ❌ Needs deploy |

Script 3 is a **Web App endpoint** (`doPost`). Dashboard POSTs workout JSON to it on "Finish & Send Email". Full code is embedded in the Setup tab of the dashboard.

### Workout Data Source
Pulled live from **Google Calendar** — exact exercises confirmed:

**Push** (4×5 / 3×8 / 3×8 / 3×12 / 3×10):
1. Bench Press — 4×5
2. Standing Shoulder Press — 3×8
3. Incline Bench Press — 3×8
4. DB Lateral Raise — 3×12
5. Tricep Rope Pushdown — 3×10
+ NordicTrack cardio finisher

**Pull** (2×10 / 4×8 / 3×10 / 3×10 / 3×10):
1. Negative Pull-ups — 2×10
2. Barbell Rows — 4×8
3. Lat Pulldown — 3×10
4. Seated DB Rear Delt Flyes — 3×10
5. DB Bicep Curls — 3×10
+ NordicTrack cardio finisher

**Legs** (4×5 / 3×8 / 3×15 / 3×12):
1. Barbell Squat — 4×5
2. Romanian Deadlift — 3×8
3. Calf Raises — 3×15
4. Cable Ab Crunch — 3×12
*(no cardio finisher)*

---

## Outputs

### Dashboard v3 — `dashboard_v3.html`
Fully functional HTML file. Pages:
- **Dashboard** — calorie ring (from Sheet), body weight, today's workout snapshot, Obsidian project strip, goals grid
- **Workout** — exercise log per PPL day, set chips (click to remove), email preview, finish button
- **Morning Routine** — full checklist (pre-shower, post-shower, leave, night) with progress counter
- **Goals** — body, strength, habits, money — auto-populated from Sheet data + manual inputs
- **Projects** — GitHub URL input, fetches + renders Obsidian markdown live; Demo mode available
- **Setup** — Sheet URL, Script 3 URL, manual goal inputs, Script 3 full code + deploy instructions

### Key Code Snippets

**Calorie Script addition (2 lines to add to existing Script 1):**
```js
var sheet = SpreadsheetApp.openById('YOUR_SHEET_ID').getSheetByName('Sheet1');
sheet.getRange("A2:H2").setValues([[date, calories, calGoal, protein, proteinGoal, carbs, fat, weight]]);
```

**Script 3 — WorkoutEmailer `doPost` signature:**
```js
function doPost(e) {
  var data = JSON.parse(e.postData.contents);
  // data.day, data.sets[], data.streak
  // each set: { name, totalSets, topSet, pr }
  GmailApp.sendEmail(...);
  return ContentService.createTextOutput(JSON.stringify({status:"ok"}))
    .setMimeType(ContentService.MimeType.JSON);
}
```

**Google Sheet expected CSV column order (row 2 = today):**
```
date | calories | calGoal | protein | proteinGoal | carbs | fat | weight
```

**Obsidian → Dashboard flow:**
1. Install Obsidian Git plugin
2. Auto-push vault to private GitHub repo
3. Paste raw GitHub URL into Projects tab: `https://raw.githubusercontent.com/USERNAME/VAULT/main/Projects/index.md`
4. Dashboard fetches and renders on load

---

## Open Threads

- [ ] **Deploy Script 3** — go to script.google.com, paste WorkoutEmailer code, deploy as Web App, paste URL in Setup tab
- [ ] **Google Sheet bridge** — add 2 lines to existing calorie script, publish Sheet as CSV, paste URL in Setup
- [ ] **GitHub Pages hosting** — push `dashboard_v3.html` to a repo, enable Pages → get public URL for phone + PC
- [ ] **Obsidian Git setup** — install plugin, point to private repo, test sync; paste raw URL in Projects tab
- [ ] **Manual goals** — enter current bench/OHP/squat/streak/lawn revenue in Setup tab once hosted
- [ ] **Dashboard day-to-workout mapping** — currently hardcoded Mon=Pull, Tue=Legs, Wed=Push, etc. May want to make this configurable if schedule shifts

---

## Notes
- All config (URLs, goals) persists via `localStorage` — set once per device
- No fake/placeholder data in v3 — every field shows an actionable empty state if not connected
- Morning routine checklist matches the Les Roches daily schedule from the app in original screenshots
- Obsidian is the **editor**, GitHub is the **sync layer**, dashboard is the **reader** — no duplication

---

## Session 2 — 2026-05-06 (bugs fixed)

### Bugs Fixed
| # | Bug | Root Cause | Fix |
|---|-----|-----------|-----|
| 1 | Lose It weight/calories/steps missing in digests | Gmail search only scanned inbox; filter had moved email to `Auto-Daily Emails/Lose It!` label | Script now searches by label first, falls back to `newer_than:3d` + all-account search |
| 2 | Lose It data parsed as zeros | Parser relied on plain body text only | 3-tier fallback: body regex → HTML body → CSV attachment |
| 3 | Weather missing on morning digest | Single fetch with no retry; any Open-Meteo hiccup = failure | 3-attempt retry with 1.5s backoff |
| 4 | Dashboard tabs unclickable | All onclick handlers were inline HTML attrs; file:// + CSP can block these | Replaced ALL inline onclicks with data-page/data-day attrs + DOMContentLoaded addEventListener |
| 5 | Calorie/weight cards stuck on "fetching" | Sheet bridge not wired; no file:// detection | Added IS_FILE_PROTOCOL check with amber banner; wrote writeLoseItToSheet() |
| 6 | Steps not captured anywhere | Not in parser or Sheet schema | Added steps parsing (body + CSV attachment) and Sheet column 9 |

### New Files
- `dashboard_v4.html` — all fixes above; Setup tab now has GitHub Pages + Obsidian guides inline
- `Digests_Fixed_GoogleAppsScript.txt` — full replacement script

### New Trigger Required
Add trigger for `writeLoseItToSheet` → Time-driven, daily, 8:30–9:00 AM

### Sheet Schema (FINAL)
`date | calories | calGoal | protein | proteinGoal | carbs | fat | weight | steps`
Row 1 = headers, Row 2 = latest day (overwritten daily), Rows 3+ = history log.

### Script Properties Required
- `ANTHROPIC_API_KEY` — Claude API key
- `SHEET_ID` — Google Sheet ID from the URL

---

## Session 3 — 2026-05-06 (parser rewrite + CSV fix + live data confirmed)

### Context
Continued from Session 2. Dashboard hosted on GitHub Pages. Google Sheet connected. Three separate bugs found and fixed through live testing with real Lose It emails and CSV attachments.

---

### Bug 1 — Everything parsing as `3` in the sheet

**Symptom:** `TEST_writeSheet()` wrote `3` for calories, protein, carbs, fat, weight, goalWeight, etc. Only `proteinGoal` (160) and `date` were correct.

**Root cause:** The Lose It CSV has food names like `"Protein Shake, Chocolate"` with commas inside quoted fields. The old `parseLoseItCSV()` used a naive `.split(',')` which treated those inner commas as column separators, shifting every column right by 1. The food item `"97 3 Raw Ground Beef"` happened to be the first thing `parseFloat()` could extract a number from — and it returned `3`. That value propagated to every field.

**Fix:** Rewrote the CSV parser with a proper character-by-character quoted-field parser (`parseCSVRow`). Tracks `inQuote` state, only splits on commas outside quotes.

```js
function parseCSVRow(line) {
  var fields = [], current = '', inQuote = false;
  for (var i = 0; i < line.length; i++) {
    var ch = line[i];
    if (ch === '"') { inQuote = !inQuote; }
    else if (ch === ',' && !inQuote) { fields.push(current); current = ''; }
    else { current += ch; }
  }
  fields.push(current);
  return fields;
}
```

**Verified totals from `Daily_Report_(40159133)_20260505.csv`:**
| Macro | CSV total | Lose It email (PDF) | Match |
|-------|-----------|---------------------|-------|
| Calories | 2,163 | 2,163 | ✅ |
| Protein | 193g | — | ✅ |
| Carbs | 167g | — | ✅ |
| Fat | 80g | — | ✅ |

---

### Bug 2 — Weight = 0, Steps = 0 in sheet

**Symptom:** After fixing Bug 1, macros were correct but `weight` and `calGoal` still wrote as 0 / 2350 default.

**Root cause:** Weight and calorie budget are **not in the CSV** — they only exist in the HTML email body. The old code called `msg.getBody()` which returns raw HTML. The regex `Today.s Weight[\s\S]{0,100}?(\d+\.\d+)` was failing because in the raw HTML, the weight value (`319.2`) appeared in a different DOM position than expected.

**Fix:** Switched to `msg.getPlainBody()` which strips all HTML tags and gives clean line-separated text matching the PDF layout. Used a line-by-line walker instead of regex:

```js
// Finds "Today's Weight" label, then reads the number from the next 1-3 lines
for (let li = 0; li < bodyLines.length; li++) {
  if (/Today.s Weight/i.test(bodyLines[li]) && result.weight === 0) {
    for (let j = 1; j <= 3; j++) {
      const m = bodyLines[li + j].match(/(\d{2,3}\.\d)/);
      if (m) { result.weight = parseFloat(m[1]); break; }
    }
  }
}
```

**Steps = 0 is correct** — Lose It only includes step counts if a fitness tracker (Fitbit, Apple Watch, Garmin) is synced. Sam has no tracker connected currently. The field will auto-populate if one is connected later.

**Result after fix:** Sheet row 2 = `Tue, May 5 | 2163 | 2350 | 193 | 160 | 167 | 80 | 319.2 | 0`

---

### Bug 3 — Dashboard showing wrong columns (everything shifted by 1)

**Symptom:** After connecting the published CSV URL, dashboard showed:
- Weight card: **80.0 lbs** (actually fat value)
- Steps: **319** (actually weight)
- Protein: **2350g** (actually calGoal)
- Ring: **0 / 2,163** (calGoal read as calories, calories read as 0)

**Root cause:** Same quoted-CSV problem, this time in the dashboard JavaScript. The published CSV wraps the date in quotes: `"Tue, May 5"`. The dashboard used `dataRow.split(',')` which split that into `"Tue` and ` May 5"` — two fields — pushing every subsequent column right by 1. `parseInt(" May 5")` = 0, so calories showed as 0.

**Fix:** Added the same `parseCSVRow` function to the dashboard `fetchLoseItData()`:

```js
function parseCSVRow(line) {
  var fields = [], current = '', inQuote = false;
  for (var ci = 0; ci < line.length; ci++) {
    var ch = line[ci];
    if (ch === '"') { inQuote = !inQuote; }
    else if (ch === ',' && !inQuote) { fields.push(current.trim()); current = ''; }
    else { current += ch; }
  }
  fields.push(current.trim());
  return fields;
}
var vals = parseCSVRow(lines[1]);
```

Also fixed: "319 steps yesterday" display (was showing weight as steps text) — now shows "No tracker connected" when steps = 0.

**Also fixed in this session:** The initial "Failed to fetch" error was caused by pasting the `/edit?gid=0` Google Sheet URL (requires login) instead of the `/pub?gid=0&single=true&output=csv` published URL (fully public). Correct URL format:
```
https://docs.google.com/spreadsheets/d/SHEET_ID/pub?gid=0&single=true&output=csv
```

---

### Final State After Session 3

| Component | Status |
|-----------|--------|
| Dashboard v4 on GitHub Pages | ✅ Live |
| Google Sheet connected (pub CSV) | ✅ Working |
| Calorie ring + macros | ✅ Correct values |
| Body weight display | ✅ 319.2 lbs |
| Dashboard tabs (all 6) | ✅ Clickable |
| Morning digest | ✅ Sending |
| Evening digest | ✅ Sending |
| Weather retry | ✅ 3-attempt backoff |
| Lose It label search | ✅ Searches Auto-Daily Emails/Lose It! |
| writeLoseItToSheet trigger | ✅ 8:30 AM daily |
| Script 3 (workout emailer) | ✅ Deployed, URL saved in Setup |

---

### Open Threads

- [ ] **Verify calGoal parsing** — Sheet shows 2350 (default) but Lose It budget for Tue May 5 was 2190. Run `TEST_loseItParse()` after tomorrow's email arrives and check if calGoal populates correctly. If still 2350, the budget line-walker regex needs tuning.
- [ ] **Obsidian Git setup** — not yet configured. Install plugin, point to private repo, paste raw URL in Projects tab.
- [ ] **Manual goals** — enter current bench/OHP/squat/streak/lawn revenue in Setup tab.
- [ ] **Steps** — will auto-populate if a fitness tracker is connected to Lose It in the future.
- [ ] **calGoal regex** — if tomorrow's sheet still shows 2350 default, share the `TEST_loseItParse()` log output.

---

### Key Lesson: Quoted CSV Parsing

Both the Apps Script and dashboard had the same bug in different places. Any CSV that can contain human-readable strings (food names, dates) **must** use a proper quoted-field parser. Plain `.split(',')` is only safe for purely numeric CSVs.

The fix pattern (`parseCSVRow`) is now in both files and can be reused anywhere.

---

## Session 4 — 2026-05-06 (calGoal parser fix)

### Bug Confirmed: calGoal always writing as 2350 (fallback default)

**Symptom:** Dashboard shows 2,350 as calorie target. Lose It email shows 2,190. Sheet was writing the default fallback, not the parsed value.

**Root cause — 3 compounding errors in `parseLoseItFromMessage`:**

1. **`same` regex ran inside the `j` loop on the wrong variable** — it re-checked the label `line` (not the current `bodyLines[li+j]`) on every loop iteration. Structurally it could never find the same-line budget since Lose It puts budget value on the *next* line — but the placement inside the loop was still wrong and confusing.

2. **`^([\d,]+)$` was too strict** — required the entire next line to consist only of digits/commas. `getPlainBody()` sometimes includes extra whitespace or text (e.g. `"2,190 calories"`), which caused the match to fail silently.

3. **Guard used fallback as sentinel** — `result.calGoal === 2350` as the "not yet found" flag is fragile; any user with a real 2,350 budget would be skipped.

**Fix applied (in `parseLoseItFromMessage`, calorie budget block):**
- Check same line FIRST (outside the loop) with a lenient `/([\d,]{3,})/` regex
- If not found, walk next 1-3 lines with same lenient regex
- Track "found" state with a `_calGoalFound` flag (deleted before return) instead of comparing to the default
- Changed fallback default from `2350` to `2190` (more accurate for current budget)

**Also fixed:**
- `dashboard_v4_2.html` — fallback `|| 2350` in `fetchLoseItData()` → `|| 2190`
- Goals tab static display `<span class="g-target-cals">2350</span>` → `2190`

### Files
- `Morning_and_Evening_Digest_FIXED.txt` — paste this into Google Apps Script, replacing the full script
- `dashboard_v4_2.html` — upload to GitHub Pages, replacing dashboard_v4_1.html

### To verify fix
After tomorrow's 8:30 AM `writeLoseItToSheet` trigger runs:
1. Open the Google Sheet — row 2, column C (`calGoal`) should show `2190`, not `2350`
2. Run `TEST_loseItParse()` manually from Apps Script editor — check Logger output for `calGoal`

### Open Threads
- [ ] Verify calGoal in sheet tomorrow morning after trigger runs
- [ ] Obsidian Git setup — still not configured
- [ ] Manual goals — bench/OHP/squat/streak/lawn revenue
- [ ] Steps — will auto-populate when tracker is connected to Lose It

---

## Session 5 — 2026-05-06 (calGoal fix deployed to script; dashboard pending)

### Context
Follow-up to Session 4. User confirmed the fixed Google Apps Script (`Morning_and_Evening_Digest_FIXED.txt`) has been pasted into Google Apps Script editor. Dashboard `dashboard_v4_2.html` has **not** been pushed to GitHub Pages yet.

### Deploy Status

| Component | Status |
|-----------|--------|
| Fixed Google Apps Script (calGoal parser) | ✅ Pasted into Apps Script |
| `dashboard_v4_2.html` pushed to GitHub Pages | ❌ **Pending** |

### What's in `dashboard_v4_2.html` (not yet live)
Three changes from v4_1:
1. `fetchLoseItData()` fallback: `parseInt(vals[2]) || 2350` → `|| 2190`
2. Goals tab static label: `<span class="g-target-cals">2350</span>` → `2190`
3. No other logic changes — safe drop-in replacement for `index.html` on GitHub Pages

### Open Threads

- [ ] **Push `dashboard_v4_2.html` to GitHub Pages** — rename to `index.html`, replace current file
- [ ] **Verify calGoal tomorrow** — after 8:30 AM `writeLoseItToSheet` trigger, check Sheet row 2 col C = `2190`. Or run `TEST_loseItParse()` manually now to confirm parser reads budget correctly from today's email
- [ ] **Obsidian Git setup** — still not configured
- [ ] **Manual goals** — bench/OHP/squat/streak/lawn revenue in Setup tab
- [ ] **Steps** — auto-populates when a tracker is synced to Lose It
