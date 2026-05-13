# Oakland Email → Calendar Workflow

**Purpose:** Scan Oakland Outlook inbox for assignment deadlines and push them to Google Calendar via Claude.
**When to run:** Every Sunday during Weekly Planning Claude Block. Update sender list each semester.

---

## Step 1 — Paste this prompt into Claude in Chrome sidebar

> Open your Oakland Outlook tab in Edge, paste this into the Claude sidebar, let it run, then copy the full output and paste it into claude.ai.

```
Go to my Oakland Outlook inbox tab. Search for emails from these senders only: zemden@oakland.edu, moodle+noreply@oakland.edu, pandey2@oakland.edu. Open only the 5 most recent emails from each sender. Do not click any links or navigate anywhere else. Do not open Moodle. Extract any assignment names, due dates, and course names. The timezone is EDT (Eastern Daylight Time, UTC-4). When done, list only upcoming deadlines after today's date in this exact format:

- [Course ID]: [Assignment Name] due [Day, Month Date, Year] at [Time] EDT

Do not include anything already past today's date. Do not include notes, assumptions, or commentary. Output the deadline list only, then on a new line say exactly: Claude — check my Google Calendar for the next 60 days, skip anything already on there, and create events for the remaining deadlines. All 11:59 PM deadlines should be created as 11:58 PM to 11:59 PM on the due date in America/Detroit timezone with a 24 hour popup reminder and a 1 hour popup reminder.
```

---

## Step 2 — Copy the full output and paste into claude.ai

Claude will check your calendar for duplicates and only create net new events.

---

## Sender List (update each semester)

| Sender | Course | Notes |
|---|---|---|
| zemden@oakland.edu | MGT1100 | Prof. Zeynep Emden — announcements |
| moodle+noreply@oakland.edu | All | Automated Moodle reminders |
| pandey2@oakland.edu | ISE 4464/5464 | Prof. Vijitashwa Pandey |

---

## Notes
- Limit is 5 most recent emails per sender to control token usage
- Do not open Moodle directly — read from emails only
- Events are created as 11:58–11:59 PM to avoid midnight overflow
- Reminders: 24 hour popup + 1 hour popup
- Claude deduplicates against existing calendar events before creating anything
