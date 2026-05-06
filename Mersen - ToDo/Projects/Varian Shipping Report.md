# Varian Shipping Report

## Goal
Fully automate the monthly Varian account shipping digest — from raw cumulative files in a shared folder to a styled HTML email sent to leadership with zero manual steps.

## Current Status
In Progress

## Open Tasks
- [ ] Test Power Automate scheduled flow to trigger the Excel macro automatically #mersen/varian #priority/high
- [ ] Confirm final recipient list with supervisor #mersen/varian #priority/medium
- [ ] Verify credit memo handling is correct across all months #mersen/varian
- [ ] Document the full pipeline for handoff / continuity #mersen/varian #priority/medium

## Waiting On
- [ ] IT approval / access to run Power Automate flows on Mersen M365 tenant #todo/waiting

## Notes / Context
- Power Query pulls from a folder of cumulative monthly files
- VBA macro cleans up duplicate invoice rows caused by multiple cumulative file versions
- Credit memos handled via Promise-Date fallback logic
- Outlook send security prompt resolved; HTML email uses inline CSS for compatibility
- File path for source folder: *(add path here)*
- Power Automate is the identified path for full server-side scheduling

## Completed
- [x] Built initial Power Query folder connection
- [x] Wrote VBA cleanup macro for duplicate invoices
- [x] Implemented styled HTML email digest
- [x] Resolved Outlook send security prompts
- [x] Fixed negative credit memo handling
