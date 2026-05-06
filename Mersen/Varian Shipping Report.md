# Varian Shipping Report

**Last updated:** 2026-05-05
**Status:** Active — Automation phase
**Owner:** Sam Blakeley (Mersen intern)

---

## Overview

Automated Excel-based daily shipping report for Mersen's Varian account. Generates a formatted Excel report and a styled HTML email digest, sent to leadership (Sales, GM, Finance, Operations) each morning. Goal: fully hands-off, no manual intervention required.

---

## Architecture

| Component | Detail |
|---|---|
| Master workbook | `Varian_Shipping_Master.xlsm` |
| Raw data folder | `ShipHistory_Raw` — receives SAP export `.xlsx` files |
| Archive folder | `Archive` — older cumulative files moved here by cleanup macro |
| Output folder | `Outputs` — generated daily Excel reports |
| Trigger script | `Task Scheduler\VarianTrigger.ps1` — PowerShell, runs every 15 min 6–10 AM |
| Data connection | Power Query folder connection to `ShipHistory_Raw` |

---

## Report Structure — 4 Sheets

1. Piece Part Summary
2. Revenue Summary
3. Piece Part Avg Rate
4. Revenue Avg Rate

**Scope:** 2025 and 2026 data
**Buckets tracked:** Monthly, 5th BD, 10th BD, 15th BD, 20th BD

---

## HTML Email Design

- **Header:** Dark blue `#1F4E79` branded banner
- **Scorecard (3 columns):** Yesterday / This Week / Month to Date (revenue + piece parts)
- **Progress bar:** MTD revenue vs. $900,000 monthly goal
- **Pace indicator:** Ahead / On Track / Slightly Behind / Behind Pace (color-coded)
- **Business days:** Elapsed vs. total for the month
- **Footer:** References attached Excel file
- **Font:** `'Segoe UI', Calibri, Arial, Helvetica, sans-serif` — applied inline on every element (prevents Outlook fallback to Times New Roman)
- **Width:** 700px, font sizes ~20% larger than original
- **Greeting:** "Team" (not individual name)

Config constants defined at module level: `MONTHLY_GOAL`, `REPORT_TO`, `OUTPUT_FOLDER`

---

## Key Technical Issues Resolved

### 1. Duplicate invoice counting
**Problem:** Multiple cumulative monthly SAP files in `ShipHistory_Raw` all loaded via Power Query folder connection → inflated revenue/quantity figures.
**Fix:** Operational rule — keep only the most recent file per month in `ShipHistory_Raw`, move older versions to `Archive`. Cleanup macro (`CleanShipHistoryAndRefresh`) automates this.

**Cleanup macro logic:** Two-pass dictionary approach — parses dates from filenames (`Varian-Ship-History_MM-DD-YY.xlsx`), identifies the latest file per month, moves all others to `Archive`.

**Bug found:** `Array()` returns 0-based indexing; `ReDim arr(1 To 3)` returns 1-based. Fixed by standardizing indexing.

### 2. Negative credit memo values excluded
**Problem:** Credit memos had no Ship-Date → excluded from aggregation → revenue overstated.
**Fix:** Promise-Date fallback column lookup. Credit memos now correctly net against positive shipments (e.g., -$17,585.68 netted to accurate $326,075.15 for April).

### 3. Outlook send hang (spinning wheel)
**Problem:** Outlook programmatic send security prompt hidden behind Excel.
**Fix:** `Application.ScreenUpdating = True` + `DoEvents` before `.Send`.

### 4. Array indexing bug in cleanup macro
**Problem:** `Array()` is 0-based; `ReDim arr(1 To 3)` is 1-based — mismatch caused runtime error.
**Fix:** Standardized indexing throughout the module. Confirmed correct via diagnostic module (all 15 files parsed correctly).

---

## Automation Plan

### Phase 1 — Power Automate (file ingestion)
- **Trigger:** Incoming SAP email with Varian attachment
- **Action:** Auto-save attachment to `ShipHistory_Raw`
- **Cost:** Free (Mersen has M365)
- **Status:** Designed, not yet deployed

### Phase 2 — Server scheduled task (macro execution)
- **Trigger:** Time-based (daily AM) or file-drop event
- **Action:** `VarianTrigger.ps1` opens master workbook, runs VBA macro
- **Requires:** IT to set up scheduled task on always-on Windows server
- **Status:** Script built, pending IT ticket

**Fallback if server blocked:** Dedicated always-on mini PC (~$200 refurbished).

---

## SAP Data Flow

1. SAP generates ship history export → sent via email to Sam
2. Outlook rule moves email to `Varian Shipping Reports` folder
3. Attachment saved to `ShipHistory_Raw` (manual currently; Power Automate in Phase 1)
4. Cleanup macro runs → keeps only latest file per month
5. Power Query refreshes → pulls clean data
6. VBA aggregates → generates 4-sheet Excel report
7. HTML email sent to leadership with scorecard + report attached
