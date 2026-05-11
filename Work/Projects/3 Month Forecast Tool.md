---
name: 3 Month Forecast Tool
status: early-scoping
goal: Build a 3-month manufacturing forecast tool that goes beyond confirmed backlog to give planning a forward-looking view
next_action: Sync with Jason (when back) to understand his current approach and avoid duplicate efforts
priority: medium
drive_url:
last_updated: 2026-05-11
---

# 3 Month Forecast Tool

**Canonical hub:** [[Work/Overview]] | [[Work/Tasks/00 - Task Dashboard]] | [[INDEX]]

**Last updated:** 2026-05-11
**Status:** Early Scoping — awaiting clarification from Brian and Jason
**Owner:** Sam Blakeley (Mersen intern)
**Requested by:** Brian Blakeley
**Secondary to:** Patrick Moore's and Marty's priorities

---

## Goal
Build a 3-month manufacturing forecast tool that goes beyond confirmed backlog. The tool should pull in forward-looking signals, flag exceptions early, and give planning a logically grounded directional view — runnable by the planning team with minimal manual analysis.

---

## Background
Brian initiated this project via email on 5/1/2026. He noted that Jason may have already started something, but his concern is that Jason's version is only looking at "what's on the books" (confirmed backlog), which won't be sufficient. The tool needs to capture more than just open orders.

Brian mentioned getting Marty's approval before full time can be dedicated to this.

---

## Forecast Structure

| Month | Owner / Basis |
|---|---|
| Month 1 (M) | Francois's forecast — most detailed, grounded in real data |
| Month 2 (M+1) | Logic-based estimate from backlog + prior FC + budget |
| Month 3 (M+2) | Logic-based estimate from backlog + prior FC + budget |

Months 2 & 3 are SWAG-level but should be logically defensible, not just a guess.

---

## Business Objectives the Tool Must Support
- [ ] Hit the Annual Budget
- [ ] Make up prior month misses (spread out, not all in one month)
- [ ] Tend toward aggressive delivery targets
- [ ] Keep shipments above $5M/month
- [ ] Keep 3rd party deliverables front of mind (Corp's main focus)

---

## Exception Flags to Capture (per Brian)
- [ ] FAI not completed or required
- [ ] Tooling on hand — flagging if missing
- [ ] Special tooling — flagging if missing
- [ ] Material on hand
- [ ] Complete through Engineering (BOM frozen, Router frozen)
- [ ] Other TBD items

---

## Open Questions
- [ ] What is Jason building, and how far is he? Parallel effort or something to build on?
- [ ] What does "beyond what's on the books" mean in practice — soft demand, customer projections, engineering readiness?
- [ ] What data sources exist beyond the formal backlog (ERP, spreadsheets, etc.)?
- [ ] Is Power BI already licensed and in use, or would that require setup?
- [ ] Is there a target delivery date or milestone Brian has in mind?
- [ ] How is data currently shared between Francois and the planning team?

---

## Open Tasks
- [ ] Sync with Jason when he returns to understand his current approach #mersen/forecast #priority/high
- [ ] Follow up with Brian after Jason sync to align on direction #mersen/forecast #priority/high
- [ ] Clarify what data sources are available beyond confirmed backlog #mersen/forecast
- [ ] Define Month 2 & 3 estimation logic once inputs are confirmed #mersen/forecast
- [ ] Determine output format (Excel, Power BI, or both) #mersen/forecast

## Waiting On
- [ ] Jason to return and share what he has built so far #todo/waiting
- [ ] Brian's "more to follow" context on data sources and requirements #todo/waiting
- [ ] Marty approval for dedicated time on this project #todo/waiting

## Completed
- [x] Read initial email chain from Brian (5/1/2026 and 5/11/2026)
- [x] Drafted and sent clarifying email to Brian re: data sources and Jason coordination

---

## Possible Approaches (Rough — Pending More Info)

### Option 1 — Simple Excel Tool
Planners input Month 1 manually; Months 2 & 3 auto-populate via formulas using backlog and budget logic. Low-tech, fast to build, planning team can own it immediately.

### Option 2 — Excel + Power BI Dashboard
Build the data model in Excel/a database, surface output in Power BI. Exception flags visible at a glance. Brian specifically mentioned Power BI as a possibility.

### Option 3 — Automated Database Pull + Exception Flagging
Pull from ERP/MRP systems via Python or SQL, auto-flag exceptions (FAI, tooling, materials, engineering status), feed results into Power BI or Excel. More ambitious but most aligned with what Brian described.

### Option 4 — Hybrid: Input Template + Logic Engine
Standardized input template + a logic layer (Excel or script) that applies M2/M3 estimation rules, reconciles against budget, and highlights gaps. Runnable by planning without IT involvement each time.

---

## Key People

| Name | Role |
|---|---|
| Brian Blakeley | Requestor / sponsor |
| Francois Benis | Owns Month 1 forecast; first pass lead |
| Joe Melzo | Involved in early email thread |
| Jason | May have started a version of this already — check when back |
| Marty | Needs to approve time allocation |
| Patrick Moore | Higher-priority project; this is secondary to his work |
| Mike & Christin | Mentioned as possible resource for crawling forward-looking backlogs |

---

## Notes / Context
- Brian's core concern: looking more than a month out is hard because there's no consolidated forward view
- The tool should move forecasting responsibility from Brian to Francois + planning team over time
- Power BI was mentioned as a possible output — "it will be ok in the end"
- This may require a multi-database draw to pull all needed signals
- As of 5/11/2026, details are still incoming — this note will be updated as more info arrives
