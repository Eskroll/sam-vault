---
type: session-note
project: Live Dashboard + Emails
area: personal
date: 2026-05-07
status: archived
canonical: false
superseded_by: Personal/Live Dashboard + Emails/00_Project_Control.md
---

# 2026-05-07 — Dashboard v5.1 Build + GitHub Sync Troubleshooting

**Vault hub:** [[Personal/Live Dashboard + Emails/00_Project_Control]] | [[INDEX]]

---

## What Happened This Session

### Dashboard v5.1 Built
Full rebuild from v4.1. Four features added:

1. **Manual body weight log** — "+ Log Weight Manually" button on weight card opens modal with weight input, optional note, auto-timestamp. Stores up to 60 entries in localStorage. Shows "MANUAL LOG" orange tag. Auto-fallback: if Lose It! data has no weight, shows most recent manual entry instead.

2. **Projects page — 3 nested tabs** — Personal / School / Work. Tasks hardcoded from live Obsidian vault data pulled during build session. Personal: Dashboard, Weight Loss, Lawn. School: MGT 1100, ISE 4464, Formula SAE. Work: Varian, Run Cycle, PostgreSQL, General.

3. **Click-to-expand project detail modal** — every project card clickable, opens modal with goal, next action, open tasks, waiting on blockers, completed items, context notes.

4. **Workout email fix** — root cause identified: Script 3 was deployed as "Access: Only myself" — browser fetch() can't authenticate silently, so the POST hit a 302 redirect to Google login and was silently dropped. Fix: redeploy with **Access: Anyone**. Added red warning box to Setup tab. Also added GET fallback in JS.

---

## GitHub Sync Troubleshooting

Sequence of issues resolved:

1. **sam-vault was private** → made public so raw.githubusercontent.com URLs work in browser fetch()
2. **PAT exposed in commit history** — `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` was in `2026-05-07-obsidian-github-setup.md` line 37 → GitHub Push Protection blocked all pushes
3. **Revoked old PAT** → generated new one → updated Windows Credential Manager
4. **Redacted token from vault** — Claude searched all notes, found and replaced the exposed token with `[REDACTED — token revoked]`
5. **Used GitHub unblock URL** to mark secret as revoked and allow push history through
6. **Vault location confirmed:** `C:\Users\Owner\OneDrive\Obsidian Vault` (not Documents)
7. **Remote URL updated via PowerShell** with new PAT embedded
8. **Obsidian Git auto-push** configured: commit-and-sync interval = 5 min

---

## Remaining Before Session Close

- [ ] Deploy v5.1 to GitHub Pages (upload as index.html to Eskroll/sam-hq)
- [ ] Redeploy Script 3 with Access: Anyone, paste new URL into dashboard Setup
