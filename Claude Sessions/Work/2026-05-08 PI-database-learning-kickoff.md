---
type: session-note
project: PI Database Learning
area: work
date: 2026-05-08
status: archived
canonical: false
superseded_by: Work/Projects/PI Database Learning.md
---

# PI Database Learning — Kickoff Session

**Session date:** 2026-05-08
**Claude project:** Mersen - PI Learning
**Documents reviewed:** PI_System_Basics_PI_System_Components.pdf, Using_PI.pdf (partial), PI_Time.pdf (intro), PI_System_Administration_Basics.pdf (intro)

---

## What Was Covered

### PI System Architecture
- PI collects Plant Information from PLCs, DCS, SCADA, and CNC machines
- Three layers: **Collect** (Interface/Connector/Adapter) → **Manage & Enhance** (PI Server: Data Archive + AF) → **Deliver** (DataLink, PI Vision, DBeaver)
- **PI Data Archive** = the historian; stores raw time-series tag data (proprietary time-series format, not SQL)
- **PI AF (Asset Framework)** = organization layer on top of Data Archive; backed by SQL Server; maps cryptic tag names to human-readable hierarchy (Mill → Machine → Spindle → Speed)
- PI Server = umbrella term for the box running both Data Archive + AF

### Data Flow (Mill Example)
```
Mill equipment (sensors, CNC controller)
  ↓ MTConnect / OPC
PI Interface — translation software, polls machine every 1–5s
  ↓ TCP port 5450
PI Data Archive — historian, stores timestamped PI tags
  ↓
PI AF — organizes tags into asset hierarchy
  ↓
PI DataLink / DBeaver / PI Vision — user retrieval
```

### MTConnect
- Open manufacturing communication standard — HTTP for machine tools
- Adapter (reads CNC registers, translates) + Agent (HTTP XML server) on machine side
- PI Interface polls XML feed, maps elements to PI tags
- Physical connection: wired Ethernet on plant floor network
- Mersen mills confirmed running MTConnect per PI engineer

### Mersen-Specific Servers Identified
- **PINODE** = Interface Node; runs PI_OPCClient; sits on plant floor network; polls mills
- **BCTY-PI** (Bay City PI) = PI Server; runs Data Archive + PI AF; historical data lives here
- Two separate servers confirmed by engineer — correct best-practice isolation architecture

### PI_OPCClient
- Specific PI Interface instance running on PINODE
- Connects to an OPC Server on plant floor network (likely Kepware bridging MTConnect → OPC)
- Admin/troubleshooting tool — not for end-user data retrieval

### Tools
| Tool | Purpose | Direction |
|---|---|---|
| PI DataLink (Excel) | Pull historical + live tag data into spreadsheet | Read |
| PI Builder (Excel) | Create/edit/bulk-manage tag configs | Write (config only) |
| DBeaver + PI OLEDB/JDBC | Query PI via SQL interface | Read |
| PI_OPCClient (PINODE) | Interface collecting mill data — admin/troubleshooting | System |

### Key Concepts
- Not a two-way connection — only PI Interface writes process data to Archive; DataLink/DBeaver are read-only
- Snapshot = live value (~5s); Archive = compressed history (only significant changes stored, PI interpolates between points)
- PI Builder writes tag configuration only, not process data values
- DBeaver: PI OLEDB/JDBC driver makes Archive look like SQL to DBeaver; good for complex cross-tag queries

---

## Open Questions
- [ ] What OPC Server bridges MTConnect → PI_OPCClient? (Kepware likely — confirm with engineer)
- [ ] Do we have PI Vision access for real-time dashboards?
- [ ] What specific mill data points are being collected?

---

## Obsidian Updates This Session
- Renamed PostgreSQL Learning → PI Database Learning (content + frontmatter fully updated)
- Updated: INDEX.md, Work/Overview.md, 00 - Task Dashboard.md, 02 - Active Projects.md, Sam Master Context.md
- Project status: waiting → active

---

## Next Session Starting Point
Continue **Using_PI.pdf** DataLink walkthrough → **PI_Time.pdf** → **PI_System_Administration_Basics.pdf**. Ask engineer about OPC Server software and PI Vision access.
