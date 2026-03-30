# ScheduleBot — Design Review Optimizer

A single-file web application that detects scheduling conflicts across design review meetings and automatically optimizes the schedule to eliminate double-bookings of key decision makers.

## Problem

When coordinating 20+ design reviews across multiple days, key decision makers and facilitators frequently get double-booked. Manually resolving these conflicts — canceling meetings, notifying attendees, finding new time slots — is tedious and error-prone.

ScheduleBot automates this entire workflow.

## How It Works

1. **Upload** your Outlook calendar export (CSV) or a formatted Excel file
2. **Tag** key decision makers and set their priority (Critical / High / Medium)
3. **Mark** must-happen meetings that get first pick of time slots
4. **Download** an optimized schedule with a full conflict report and change log

Everything runs client-side in the browser. No server, no backend, no data leaves your machine.

## Features

- **Auto-format detection** — Recognizes Outlook CSV exports and structured Excel files automatically
- **Conflict detection** — Finds every instance where a key person is double-booked across overlapping meetings
- **Greedy constraint-based optimizer** — Runs 30 randomized iterations to find the best conflict-free arrangement
- **Must-happen meetings** — Pin critical meetings so they get scheduled first in the best slots
- **Room preference** — In-person meetings preferred; virtual meetings restricted to 8–11 AM
- **Manager report** — Downloadable Excel with unresolved conflicts, recommended actions, rescheduled meetings, and key people workload summary
- **7-sheet Excel output** — Optimized Schedule, Change Log, Conflict Reports (Before/After), Person Schedules, Manager Report, Key People

## Scheduling Rules

| Rule | Detail |
|------|--------|
| Work hours | 8:00 AM – 5:00 PM |
| Lunch break | 12:00 – 1:00 PM (blocked) |
| Slot granularity | 30 minutes |
| Day pinning | Meetings stay on their original day |
| Virtual meetings | Scheduled between 8:00 – 11:00 AM only |
| Room preference | Physical rooms preferred over virtual |
| Priority order | Must-happen > Critical attendees > High > Medium |

## Input Formats

### Option A: Outlook Calendar Export (recommended)

Export directly from Outlook:

**File** → **Open & Export** → **Import/Export** → **Export to a file** → **Comma Separated Values** → select your Calendar → choose date range.

Upload the CSV. ScheduleBot auto-detects the Outlook format and presents a UI to tag key people.

Expected columns: `Subject`, `Start Date`, `Start Time`, `End Date`, `End Time`, `All Day Event`, `Location`, `Meeting Organizer`, `Required Attendees`, `Optional Attendees`, `Show As`

### Option B: Formatted Excel (two sheets)

**Sheet 1 — "Key People"**

| Name | Role | Priority |
|------|------|----------|
| Sarah Chen | VP Design | Critical |
| Marcus Johnson | Lead Architect | High |

**Sheet 2 — "Meetings"**

| Meeting ID | Meeting Name | Date | Start Time | End Time | Room / Link | Organizer | Required Attendees | Optional Attendees | Meeting Size | Status |
|------------|-------------|------|------------|----------|-------------|-----------|-------------------|-------------------|-------------|--------|
| M001 | Homepage Redesign Review | 2026-03-25 | 9:00 AM | 9:45 AM | Room A - Horizon | Aisha Williams | Sarah Chen, Marcus Johnson | David Kim | 5 | Scheduled |

Attendee names can be comma-separated or semicolon-separated.

## Output Excel Sheets

| Sheet | Contents |
|-------|----------|
| Optimized Schedule | Full meeting list with new times, rooms, and change flags |
| Change Log | What moved: old time → new time, old room → new room |
| Conflict Report (Before) | All conflicts in the original schedule |
| Conflict Report (After) | Remaining conflicts after optimization (ideally zero) |
| Person Schedules | Each key person's daily agenda with conflict flags |
| Manager Report | Summary stats, unresolved conflicts with recommended actions, rescheduled meetings, workload summary |
| Key People | Preserved input list of tracked people |


### Local

Open `schedulebot.html` directly in any modern browser.

## Tech Stack

- Vanilla HTML/CSS/JS — no frameworks
- [SheetJS (xlsx.js)](https://sheetjs.com/) via CDN — client-side Excel/CSV reading and writing
- DM Sans + JetBrains Mono — typography via Google Fonts

## License

MIT

