---
type: project-control
project: Mersen - PI Project
area: work
last_updated: 2026-05-12 13:00
status: active
---

# Mersen - PI Project — Project Control

**Canonical hub for all PI/mill data integration work at Mersen**

---

## Next Action
→ Try machine parameter 7224 on TNC640 terminal to find DNC enable setting
→ If not found: call Heidenhain US support at 847-490-1191 re: DNC on TNC640 SP5/SP7
→ Build screen dump polling script as interim PI data source

---

## Open Threads

### Mikron Mills (LSV-2 / Heidenhain TNC640)
- [ ] Enable DNC login on 192.168.94.123 (SP5) and 192.168.94.122 (SP7)
- [ ] Try machine parameter 7224 in HEROS terminal
- [ ] Contact Heidenhain support if parameter not found (847-490-1191)
- [ ] Confirm who used service key March 2025 — they know the supervisor password
- [ ] Find and power on Mikrons at 192.168.94.11 and 192.168.94.9
- [ ] Build screen dump polling script (grab_screen_dump → OCR → PI tag)
- [ ] Build tool table parser (tool.t → CUR_TIME, LAST_USE → PI tags)
- [ ] Enable OPC-UA on TNC640 (same MOD menu visit as DNC)

### Makino Mills (Fanuc Focas)
- [ ] Begin Focas connection investigation (192.168.94.103, .104, .116, .128 all alive)
- [ ] Confirm Focas port (typically 8193)
- [ ] Install focas Python library

### Okuma Mills
- [ ] Identify Okuma protocol (OSP-P300/P500)
- [ ] Research PI connector options for Okuma OSP

### PI Integration
- [ ] Build PI polling script once DNC enabled
- [ ] Map ExecState/PgmState to PI Digital States
- [ ] Create PI tags for Mikron run state, program name, tool number
- [ ] Create PI tags for tool life (CUR_TIME per tool)

---

## Machine Inventory

| Asset | Make | Model | Controller | IP | LSV-2 | DNC |
|---|---|---|---|---|---|---|
| B251-3 | Mikron | S400U | iTNC 530 | 192.168.94.11 | ❌ Offline | ❌ |
| B251-4 | Mikron | S400U | TNC640 SP5 | 192.168.94.123 | ✅ | ❌ |
| B251-5 | Mikron | S600U | TNC640 SP7 | 192.168.94.122 | ✅ | ❌ |
| B251-7 | Mikron | S600U | iTNC 530 | 192.168.94.9 | ❌ Offline | ❌ |
| B252-2 | Makino | V56i | Pro 5 | 192.168.94.103 | N/A | N/A |
| B252-1 | Makino | V56i | Pro 6 | 192.168.94.104 | N/A | N/A |
| B252-7 | Makino | F5 | Pro 6 | 192.168.94.116 | N/A | N/A |
| B252-9 | Makino | F5 | Pro 5 | 192.168.94.128 | N/A | N/A |
| B341-4 | Okuma | U3000 | OSP-P500 | 192.168.94.124 | N/A | N/A |
| B341-3 | Okuma | U3000 | OSP-P500 | 192.168.94.126 | N/A | N/A |
| B340 | Okuma | LU35II | OSP-P300 | 192.168.94.118 | N/A | N/A |
| B256-2 | Okuma | LB4000 | OSP-P500 | 192.168.94.131 | N/A | N/A |
| B256-1 | Okuma | LB4000 | OSP-P500 | 192.168.94.132 | N/A | N/A |

---

## What Works Right Now (No DNC)
- Live screen dumps from Mikrons (.122 and .123)
- Tool table download (tool.t — 14 tools with runtime hours)
- File system browsing on TNC: drive
- Machine version identification

## What Needs DNC
- execution_state() — run/stop/hold/reset
- program_status() — started/stopped/interrupted
- axes_location() — X/Y/Z positions
- spindle_tool_status() — current tool/speed
- get_error_messages() — alarms

---

## PI Digital State Mapping (Ready to Implement)

| PI State # | Name | PgmState | ExecState |
|---|---|---|---|
| 0 | Reset | CANCELLED | — |
| 1 | Run | STARTED | AUTOMATIC |
| 2 | Stop | STOPPED | — |
| 3 | Hold | INTERRUPTED | — |
| 4 | Finished | FINISHED | — |
| 5 | Jog_MDI | — | MDI |

---

## Key Contacts
- Heidenhain US Support: 847-490-1191
- Machine SN: 1088 (B251-4, 192.168.94.123)
- Service key used March 2025 — find who did it

---

## Session Notes
- [[Claude Sessions/Work/Mersen - PI Project/2026-05-12 13:00 mikron-lsv2-dnc-investigation.md]]
