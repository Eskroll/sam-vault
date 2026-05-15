---
type: project-control
project: Mersen - PI Project
area: work
last_updated: 2026-05-13 16:30
status: active
---

# Mersen - PI Project — Project Control

**Canonical hub for all PI/mill data integration work at Mersen**

---

## Next Action
→ Go to shop floor — physically check B251-3 (.11) and B251-7 (.9): cable plugged in? Link light lit? MOD → Network Settings to confirm IP
→ Ask Dan to SSH into router 10.134.0.11 and check ARP table + interface status for .11 and .9
→ Correct Digital State mapping in PI SMT — Making_Run_State integer order does not match FOCAS2 16i/18i statinfo_run codes
→ Decide with Patrick: integrate Yokogawa scrubber data into PI via Modbus TCP (port 502 confirmed open)
→ Make persistent route permanent on PINODE: `route add 192.168.94.0 mask 255.255.255.0 10.134.0.11 -p`
→ Decide with Patrick and Devin: does Mikron data go to PI, SQL, or both?
→ Wait for TOP Server Fanuc Focas driver license (Devin/process engineer purchasing)

---

## Open Threads

### Mikron Mills (LSV2 / pyLSV2) ✅ WORKING
- [x] DNC login confirmed — blank password works
- [x] Live data polling working on B251-4 (.123) and B251-5 (.122)
- [x] 17 tags per machine identified and confirmed
- [x] Excel logger script written and tested (`C:\Users\piaf\mikron_log.py`)
- [x] Network scanner built — `mikron_scanner.py` confirms .122 and .123 reachable on port 19000
- [ ] B251-3 (.11) and B251-7 (.9) — no ping, no TCP, no ARP — physical check required
- [ ] Correct Making_Run_State digital set numbering in PI SMT
- [ ] Decide PI vs SQL vs both for data destination
- [ ] Make route add persistent on PINODE

### Makino Mills (Fanuc Focas)
- [ ] TOP Server Fanuc Focas driver license — being purchased by process engineer
- [ ] All 4 online Makinos confirmed pingable: .103, .104, .116, .128
- [ ] B252-3 (.51) and B252-9 (192.168.81.27) offline
- [ ] Once licensed: add Focas channel in TOP Server, port 8193
- [ ] Build Makino_Run_State digital states in PI SMT (numbering mismatch identified — see session note)

### Okuma Mills
- [ ] Already connected via existing PI interface (MTConnect or OSP connector)
- [ ] B256-1, B256-2, B341-3, B341-4, B340 all online
- [ ] B341-1, B341-2 online on 10.134.x.x subnet
- [ ] B331 offline

### Yokogawa Scrubber Recorder ✅ NEW DISCOVERY 2026-05-13
- [x] Device found at 10.134.0.166 during network scan
- [x] Web UI accessible — live PH, flow, temp for E4, E5, B scrubbers
- [x] Modbus TCP confirmed open on port 502, Unit ID 1
- [x] Register map reverse-engineered: FC03, regs 2-7, scale ×1
- [x] Working reader script: `C:\Users\piaf\yokogawa_scrubber.py`
- [x] Master log file: `C:\Users\piaf\yokogawa_master_log.csv` — appends across sessions
- [x] Second Yokogawa recorder at 192.168.111.230 — not yet interrogated
- [ ] Decide with Patrick: integrate into PI via TOP Server Modbus TCP driver
- [ ] Investigate +Over readings on E5 PH, E5 Temp, B5 Flow channels
- [ ] Interrogate second recorder at 192.168.111.230

### PI Integration
- [ ] Confirm PI Web API available: https://localhost/piwebapi on PINODE
- [ ] Decide polling architecture: Python → PI Web API, or Python → flat file → PI interface
- [ ] Build PI tags for Mikron data (17 tags × 4 machines = 68 tags)
- [ ] Build Makino digital state sets in PI SMT once Focas data confirmed

### Network — NEW FINDINGS 2026-05-13
- [x] Router confirmed at 10.134.0.11 — OpenSSH 9.7, Linux-based, MAC 64-62-66-21-4a-5b
- [x] Full site network map completed — 10.134.0.0/24 and 192.168.111.0/24 scanned
- [x] TTL=63 confirms one router hop between PINODE and machine network
- [x] B251-3 and B251-7 — packets die at router, no ARP entry ever seen — physical problem
- [x] Production server found: productionsvr.baycity.local (10.134.0.34)
- [x] Cisco wireless controller found: cisco-capwap-controller (10.134.0.70)
- [ ] Ask Dan: ARP table on router for .11 and .9; confirm switch port link status
- [ ] Make route persistent with `-p` flag
- [ ] Machines with no IP yet: B245-1 (Mazak), B252-6, B252-4, B251-2 (Hurco)

---

## Machine Fleet Status

| IP | Asset | Make | Model | Controller | Location | Status |
|---|---|---|---|---|---|---|
| 192.168.94.123 | B251-4 | Mikron | S400U | TNC640 SP5 | Admin | ✅ Online |
| 192.168.94.122 | B251-5 | Mikron | S600U | TNC640 SP7 | Admin | ✅ Online |
| 192.168.94.11 | B251-3 | Mikron | S400U | iTNC530 | Admin | ❌ Physical disconnect |
| 192.168.94.9 | B251-7 | Mikron | S600U | iTNC530 | Admin | ❌ Physical disconnect |
| 192.168.94.103 | B252-2 | Makino | V56i | Pro 5 | F5 Area | ✅ Online |
| 192.168.94.104 | B252-1 | Makino | V56i | Pro 6 | F5 Area | ✅ Online |
| 192.168.94.128 | B252-8 | Makino | V56i | Pro 5 | F5 Area | ✅ Online |
| 192.168.94.116 | B252-7 | Makino | F5 | Pro 6 | F5 Area | ✅ Online |
| 192.168.94.51 | B252-3 | Makino | F5 | Pro 6 | F5 Area | ❌ Offline |
| 192.168.81.27 | B252-9 | Makino | F5 | Pro 5 | F5 Area | ❌ Offline |
| 192.168.94.131 | B256-2 | Okuma | LB4000 | OSP-P500 | Admin | ✅ Online |
| 192.168.94.132 | B256-1 | Okuma | LB4000 | OSP-P500 | Admin | ✅ Online |
| 192.168.94.124 | B341-4 | Okuma | U3000 | OSP-P500 | North Shop | ✅ Online |
| 192.168.94.126 | B341-3 | Okuma | U3000 | OSP-P500 | North Shop | ✅ Online |
| 192.168.94.118 | B340 | Okuma | LU35II | OSP-P300 | North Shop | ✅ Online |
| 10.134.2.220 | B341-1 | Okuma | U3000 | OSP-P300 | North Shop | ✅ Online |
| 10.134.2.136 | B341-2 | Okuma | U3000 | OSP-P300 | North Shop | ✅ Online |
| 10.134.2.126 | B331 | Okuma | LB35II | OSP-P200 | North Shop | ❌ Offline |
| — | B245-1 | Mazak | VTC-300C | Mazatrol 640NM | Admin | ❓ No IP |
| — | B252-6 | Makino | F5 | Pro 6 | Admin | ❓ No IP |
| — | B252-4 | Makino | F5 | Pro 5 | Admin | ❓ No IP |
| — | B251-2 | Hurco | BX40ui | Heidenhain ? | Admin | ❓ No IP |

---

## Key Contacts
- Devin Heck (Automation Engineer): devin.heck@i.mersen.com — 989-414-8422
- Patrick Moore: PI/data side
- Dan Carson (IT): dcarson@netservicesgroup.com — 989-776-2080
- Alan Tomaso (Mikron/United Machining): alan.tomaso@machining.com — 847-913-5300 x1942
- Joseph Pizzoferrato (Heidenhain TNC Specialist): jpizzoferrato@heidenhain.com — 847-519-4851

---

## Session Notes
- [[Claude Sessions/Work/Mersen - PI Project/2026-05-13 16:30 network-scan-yokogawa-discovery.md]]
- [[Claude Sessions/Work/Mersen - PI Project/2026-05-13 09:00 mikron-dnc-breakthrough-and-fleet-scan.md]]
- [[Claude Sessions/Work/Mersen - PI Project/2026-05-12 13:00 mikron-lsv2-dnc-investigation.md]]
