---
type: project-control
project: 3 Month Forecast Tool
area: work
status: early-scoping
priority: medium
canonical: true
last_updated: 2026-05-11
next_action: Sync with Jason when he returns to understand his current approach and avoid duplicating effort
context_load_order:
  - 00_Project_Control
  - 04_Links_And_Paths
  - 01_Current_State
archive: false
---

# 3 Month Forecast Tool — Project Control

**Vault hub:** [[INDEX]] | [[Work/Overview]]
**This project:** [[Work/Projects/3 Month Forecast Tool/01_Current_State]] | [[Work/Projects/3 Month Forecast Tool/04_Links_And_Paths]]

> **Canonical source of truth. Read this first.**

---

## What This Project Is

A 3-month manufacturing forecast tool requested by Brian Blakeley. The goal is to move beyond just "what's on the books" (confirmed backlog) and give planning a logically grounded, forward-looking 3-month view — runnable by planning with minimal manual analysis.

Month 1 = Francois's detailed forecast. Months 2 & 3 = logic-based estimates using backlog, prior FC, and budget. Output TBD (Excel, Power BI, or both).

This is a **secondary project** — behind Patrick Moore's and Marty's priorities. Brian is seeking Marty's approval for dedicated time.

---

## Current Status

| Component | Status | Notes |
|---|---|---|
| Project scope | Early scoping | Details still incoming from Brian |
| Jason's version | Unknown | He may have started something — check when back |
| Data sources | TBD | Need to clarify what's available beyond backlog |
| Output format | TBD | Excel, Power BI, or hybrid |
| Time allocation | Pending | Brian seeking Marty approval |

---

## Open Threads

- [ ] **Sync with Jason** — understand his current approach, how far he is, and whether this is parallel or one combined effort #priority/high
- [ ] **Follow up with Brian** — after Jason sync, align on direction and clarify data sources #priority/high
- [ ] **Clarify "beyond the books"** — soft demand? customer projections? engineering/tooling readiness flags?
- [ ] **Confirm data sources** — what systems hold backlog, demand, engineering status today?
- [ ] **Determine output format** — Excel tool, Power BI dashboard, or hybrid?
- [ ] **Get rough timeline** from Brian — any target date or milestone?

---

## Key People

| Name | Role |
|---|---|
| Brian Blakeley | Requestor / sponsor |
| Francois Benis | Owns Month 1 forecast; first pass lead |
| Joe Melzo | Involved in early email thread |
| Jason | May have already started a version — sync when back |
| Marty | Needs to approve time allocation |
| Patrick Moore | Higher-priority project; this is secondary |
| Mike & Christin | Mentioned as possible resource for crawling forward-looking backlogs |

---

## Key Architecture Decisions (TBD — Pending Clarification)

### Forecast Month Logic
- **Month 1:** Francois's forecast — most detailed, grounded in real data
- **Month 2 & 3:** SWAG-level but logically defensible — backlog + prior FC + budget logic

### Business Goals the Tool Must Support
1. Make the Annual Budget
2. Make up prior months' misses (not all in one month)
3. Tend toward aggressive delivery
4. Keep shipments above $5M/month
5. Keep 3rd party deliverables front of mind (Corp priority)

### Exception Flags to Surface (per Brian)
- FAI not completed or required
- Tooling on hand — flag if missing
- Special tooling — flag if missing
- Material on hand
- Complete through Engineering (BOM frozen, Router frozen)
- Other TBD

### Possible Approaches (Rough — Pending More Info)
1. **Simple Excel Tool** — manual M1 input, M2/M3 auto via formulas
2. **Excel + Power BI** — data model in Excel, surfaced in Power BI (Brian mentioned this)
3. **Automated DB Pull + Exception Flagging** — ERP/MRP → Python/SQL → Power BI
4. **Hybrid: Input Template + Logic Engine** — standardized input + estimation logic, runnable by planning

---

## Session Notes (Historical)

Located in `Claude Sessions/Work/`:
- `2026-05-11 — 3mo-forecast-kickoff.md` — Initial project summary from Brian's email chain; drafted clarifying email to Brian
