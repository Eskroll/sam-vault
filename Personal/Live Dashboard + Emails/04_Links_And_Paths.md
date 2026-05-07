---
type: project-links
project: Live Dashboard + Emails
canonical: true
last_updated: "2026-05-07 16:45"
---

# Live Dashboard + Emails — Links & Paths

**Canonical hub:** [[Personal/Live Dashboard + Emails/00_Project_Control]] | [[INDEX]]
**Related:** [[Personal/Live Dashboard + Emails/01_Current_State]] | [[Personal/Live Dashboard + Emails/04_Links_And_Paths]]

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

> Open via Google Apps Script dashboard: https://script.google.com

| Script | Notes |
|---|---|
| Morning Digest | Trigger: 5:00 AM daily |
| Evening Digest | Trigger: 9:30 PM daily |
| Lose It! → Sheet Writer | Trigger: 7:00 AM daily |
| Mersen Shipping Summary | Trigger: 6:30 AM daily |
| Post-Workout Emailer | Standalone script — deploy with **Access: Anyone** |

---

## Google Sheet

- **Published CSV URL:** set in dashboard Setup tab (localStorage key: `sheetUrl`)
- Schema: `date | calories | calGoal | protein | proteinGoal | carbs | fat | weight | steps`

---

## Weather API

- Provider: Open-Meteo (https://open-meteo.com)
- Free, no API key required
- Location: Madison Heights, MI (hardcoded in scripts)

---

## Local Desktop Tools

- **Vault location:** `C:\Users\Owner\OneDrive\Obsidian Vault`
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
