---
type: current-state
project: Mersen - PI Project
area: work
last_updated: 2026-05-22 15:45
---

# Current State — Mersen PI Project

## Active Focus
All 4 Mikrons now online and polling. Full-day data capture in progress (10:30am-8pm). JDE scheduling log correlation confirmed working. Next: build analytics dashboard from full-day data, move logging to PostgreSQL, set up Windows Service. Waiting on TOP Server Focas license for Makino side.

## Mikron Status
- ALL 4 MIKRONS ONLINE AND POLLING as of 2026-05-22
- B251-4 (.123): TNC640 SP5 ✅
- B251-5 (.122): TNC640 SP7 ✅
- B251-3 (.11): TNC640 SP3 ✅ (was offline — missing cable, fixed by Dan today)
- B251-7 (.9): TNC640 SP4 (SW17) ✅ (was offline — missing cable, fixed by Dan today)
- KEY FIX: All 4 controllers are TNC640, NOT iTNC530 as previously believed
- KEY FIX: `safe_mode=False` required for SP3/SP4 to connect
- DNC login: blank password + safe_mode=False works on all 4
- Logger script: `C:\Users\piaf\mikron_log.py` — polls all 4 every 30s, writes to `mikron_data.xlsx`
- Route to 192.168.94.x now confirmed persistent (-p flag added)
- Full day data capture running: started 10:30am 2026-05-22, stopping ~8pm
- Part number bridge confirmed: program `901217-002A-R-0` → JDE `B901217-002`
- JDE scheduling log correlation working — all 4 running parts found in scheduling log with late WOs

## Makino Status
- All 4 primary Makinos online: .103, .104, .116, .128
- TOP Server installed on PINODE but Focas driver license not yet purchased
- Making_Run_State digital set exists in PI SMT but integer mapping does not match FOCAS2 16i/18i statinfo_run codes — needs correction before data is archived

## Network — Updated 2026-05-13
- PINODE hostname: PINode.Baycity.local
- PINODE IPs: 10.0.0.27 (primary), 10.134.0.29, 10.134.5.27, 192.168.22.27, 192.168.111.27
- Route to 192.168.94.x added but NOT yet persistent — add `-p` flag
- Gateway / router: 10.134.0.11 (OpenSSH 9.7, Linux-based, MAC 64-62-66-21-4a-5b)
- PI server: bcty-pi.baycity.local at 10.134.0.44
- Production server: productionsvr.baycity.local at 10.134.0.34
- Cisco wireless controller: 10.134.0.70
- DNS server: baycity-dc2.baycity.local at 10.134.2.10
- Domain: baycity.local / Baycity.local

## Yokogawa Scrubber Recorder — NEW 2026-05-13
- IP: 10.134.0.166, Port 502 (Modbus TCP), Unit ID 1
- Web UI: http://10.134.0.166 — live display of all channels
- Protocol confirmed: FC03, registers 2-7, int16, scale ×1
- Live channels confirmed working:
  - Reg 2: H2O Flow - E5 Scrubber (~185 GPM)
  - Reg 3: PH - E4 Scrubber (~9.0 pH)
  - Reg 4: TEMP - E4 Scrubber (~54°F)
  - Reg 5: H2O Flow - E4 Scrubber (~430 GPM)
  - Reg 6: PH - B Scrubber (~9-10 pH)
  - Reg 7: TEMP - B Scrubber (~68°F)
- E5 PH, E5 Temp, B5 Flow showing +Over on web UI — sensors may be disconnected
- Second recorder at 192.168.111.230 — not yet interrogated
- Reader script: `C:\Users\piaf\yokogawa_scrubber.py`
- Master log: `C:\Users\piaf\yokogawa_master_log.csv`

## PI Status
- PI SMT accessible on PINODE
- Making_Run_State digital set exists but has numbering mismatch vs Focas codes
- PI Web API availability not yet confirmed

## Scripts on PINODE (C:\Users\piaf\)
- `mikron_log.py` — main Mikron logger
- `mikron_scanner.py` — network ping sweep + LSV2 fingerprinter
- `yokogawa_scrubber.py` — Yokogawa Modbus reader, master log mode
- `yokogawa_probe.py` — register map discovery tool
- `modbus_test.py` — Modbus FC/address tester
- `modbus_sweep.py` — wide register sweep
- `reg_debug.py` — low-level register byte debugger
- `yokogawa_master_log.csv` — live scrubber data, appends across sessions
- `mikron_data.xlsx` — live Mikron logged data output
