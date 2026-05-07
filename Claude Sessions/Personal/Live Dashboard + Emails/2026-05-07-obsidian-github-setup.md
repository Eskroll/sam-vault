---
project: Personal - Live Dashboard + Emails
date: 2026-05-07
session: obsidian-github-desktop-setup
tags:
  - claude-session
  - dashboard
  - obsidian
  - setup
canonical: "false"
superseded_by: '"Personal/Live Dashboard + Emails/04_Links_And_Paths.md"'
---

# Session: Obsidian + GitHub Setup on Desktop

> **Historical record.** Current state lives in [[Personal/Live Dashboard + Emails/00_Project_Control]] and [[INDEX]].
**Date:** 2026-05-07
**Project:** Personal - Live Dashboard + Emails

## What We Did

### Vault Structure Redesign
- Audited current Obsidian vault state — flat Claude Sessions dump, split Mersen folders
- Designed new folder structure with Claude Sessions broken out by domain (Personal/, Work/, School/)
- Created unified Work/ folder replacing Mersen/ and Mersen - ToDo/
- Executed full vault reorganization on 2026-05-07

### Desktop Obsidian Setup
- Installed Obsidian on desktop PC — opened existing vault from OneDrive
- Confirmed Git installed and working (git --version returned version number)
- Connected Obsidian Git plugin to Eskroll/sam-vault on branch master
- Fixed remote URL with PAT token authentication
- Set ExecutionPolicy to RemoteSigned to allow PowerShell scripts
- Tested Local REST API write via PowerShell script — SUCCESS

### GitHub API Write Bridge
- Confirmed PAT token: [REDACTED — token revoked] (expires ~Aug 2026)
- Built PowerShell obsidian-write.ps1 script saved to Desktop
- 
- Obsidian MCP connector available on laptop for direct writes

## New Vault Structure (implemented 2026-05-07)
```
Work/
  Overview.md
  Tasks/  (00 Dashboard, 01 Inbox, 02 Active, 03 Waiting, 04 Completed)
  Projects/
    Varian Shipping Report.md
    Run Cycle Standardization.md
    PostgreSQL Learning.md
    General Work Tasks.md
    AI CNC Research.md
  Archive/
Claude Sessions/
  Personal/
  Work/
  School/
  README.md
School/
  Summer 2026/
    ISE 4464/ (Syllabus, Notes/, Assignments/)
    MGT 1100/ (Syllabus, Notes/, Assignments/)
  Archive/
Templates/  (unchanged)
```

## Open Threads
- [ ] Delete old Mersen/ and Mersen - ToDo/ folders (content migrated to Work/)
- [ ] Delete old flat Claude Session files from Claude Sessions/ root (moved to subfolders)
- [ ] Set auto commit-and-sync interval to 5 minutes on desktop Obsidian Git
- [ ] Update dashboard v5 Projects tab URL to point at `Work/Projects/` (was `Mersen - ToDo/Projects/`)
- [ ] Make sam-vault repo public for browser fetch() to work in dashboard
- [ ] Calendar reminder to renew PAT before Aug 2026
