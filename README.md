# Close Risk Dashboard

Self-contained, single-file HTML dashboard for tracking month-end close checklist status.

Open `index.html` directly in any browser — no build step, no server, no external dependencies.

## Features
- Progress (Done / total tasks)
- Blocked task count
- Overdue & unfinished task count
- Total open exceptions
- Blockers section (Task, Owner, Due Date, Reason, Exceptions)
- Full task table with Status and Owner filters
- Sign-off banner that stays LOCKED unless every task is Done and open exceptions = 0
- "Upload CSV" button to load a new checklist locally in the browser (all parsing is client-side; no data leaves the page)

## Data
Ships with sample data from a training close-checklist exercise. Load your own checklist via the Upload CSV button — expected columns: Task, Owner, Due_Date, Status, Open_Exceptions, Last_Updated, Notes (column order and exact naming are flexible).
