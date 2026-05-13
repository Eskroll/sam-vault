---
type: links-and-paths
project: Mersen - PI Project
area: work
last_updated: 2026-05-12 13:00
---

# Links and Paths — Mersen PI Project

## Network

| Machine | Asset | IP | Port | Protocol |
|---|---|---|---|---|
| Mikron S400U (SP5) | B251-4 | 192.168.94.123 | 19000 | LSV-2 |
| Mikron S600U (SP7) | B251-5 | 192.168.94.122 | 19000 | LSV-2 |
| Mikron S400U (530) | B251-3 | 192.168.94.11 | 19000 | LSV-2 (offline) |
| Mikron S600U (530) | B251-7 | 192.168.94.9 | 19000 | LSV-2 (offline) |
| Makino V56i | B252-2 | 192.168.94.103 | TBD | Focas |
| Makino V56i | B252-1 | 192.168.94.104 | TBD | Focas |
| Makino F5 | B252-7 | 192.168.94.116 | TBD | Focas |
| Makino F5 | B252-9 | 192.168.94.128 | TBD | Focas |
| Okuma U3000 | B341-4 | 192.168.94.124 | TBD | OSP |
| Okuma U3000 | B341-3 | 192.168.94.126 | TBD | OSP |
| Okuma LU35II | B340 | 192.168.94.118 | TBD | OSP |
| Okuma LB4000 | B256-2 | 192.168.94.131 | TBD | OSP |
| Okuma LB4000 | B256-1 | 192.168.94.132 | TBD | OSP |
| PI Server | BCTY-PI | TBD | TBD | PI |
| PI Interface Node | PINODE | TBD | TBD | PI |
| PI server workstation | — | 10.0.0.27 | — | Windows |

## Local File Paths (PI Server — C:\Users\piaf\)
- `tool.t` — Mikron B251-4 tool table (downloaded 2026-05-12)
- `scrdump.png` — Live screen capture from B251-4
- `scrdump_live.png` — Second live capture (confirmed 10:01)
- `AFC.TAB` — Adaptive feed control table
- `tool_p.tch` — Touch probe measurement data

## Machine File System Paths
- TNC: drive → `/mnt/tnc/` on HEROS Linux
- SYS: drive → `/mnt/sys/` on HEROS Linux
- PLC: drive → `/mnt/plc/` on HEROS Linux
- NC programs → `TNC:\nc_prog\`
- Tool table → `TNC:\table\tool.t`
- DNC config → `/mnt/sys/etc/ncrights.cfg` (read-only)

## Key Contacts
- Heidenhain US Support: **847-490-1191**
- Machine SN: **1088** (B251-4)

## Documentation
- pyLSV2 GitHub: https://github.com/drunsinn/pyLSV2
- Heidenhain LSV-2 docs: contact Heidenhain directly
- Makino Focas PDF: in project files (Makino_Focas.pdf)
- PI Digital States screenshot: in project files (PI_Digital_States.png)

## Outputs Created
- `Mikron_S600_Setup_Checklist.docx` — technician guide for DNC + OPC-UA enable
