---
type: current-state
project: Mersen - PI Project
area: work
last_updated: 2026-05-13 12:00
---

# Current State — Mersen PI Project

## Active Focus
Mikron LSV2 connection is working and logging live data. Waiting on TOP Server Focas license for Makino side. Deciding on PI vs SQL data destination.

## Mikron Status
- B251-4 (.123) and B251-5 (.122): ✅ Live data polling working
- DNC login: blank password confirmed
- Logger script: `C:\Users\piaf\mikron_log.py` — polls every 30s, writes to `mikron_data.xlsx`
- 17 tags confirmed working including tool number, spindle load, pallet number from PLC STRING memory
- B251-3 (.11) and B251-7 (.9): offline, iTNC530 controllers

## Makino Status
- All 4 primary Makinos online: .103, .104, .116, .128
- TOP Server installed on PINODE but Focas driver license not yet purchased
- Will configure via TOP Server once licensed — port 8193

## Network
- PINODE IP: 10.0.0.27
- Route to 192.168.94.x added but NOT yet persistent — add `-p` flag
- Gateway used: 10.134.0.11

## PI Status
- PI SMT accessible on PINODE
- Makino_Run_State digital set exists but has numbering mismatch vs Focas codes
- PI Web API availability not yet confirmed

## Scripts on PINODE
- `C:\Users\piaf\mikron_log.py` — main logger
- `C:\Users\piaf\full_dump.py` — discovery dump
- `C:\Users\piaf\string_parse_test.py` — PLC string parser test
- `C:\Users\piaf\discover.py` — file listing
