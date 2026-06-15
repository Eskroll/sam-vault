---
tags:
  - claude
  - meta
last_updated: "2026-05-11"
---
   
# Claude Sessions

**Vault hub:** [[INDEX]]

This folder contains saved summaries from Claude chat sessions. These are **historical records** — they are not current project state. For current project state, read the canonical files in `Personal/` or `Work/`.

---

## Folder Structure

```
Claude Sessions/
  Personal/
    Live Dashboard + Emails/    ← dashboard, automation, productivity sessions
  Work/                         ← Mersen projects, reporting, tools
  School/                       ← course-specific sessions by class
  README.md                     ← this file
```

---

## Naming Convention

```
YYYY-MM-DD HH:MM topic-keyword.md
```

Example: `2026-05-07 10:45 vault-restructure.md`

The time is 24-hour format. This disambiguates multiple sessions on the same day.

---

## When to Read Session Notes

**Do not read these by default.** Claude should read canonical project files (`00_Project_Control.md`, etc.) first.

Read session notes only when:
- Canonical files are missing context Claude needs
- Sam explicitly asks for history
- There is a conflict that needs investigation

---

## End-of-Chat Prompt

> Copy and paste this block at the end of any chat to hand off context or keep a record.

```
Update canonical files for this project: 00_Project_Control.md (open threads, next action, last_updated in YYYY-MM-DD HH:MM). Update 01_Current_State.md if system details changed. Update 04_Links_And_Paths.md if URLs/paths changed.

If session had significant decisions, builds, or changes: save a session note in the matching Claude Sessions/ subfolder. Filename: YYYY-MM-DD HH:MM topic-keyword.md. Frontmatter: type: session-note, project, area, date, status: archived, canonical: false, superseded_by: 00_Project_Control.md. Skip if session was minor.
```

---

## Beginning-of-Chat Prompts

> Pick the block for your task. Copy. Paste at the start of a new Claude chat.

---

### 🟦 Personal

**Dashboard / Automation / Server**
```
Load: INDEX.md → Personal/Live Dashboard + Emails/00_Project_Control.md → 04_Links_And_Paths.md → 01_Current_State.md. Confirm next action in one line. Don't read session notes unless a canonical file is missing context.
```

---

**🟩 School**
```
Load: INDEX.md → School/Summer 2026/COURSE/Syllabus.md → relevant Notes/ or Assignments/ files. Flag upcoming due dates. Confirm task in one line. Don't read session notes unless a canonical file is missing context.
```
*(swap COURSE before pasting)*

---

**⬜ Tasks / General**
```
Load: INDEX.md → Work/Tasks/00 - Task Dashboard.md. Confirm next action in one line. Don't read session notes unless a canonical file is missing context.
```

---

**🔁 Vault / Obsidian**
```
Load: INDEX.md → Claude Sessions/README.md. Confirm next action in one line. Don't read session notes unless a canonical file is missing context.
```

---

### 🟧 Work Projects

**3 Month Forecast Tool**
```
Load: INDEX.md → Work/Overview.md → Work/Projects/3 Month Forecast Tool/00_Project_Control.md → 01_Current_State.md → 04_Links_And_Paths.md. Confirm next action in one line. Don't read session notes unless a canonical file is missing context.
```

---

**AI CNC Research** *(completed — reference only)*
```
Load: INDEX.md → Work/Overview.md → Work/Projects/AI CNC Research.md. Confirm status and any follow-up needed in one line. Don't read session notes unless a canonical file is missing context.
```

---

**General Work Tasks**
```
Load: INDEX.md → Work/Overview.md → Work/Projects/General Work Tasks.md → Work/Tasks/00 - Task Dashboard.md. Confirm next action in one line. Don't read session notes unless a canonical file is missing context.
```

---

**PI Database Learning**
```
Load: INDEX.md → Work/Overview.md → Work/Projects/PI Database Learning.md. Confirm next action in one line. Don't read session notes unless a canonical file is missing context.
```

---

**Mersen - PI Project** *(machine monitoring — Mikron, Makino, Okuma)*
```
Load from Obsidian: INDEX.md - ClaudeSessions - Work - Mersen - PI Project, remember where we were at, then I will dump some new info
```
> Canonical files: `Claude Sessions/Work/Mersen - PI Project/00_Project_Control.md` → `01_Current_State.md` → `04_Links_And_Paths.md`

---

**Run Cycle Standardization**
```
Load: INDEX.md → Work/Overview.md → Work/Projects/Run Cycle Standardization.md. Confirm next action in one line. Don't read session notes unless a canonical file is missing context.
```

---

**Varian Shipping Report** *(archived — live and complete)*
```
Load: INDEX.md → Work/Archive/Varian Shipping Report.md. Confirm status in one line. Don't read session notes unless a canonical file is missing context.
```


---

### 🟨 Career

**Job Search 2027** *(internship repository — Summer 2027 target)*
```
Load from Obsidian: INDEX.md → Career/Job Search 2027/00_Project_Control.md → Career/Job Search 2027/Listings Repository.md. Confirm listing count and next action in one line. Don't read session notes unless a canonical file is missing context.
```
> Canonical files: `Career/Job Search 2027/00_Project_Control.md` → `Listings Repository.md` → `Target Companies.md`
>
> **Context:** Sam is a Junior ISE at Oakland University targeting Summer 2027 internships at Fortune 500 / brand-name companies in ops, IE, or rotational roles. Geography: Michigan + OH/IN/IL/WI for summer; 30-mile radius of OU or remote during school year. Goal path: High-Output Operator → COO (Fortune 500) or Industrial PE Partner. Do NOT suggest returning to Mersen. Priority industries: semiconductors, advanced mfg, CPG, aerospace. Avoid defense. Application season opens Aug 15, 2026 — Fortune 500 rotational cycles post Aug–Nov. Repository target: 100 listings.

---

**COO Roadmap** *(certs, MBA, career timeline — long-term path)*
```
Load from Obsidian: INDEX.md → Career/COO Roadmap/00_Project_Control.md → Career/COO Roadmap/01_Cert_Roadmap.md → Career/COO Roadmap/02_MBA_Roadmap.md → Career/COO Roadmap/03_Career_Timeline.md. Confirm current phase and next action in one line. Don't read session notes unless a canonical file is missing context.
```
> Canonical files: `Career/COO Roadmap/00_Project_Control.md` → `01_Cert_Roadmap.md` → `02_MBA_Roadmap.md` → `03_Career_Timeline.md`
>
> **Context:** Sam is targeting Fortune 500 COO via the Enterprise Operator track (ISE → Fortune 500 rotational → LSS Black Belt → Kellogg/Ross/Booth MBA → plant manager → VP Ops → COO). Wealth endgame is PE Operating Partner / carried interest. Cert priority: Green Belt now → ASQ CSSBB Black Belt age 24–25 → APICS CPIM age 25–27 → Top-10 MBA 2030–2032 → PMP age 26–28. Mersen $500K+ project pre-qualifies as ASQ CSSBB project documentation. GMAT target 720+, begin Target Test Prep Oct 2027. Avg COO appointment age ~46; 79% internal appointments.
