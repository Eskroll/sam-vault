---
type: project-links
project: Live Dashboard + Emails
canonical: true
last_updated: "2026-05-08 16:45"
---

# Live Dashboard + Emails — Links & Paths

**Canonical hub:** [[Personal/Live Dashboard + Emails/00_Project_Control]] | [[INDEX]]
**Related:** [[Personal/Live Dashboard + Emails/01_Current_State]]

---

## Oracle Cloud Server (LIVE — always-on)

| Field | Value |
|---|---|
| Public IP | 164.152.111.208 |
| Port | 5000 |
| Base URL | http://164.152.111.208:5000 |
| Ping | http://164.152.111.208:5000/ping |
| Today API | http://164.152.111.208:5000/api/today |
| Week API | http://164.152.111.208:5000/api/week |
| Weight API | http://164.152.111.208:5000/api/weight |
| Stats API | http://164.152.111.208:5000/api/stats |
| Health webhook | http://164.152.111.208:5000/health (POST) |

**SSH command:**
```powershell
ssh -i "C:\Users\sambl\Documents\ssh-key-2026-05-08.key" ubuntu@164.152.111.208
```

**SSH key location:** `C:\Users\sambl\Documents\ssh-key-2026-05-08.key`

**Files on server:** `/home/ubuntu/sam-db/`

---

## GitHub Repos

| Repo | URL | Purpose |
|---|---|---|
| sam-hq | https://github.com/Eskroll/sam-hq | Dashboard HTML — deployed via GitHub Pages |
| sam-vault | https://github.com/Eskroll/sam-vault | Obsidian vault sync — branch: `master` |

- **Dashboard live URL:** https://eskroll.github.io/sam-hq/
- **sam-vault must be public** for browser fetch() to work

## Raw GitHub URL Pattern

```
https://raw.githubusercontent.com/Eskroll/sam-vault/master/FOLDER/FILE.md
```
Spaces encoded as `%20`.

**Projects tab base URL:**
```
https://raw.githubusercontent.com/Eskroll/sam-vault/master/Work/Projects/
```

---

## GitHub PAT

- Token: stored in Apps Script project properties (do not paste here)
- Scope: `repo`
- Type: Classic PAT
- **Expiration: ~Aug 2026 — set calendar reminder to renew**

---

## Google Apps Script Links

> Open via: https://script.google.com

| Script | Notes |
|---|---|
| Morning Digest | Trigger: 5:00 AM daily |
| Evening Digest | Trigger: 9:30 PM daily |
| Lose It! → Sheet Writer | Trigger: 7:00 AM daily |
| Mersen Shipping Summary | Trigger: 6:30 AM daily |
| Post-Workout Emailer (WorkoutEmailer) | Deploy with **Access: Anyone** — handles workout email + weight log |

---

## Google Sheet

**Sheet1 (Calorie data):**
- Published CSV URL: set in dashboard Setup tab (localStorage key: `samhq_sheetUrl`)
- Schema: `date | calories | calGoal | protein | proteinGoal | carbs | fat | steps`

**weights tab (Manual weight log):**
- Published CSV URL: **hardcoded in dashboard HTML** as `WEIGHTS_CSV_URL`
- `https://docs.google.com/spreadsheets/d/e/2PACX-1vTCJmlNFbxmBp0Q8rL8I12VwdpVjtQySxrEncSfBuqaeN0gyRf7bhPBeK675wkOtrECLxEmenUzGW5n/pub?gid=796153409&single=true&output=csv`
- Schema: `date | weight`
- All 3 devices read this URL — no per-device Setup needed

---

## Weather API

- Provider: Open-Meteo (https://open-meteo.com)
- Free, no API key required
- Location: Rochester Hills / Madison Heights, MI (coords hardcoded in scripts)
- **Field names (post-API update): `weather_code`, `wind_speed_10m`, `wind_speed_10m_max`**

---

## Local Desktop Tools

- **Vault location:** `C:\Users\Owner\OneDrive\Obsidian Vault`
- **sam-db location:** `C:\Users\sambl\Documents\sam-db\`
- **PowerShell write bridge:** `obsidian-write.ps1` saved to Desktop
- **Local REST API:** `http://127.0.0.1:27123` (Obsidian Local REST API plugin, desktop)
- **ExecutionPolicy:** set to `RemoteSigned`

---

## OneDrive — Mersen Work Files

```
OneDrive - MERSEN Corporate Services S.A.S\
└── Varian Shipping Report\
    ├── Varian_Shipping_Master.xlsm
    ├── ShipHistory_Raw\
    ├── Archive\
    ├── Outputs\
    └── Task Scheduler\
        └── VarianTrigger.ps1
```

---

## Email

- Digest recipient: samblakeley71@gmail.com
