---
name: Varian Shipping Report
status: in-progress
goal: Fully automate the monthly Varian account shipping digest with zero manual steps
next_action: Test Power Automate scheduled flow to trigger the Excel macro automatically
priority: high
drive_url:
last_updated: 2026-05-06
---

# Varian Shipping Report

**Canonical hub:** [[Work/Overview]] | [[Work/Tasks/00 - Task Dashboard]] | [[INDEX]]

**Last updated:** 2026-05-06
**Status:** Active — Automation phase
**Owner:** Sam Blakeley (Mersen intern)

---

## Goal
Fully automate the monthly Varian account shipping digest — from raw cumulative files in a shared folder to a styled HTML email sent to leadership with zero manual steps.

## Current Status
In Progress

## Open Tasks
- [ ] Test Power Automate scheduled flow to trigger the Excel macro automatically #mersen/varian #priority/high
- [ ] Confirm final recipient list with supervisor #mersen/varian #priority/medium
- [ ] Verify credit memo handling is correct across all months #mersen/varian
- [ ] Document the full pipeline for handoff / continuity #mersen/varian #priority/medium

## Waiting On
- [ ] IT approval / access to run Power Automate flows on Mersen M365 tenant #todo/waiting

## Completed
- [x] Built initial Power Query folder connection
- [x] Wrote VBA cleanup macro for duplicate invoices
- [x] Implemented styled HTML email digest
- [x] Resolved Outlook send security prompts
- [x] Fixed negative credit memo handling

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
- **Font:** `'Segoe UI', Calibri, Arial, Helvetica, sans-serif` — inline on every element

---

## Key Technical Issues Resolved

### Duplicate invoice counting
**Fix:** Operational rule — keep only the most recent file per month in `ShipHistory_Raw`. Cleanup macro (`CleanShipHistoryAndRefresh`) automates this with a two-pass dictionary approach.

### Negative credit memo values excluded
**Fix:** Promise-Date fallback column lookup. Credit memos now correctly net against positive shipments.

### Outlook send hang
**Fix:** `Application.ScreenUpdating = True` + `DoEvents` before `.Send`.

---

## Automation Plan

### Phase 1 — Power Automate (file ingestion)
- **Trigger:** Incoming SAP email with Varian attachment
- **Action:** Auto-save attachment to `ShipHistory_Raw`
- **Status:** Designed, not yet deployed

### Phase 2 — Server scheduled task (macro execution)
- **Trigger:** Time-based (daily AM)
- **Action:** `VarianTrigger.ps1` opens master workbook, runs VBA macro
- **Status:** Script built, pending IT ticket

---

## Notes / Context
- Power Query pulls from a folder of cumulative monthly files
- VBA macro cleans up duplicate invoice rows
- Credit memos handled via Promise-Date fallback logic
- Outlook send security prompt resolved; HTML email uses inline CSS for Outlook compatibility
