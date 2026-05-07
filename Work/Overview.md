---
last_updated: '"2026-05-07 10:45"'
role: Returning intern — Data Reporting & Operations
company: Mersen Corporate Services S.A.S (US branch)
---

# Mersen — Work Overview

**Last updated:** 2026-05-07
**Role:** Returning intern — Data Reporting & Operations
**Company:** Mersen Corporate Services S.A.S (US branch)

---

## Role Context

Sam transitioned from an internship at Ascential Technologies (Burke Porter Group) to a returning role at Mersen around late March 2026. The role was elevated — effectively filling a senior engineering gap — involving data reporting, operations analysis, and AI research deliverables for leadership.

Primary audience for deliverables: Sales, GM, Finance, Operations leadership.

---

## Active Projects

| Project | Status | Notes |
|---|---|---|
| [[Projects/Varian Shipping Report]] | Active / Automation phase | Daily Excel/VBA report with HTML email; exploring full automation |
| [[Projects/Run Cycle Standardization]] | In Progress | Standardize tracking sheets; add atmospheric Wolfspeed recipe tracking |
| [[Projects/PostgreSQL Learning]] | Waiting | Pending DB location confirmation from IT |
| [[Projects/General Work Tasks]] | Active | Catch-all for daily ops tasks |
| [[Projects/AI CNC Research]] | Delivered | Excel + PDF + PowerPoint delivered to leadership |

---

## Environment & Tools

- **Platform:** Microsoft 365 (OneDrive, Outlook, Power Automate available)
- **File drives:** K, M, L drives — accessed via firewall login (network drives)
- **Server infrastructure:** Available for scheduled tasks (IT required)
- **Excel/VBA:** Primary delivery tool
- **Power Query:** Connected to folder for raw data ingestion
- **SAP:** Source system for shipping data (exports via email attachment)
- **SolidWorks + Mastercam:** Used by the CNC programming team (awareness, not direct use)

---

## Key People / Stakeholders

- Direct manager: [name not recorded] — receives and reviews deliverables
- Leadership audience: GM + Sales + Finance + Operations

---

## File Paths (OneDrive)

```
OneDrive - MERSEN Corporate Services S.A.S\
└── Varian Shipping Report\
    ├── Varian_Shipping_Master.xlsm   ← Master workbook
    ├── ShipHistory_Raw\              ← Raw SAP data drops
    ├── Archive\                      ← Older cumulative files moved here
    ├── Outputs\                      ← Generated daily reports
    └── Task Scheduler\
        └── VarianTrigger.ps1         ← PowerShell trigger script
```

---

## Automation Roadmap

**Phase 1 — Power Automate (file ingestion)**
- Trigger: SAP email arrives with Varian ship history attachment
- Action: Auto-save to `ShipHistory_Raw` folder
- Status: Designed, not yet deployed

**Phase 2 — Server scheduled task (macro execution)**
- Trigger: New file in `ShipHistory_Raw` OR time-based (daily AM)
- Action: PowerShell script opens master workbook, runs VBA macro
- Requires: IT to assign scheduled task slot on always-on Windows server
- Status: Script built (`VarianTrigger.ps1`), pending IT deployment

**Fallback:** Dedicated always-on mini PC (~$200) if server access is blocked.
