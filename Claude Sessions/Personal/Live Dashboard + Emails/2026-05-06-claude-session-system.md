---
date: '"2026-05-06 00:00"'
topic: Claude Session System Setup
tags:
  - claude
  - obsidian
  - productivity
canonical: "false"
superseded_by: '"Personal/Live Dashboard + Emails/00_Project_Control.md"'
---

# Claude Session System Setup

> **Historical record.** Current state lives in [[Personal/Live Dashboard + Emails/00_Project_Control]] and [[INDEX]].

## Summary
Set up a reusable system for saving Claude chat sessions as markdown notes in Obsidian. Goal is a low-token memory layer for carrying context across conversations without re-explaining things from scratch.

## Key Decisions
- Folder: `Claude Sessions/` in Obsidian vault, organized by domain (Personal/, Work/, School/)
- Naming: `YYYY-MM-DD Topic-Keyword.md`
- Retrieval: tell Claude to "pull my session note on [topic]" at the start of a new chat
- Obsidian chosen over Google Drive for vault-native integration

## Save Prompt
> Save this chat as a Claude session note in Obsidian under `Claude Sessions/`. Use today's date and a short topic keyword for the filename. Include: date, topic, key decisions or outputs, any important code or data, and open threads. Format it cleanly with frontmatter tags.

## Open Threads
- Set up Obsidian sync to phone if phone access becomes a priority
