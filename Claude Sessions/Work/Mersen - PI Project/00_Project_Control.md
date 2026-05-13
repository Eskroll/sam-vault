---
type: project-control
project: Mersen - PI Project
area: work
last_updated: 2026-05-13 12:00
status: active
---

# Mersen - PI Project — Project Control

**Canonical hub for all PI/mill data integration work at Mersen**

---

## Next Action
→ Wait for TOP Server Fanuc Focas driver license (Devin/process engineer purchasing)
→ Once licensed: configure Focas channel in TOP Server for Makino machines
→ Make persistent route permanent on PINODE: `route add 192.168.94.0 mask 255.255.255.0 10.134.0.11 -p`
→ Decide with Patrick and Devin: does Mikron data go to PI, SQL, or both?
→ Ask Devin about B251-3 (.11) and B251-7 (.9) — are they in service or idle?

---

## Open Threads

### Mikron Mills (LSV2 / pyLSV2) ✅ WORKING
- [x] DNC login confirmed — blank password works (`pyLSV2.Login.DNC, password=''`)
- [x] 807667 does NOT work for DNC login (works for file access only)
- [x] Live data polling working on B251-4 (.123) and B251-5 (.122)
- [x] 17 tags per machine identified and confirmed
- [x] Excel logger script written and tested (`C:\Users\piaf\mikron_log.py`)
- [x] Part number parsed from filename (e.g. 902620 from 902620-009A-R-0.H)
- [x] Tool number from STRING[1] PLC memory
- [x] Spindle load and pallet number from STRING[2] PLC memory
- [ ] B251-3 (.11) and B251-7 (.9) — offline, need to be powered on
- [ ] Decide PI vs SQL vs both for data destination
- [ ] Make route add persistent on PINODE

### Makino Mills (Fanuc Focas)
- [ ] TOP Server Fanuc Focas driver license — being purchased by process engineer
- [ ] All 4 online Makinos confirmed pingable: .103, .104, .116, .128
- [ ] B252-3 (.51) and B252-9 (192.168.81.27) offline
- [ ] Once licensed: add Focas channel in TOP Server, port 8193
- [ ] Build Makino_Run_State digital states in PI SMT (numbering mismatch identified)

### Okuma Mills
- [ ] Already connected via existing PI interface (MTConnect or OSP connector)
- [ ] B256-1, B256-2, B341-3, B341-4, B340 all online
- [ ] B341-1, B341-2 online on 10.134.x.x subnet
- [ ] B331 offline

### PI Integration
- [ ] Confirm PI Web API available: https://localhost/piwebapi on PINODE
- [ ] Decide polling architecture: Python → PI Web API, or Python → flat file → PI interface
- [ ] Build PI tags for Mikron data (17 tags × 4 machines = 68 tags)
- [ ] Build Makino digital state sets in PI SMT once Focas data confirmed

### Network
- [x] Route to 192.168.94.x added on PINODE: `route add 192.168.94.0 mask 255.255.255.0 10.134.0.11`
- [ ] Make route persistent with `-p` flag
- [ ] Machines with no IP yet: B245-1 (Mazak), B252-6, B252-4, B251-2 (Hurco)

---

## Machine Fleet Status

| IP | Asset | Make | Model | Controller | Location | Status |
|---|---|---|---|---|---|---|
| 192.168.94.123 | B251-4 | Mikron | S400U | TNC640 SP5 | Admin | ✅ Online |
| 192.168.94.122 | B251-5 | Mikron | S600U | TNC640 SP7 | Admin | ✅ Online |
| 192.168.94.11 | B251-3 | Mikron | S400U | iTNC530 | Admin | ❌ Offline |
| 192.168.94.9 | B251-7 | Mikron | S600U | iTNC530 | Admin | ❌ Offline |
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

## Mikron Tag List (Per Machine)

| PI Tag | Description | Source | Type |
|---|---|---|---|
| Machine.ExecState | Execution mode | `execution_state()` | Digital |
| Machine.PgmState | Program run state | `program_status()` | Digital |
| Machine.ProgramName | Active NC program | `program_stack().main` | String |
| Machine.PartNumber | Part number from filename | parsed | String |
| Machine.LineNumber | Current line number | `program_stack().line_no` | Integer |
| Machine.ToolNumber | Active tool in spindle | `STRING[1]` | Integer |
| Machine.SpindleLoad | Spindle load % | `STRING[2]` regex | Float |
| Machine.SpindleOverride | Spindle override % | `override_state().spindle` | Float |
| Machine.PalletNumber | Active pallet | `STRING[2]` regex | Integer |
| Machine.FeedOverride | Feed override % | `override_state().feed` | Float |
| Machine.RapidOverride | Rapid override % | `override_state().rapid` | Float |
| Machine.ActiveCycle | Current cycle name | `STRING[24]` | String |
| Machine.X | X axis position | `axes_location()['X']` | Float |
| Machine.Y | Y axis position | `axes_location()['Y']` | Float |
| Machine.Z | Z axis position | `axes_location()['Z']` | Float |
| Machine.B | B axis position | `axes_location()['B']` | Float |
| Machine.C | C axis position | `axes_location()['C']` | Float |

---

## Key Contacts
- Devin Heck (Automation Engineer): devin.heck@i.mersen.com — 989-414-8422
- Patrick Moore: PI/data side
- Alan Tomaso (Mikron/United Machining): alan.tomaso@machining.com — 847-913-5300 x1942
- Joseph Pizzoferrato (Heidenhain TNC Specialist): jpizzoferrato@heidenhain.com — 847-519-4851

---

## Session Notes
- [[Claude Sessions/Work/Mersen - PI Project/2026-05-13 09:00 mikron-dnc-breakthrough-and-fleet-scan.md]]
- [[Claude Sessions/Work/Mersen - PI Project/2026-05-12 13:00 mikron-lsv2-dnc-investigation.md]]
