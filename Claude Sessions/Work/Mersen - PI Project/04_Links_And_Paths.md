---
type: links-and-paths
project: Mersen - PI Project
area: work
last_updated: 2026-05-13 12:00
---

# Links and Paths — Mersen PI Project

## Network

| Machine | Asset | IP | Port | Protocol | Status |
|---|---|---|---|---|---|
| Mikron S400U (SP5) | B251-4 | 192.168.94.123 | 19000 | LSV2 | ✅ Working |
| Mikron S600U (SP7) | B251-5 | 192.168.94.122 | 19000 | LSV2 | ✅ Working |
| Mikron S400U (530) | B251-3 | 192.168.94.11 | 19000 | LSV2 | ❌ Offline |
| Mikron S600U (530) | B251-7 | 192.168.94.9 | 19000 | LSV2 | ❌ Offline |
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
| PI Server | BCTY-PI | — | — | PI | — |
| PI Interface Node | PINODE | 10.0.0.27 | — | PI | ✅ Active |

## Network Config on PINODE
- Route to 192.168.94.x: `route add 192.168.94.0 mask 255.255.255.0 10.134.0.11`
- Make persistent: `route add 192.168.94.0 mask 255.255.255.0 10.134.0.11 -p`
- Gateway: 10.134.0.11

## Scripts on PINODE (C:\Users\piaf\)
- `mikron_log.py` — main polling logger, outputs to `mikron_data.xlsx`
- `full_dump.py` — full LSV2 capability discovery dump
- `string_parse_test.py` — PLC STRING memory parser test
- `discover.py` — nc_prog directory listing
- `extra_dump.py` — data path and scope signal tests
- `plc_dump.py` — PLC memory type discovery
- `mikron_data.xlsx` — live logged data output
- `full_dump_output.txt` — LSV2 capability dump output

## DNC Login
- Method: `con.login(login=pyLSV2.Login.DNC, password='')`
- Password: blank (empty string)
- Note: password '807667' from Mikron support works for file access only, NOT DNC login

## Key Contacts
- Devin Heck (Automation Engineer): devin.heck@i.mersen.com — 989-414-8422 / cell 989-439-2938
- Patrick Moore: PI/data side
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
- DNC email chain: in project files (MikronDNCControl.pdf)
