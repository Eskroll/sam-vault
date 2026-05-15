---
type: links-and-paths
project: Mersen - PI Project
area: work
last_updated: 2026-05-13 16:30
---

# Links and Paths — Mersen PI Project

## Network

| Machine | Asset | IP | Port | Protocol | Status |
|---|---|---|---|---|---|
| Mikron S400U (SP5) | B251-4 | 192.168.94.123 | 19000 | LSV2 | ✅ Working |
| Mikron S600U (SP7) | B251-5 | 192.168.94.122 | 19000 | LSV2 | ✅ Working |
| Mikron S400U (530) | B251-3 | 192.168.94.11 | 19000 | LSV2 | ❌ Physical disconnect |
| Mikron S600U (530) | B251-7 | 192.168.94.9 | 19000 | LSV2 | ❌ Physical disconnect |
| Makino V56i | B252-2 | 192.168.94.103 | 8193 | Focas | ✅ Pingable |
| Makino V56i | B252-1 | 192.168.94.104 | 8193 | Focas | ✅ Pingable |
| Makino V56i | B252-8 | 192.168.94.128 | 8193 | Focas | ✅ Pingable |
| Makino F5 | B252-7 | 192.168.94.116 | 8193 | Focas | ✅ Pingable |
| Makino F5 | B252-3 | 192.168.94.51 | 8193 | Focas | ❌ Offline |
| Makino F5 | B252-9 | 192.168.81.27 | 8193 | Focas | ❌ Offline |
| Okuma LB4000 | B256-2 | 192.168.94.131 | — | OSP | ✅ Online |
| Okuma LB4000 | B256-1 | 192.168.94.132 | — | OSP | ✅ Online |
| Okuma U3000 | B341-4 | 192.168.94.124 | — | OSP | ✅ Online |
| Okuma U3000 | B341-3 | 192.168.94.126 | — | OSP | ✅ Online |
| Okuma LU35II | B340 | 192.168.94.118 | — | OSP | ✅ Online |
| Okuma U3000 | B341-1 | 10.134.2.220 | — | OSP | ✅ Online |
| Okuma U3000 | B341-2 | 10.134.2.136 | — | OSP | ✅ Online |
| Okuma LB35II | B331 | 10.134.2.126 | — | OSP | ❌ Offline |
| PI Server | BCTY-PI | 10.134.0.44 | — | PI | ✅ Active |
| PI Interface Node | PINODE | 10.134.0.29 / 10.0.0.27 | 4840 | OPC-UA + PI | ✅ Active |
| Yokogawa Scrubber Recorder | — | 10.134.0.166 | 502 | Modbus TCP | ✅ Working |
| Yokogawa Scrubber Recorder 2 | — | 192.168.111.230 | 502 | Modbus TCP | ❓ Not yet interrogated |
| Router / Gateway | — | 10.134.0.11 | 22 | SSH | ✅ Active |
| Production Server | productionsvr | 10.134.0.34 | 80 | HTTP | ✅ Active |
| DNS / Domain Controller | baycity-dc2 | 10.134.2.10 | — | AD | ✅ Active |
| Cisco Wireless Controller | — | 10.134.0.70 | — | CAPWAP | ✅ Active |

## Network Config on PINODE
- Primary IP: 10.0.0.27 (subnet 255.255.255.0)
- Also bound: 10.134.0.29, 10.134.5.27, 192.168.22.27, 192.168.111.27
- Route to 192.168.94.x: `route add 192.168.94.0 mask 255.255.255.0 10.134.0.11`
- Make persistent: `route add 192.168.94.0 mask 255.255.255.0 10.134.0.11 -p`
- Gateway / router: 10.134.0.11 (OpenSSH 9.7, Linux-based)
- Domain: baycity.local

## Yokogawa Scrubber Register Map (CONFIRMED)
- IP: 10.134.0.166, Port: 502, Unit ID: 1
- Function code: FC03 (Read Holding Registers)
- Base address: 2, Count: 6, Scale: ×1 (raw = engineering value)
- Reg 2 = H2O Flow - E5 Scrubber (GPM)
- Reg 3 = PH - E4 Scrubber (pH)
- Reg 4 = TEMP - E4 Scrubber (F)
- Reg 5 = H2O Flow - E4 Scrubber (GPM)
- Reg 6 = PH - B Scrubber (pH)
- Reg 7 = TEMP - B Scrubber (F)
- Sentinels: 0xFFFF=not configured, 0x8001=over range, 0x8002=error

## Scripts on PINODE (C:\Users\piaf\)
- `mikron_log.py` — main Mikron logger, outputs to `mikron_data.xlsx`
- `mikron_scanner.py` — network ping sweep + LSV2 fingerprinter
- `yokogawa_scrubber.py` — Yokogawa Modbus reader, master log mode
- `yokogawa_probe.py` — register map discovery tool
- `modbus_test.py` — Modbus FC/address tester
- `modbus_sweep.py` — wide register address sweep
- `reg_debug.py` — low-level register byte debugger
- `yokogawa_master_log.csv` — master scrubber log (appends across sessions)
- `mikron_data.xlsx` — live Mikron logged data output
- `full_dump.py` — full LSV2 capability discovery dump
- `string_parse_test.py` — PLC STRING memory parser test

## DNC Login
- Method: `con.login(login=pyLSV2.Login.DNC, password='')`
- Password: blank (empty string)
- Note: password '807667' from Mikron support works for file access only, NOT DNC login

## Key URLs
- Yokogawa web UI: http://10.134.0.166
- PI Web API (to confirm): https://localhost/piwebapi on PINODE

## Key Contacts
- Devin Heck (Automation Engineer): devin.heck@i.mersen.com — 989-414-8422 / cell 989-439-2938
- Patrick Moore: PI/data side
- Dan Carson (IT / Net Services Group): dcarson@netservicesgroup.com — 989-776-2080 / fax 989-776-2083
- Alan Tomaso (Mikron/United Machining): alan.tomaso@machining.com — 847-913-5300 x1942
- Michael Mitchell Sr (Mikron Advanced Technical Support): michael.mitchellsr@machining.com — 847-913-5300 x7451
- Joseph Pizzoferrato (Heidenhain TNC Specialist): jpizzoferrato@heidenhain.com — 847-519-4851 / 847-284-8350

## Machine Serial Numbers (Mikron)
- S400U: 107.109.00.1051, 107.109.00.1113
- S600U: 107.115.00.1088, 107.115.00.1107, 107.115.00.1108

## Documentation
- pyLSV2 GitHub: https://github.com/drunsinn/pyLSV2
- Heidenhain TNC Machine Data PDF: https://www.inventcom.net/s1/_pdf/Heidenhain_TNC_Machine_Data.pdf
- Fanuc Focas2 download: https://focas.fanuc.co.jp/en/download.html
- Makino Focas PDF: in project files (Makino_Focas.pdf)
- PI Digital States screenshot: in project files (PI_Digital_States.png)
- SRD Process DB Phase 1: in project files (SRD_ProcessDB_Phase1.pdf)
