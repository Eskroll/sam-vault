---
type: current-state
project: Mersen - PI Project
area: work
last_updated: 2026-05-12 13:00
---

# Current State — Mersen PI Project

## Active Focus
Connecting Mikron mills to PI via LSV-2/pyLSV2. DNC login blocked — investigating enable path.

## System Details

### PI Servers
- **PINODE** — Interface Node, plant floor network, runs PI_OPCClient
- **BCTY-PI** — PI Server (Data Archive + PI AF), Bay City

### Test Machine
- Mikron S600 SN-1088, Asset B251-4
- IP: 192.168.94.123, Port: 19000
- Controller: Heidenhain TNC640 / 340590 10 SP5
- HEROS Linux version: 4.9.214-rt137-heros5_64
- pyLSV2 version: 1.5
- Password tested: 807667 (INSPECT works, MONITOR accepts any password)

### Python Environment
- Machine: PI server (Windows 7, 10.0.0.27)
- Python 3.8.10
- pyLSV2 1.5 installed
- opcua 0.98.13 installed
- Working directory: C:\Users\piaf\

### Connection Status
```
LSV-2 Port 19000: LISTENING + ESTABLISHED ✅
INSPECT login: ✅
FILETRANSFER login: ✅  
MONITOR login: ✅ (no password enforcement)
DNC login: ❌ not enabled
PLCDEBUG login: ❌ not available
OPC-UA Port 4840: ❌ refused
```

### File System Paths Found
- TNC: drive → /mnt/tnc/
- SYS: drive → /mnt/sys/
- PLC: drive → /mnt/plc/
- DNC config area: /mnt/sys/etc/ (ncrights.cfg found, read-only)
- Active NC program: TNC:\nc_prog\WARMUP.H (running at session time)

### Downloads Retrieved
- C:\Users\piaf\tool.t — 14 active tools with runtime hours
- C:\Users\piaf\scrdump.png — live screen capture confirmed working
- C:\Users\piaf\AFC.TAB — adaptive feed control table
- C:\Users\piaf\tool_p.tch — touch probe measurement table

## Blockers
1. DNC login not enabled — needs MOD menu supervisor password or Heidenhain support
2. Machines at .11 and .9 offline/unreachable
3. OPC-UA not enabled on any machine
