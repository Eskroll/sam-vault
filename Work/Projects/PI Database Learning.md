---
name: PI Database Learning
status: active
goal: Learn the PI system — architecture, data flow, tools, and querying — to
  work with Mersen mill data independently
next_action: Continue working through PI docs and document key concepts in Obsidian
priority: medium
drive_url: null
last_updated: 2026-05-08
---

# PI Database Learning

**Canonical hub:** [[Work/Overview]] | [[Work/Tasks/00 - Task Dashboard]] | [[INDEX]]

## Goal
Understand the PI system end-to-end — from how mill data is collected via MTConnect, through the Data Archive and PI AF, to pulling and querying data via PI DataLink, DBeaver, and SQL.

## Current Status
Active — working through PI documentation with Claude

## Open Tasks
- [ ] Finish PI System Basics doc review #mersen/pi
- [ ] Work through Using PI doc (DataLink, snapshot vs archive) #mersen/pi
- [ ] Work through PI Time doc #mersen/pi
- [ ] Work through PI Admin Basics doc #mersen/pi
- [ ] Practice pulling mill data in PI DataLink #mersen/pi #priority/medium
- [ ] Learn DBeaver + PI OLEDB connection to query PI via SQL #mersen/pi #priority/medium

## Waiting On
- [ ] Confirm OPC Server software in use (Kepware?) with engineer #mersen/pi

## Notes / Context
- Learning in context of Mersen mill data and reporting work
- Servers: PINODE (Interface Node — translator) | BCTY-PI (PI Server — Data Archive + AF)
- Tools available: PI DataLink (Excel), PI Builder (Excel), DBeaver + PI OLEDB/JDBC
- Mill data path: Mills (MTConnect) → PINODE (PI_OPCClient) → BCTY-PI (Data Archive) → PI AF → DataLink / DBeaver
- PI AF sits on SQL Server; DBeaver connects via PI OLEDB/JDBC driver

## Completed
- [x] Mapped full PI data flow — MTConnect → Interface Node → Data Archive → PI AF → client tools
- [x] Identified key tools: PI DataLink, PI Builder, DBeaver, PI_OPCClient
- [x] Identified servers: PINODE and BCTY-PI
