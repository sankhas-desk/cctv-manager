# Changelog

All notable changes to Service Manager. Newest first.

## [2.2] - 2026-11-23
### Added
- **"Job successfully created" message.** A green confirmation appears at the top of the screen every time the admin posts jobs (it also confirms invoices and reschedules).
- **Admin can reschedule jobs.** A new **📅 Reschedule** button on pending jobs, highlighted on "Cannot access" jobs, opens a box for the new date and an optional reason. The job moves to the new date, the "cannot access" flag is cleared, and the card shows a "📅 Rescheduled from <date>" note.
- **Calendar for admin and workers.** Shown beside the job list on wide screens and as a collapsible panel at the top on phones. Dots mark days with pending (orange), cannot-access (red) and completed (green) jobs. Tapping a day opens it in the main list and lists what was **posted** and what was **completed** that day.
- **Admin can assign workers to a job**, when posting (per job) or when editing. Assigned names show on the job card and in the calendar. Workers see an "Assigned to you" badge, and the report form starts with the assigned workers ticked under "Who attended".
- **Non invoiced jobs section (admin).** Lists every completed job without an invoice, across all dates, oldest first, with how long it has been waiting. **Mark invoiced** opens the invoice box, and the job disappears from the section as soon as it is saved.

### Changed
- The page layout is now two columns on wide screens (jobs on the left, calendar on the right).

### Requires
- No new collections. Admin accounts must be allowed to write the new job fields `assignedTo`, `rescheduledFrom`, `rescheduledAt`, `rescheduledByName` and `rescheduleNote` on `tasks`. Admins can normally update any field already, so check `firestore.rules` and publish only if it limits the fields.

## [2.1] - 2026-11-19
### Added
- **Unfinished jobs carry over.** Any job that is still pending shows up again on every following day (today and future dates) with a "↪ From <date>" badge, and stays until it is done.
- Carried-over jobs are **always at the top** of the list (oldest first) for both workers and admin.
- A carried-over job that is completed today stays visible today as done, so it doesn't vanish when the worker submits it.
- The daily progress summary shows how many jobs are carried over.
- Past dates still show only the jobs posted for that date, so history stays clean. The job keeps its original date (reports and CSV still list it under the day it was posted).

### Changed
- App renamed to **Pentech CCTV Service Manager** (browser title, login heading and header).

## [2.0] - 2026-11-18
### Changed
- **Vehicle mileage is now one shared record per day** (one company vehicle) instead of one per worker. There are no per-worker mileage reports, only the kilometres driven in the month.
- **Workers can only add mileage for today.** Changing the date at the top no longer changes which day they are entering. (The admin can still pick any date to correct a reading.)
- **Start in the morning, end in the evening, never both at once.**
  - Any worker saves the start km; it is then greyed out and locked for everyone, and shows who saved it and when.
  - The end km box opens only after the start is saved. Any worker saves it once, and the day is complete.
  - End km cannot be less than start km.
- CSV report: mileage section is now one row per day (date, start, end, km, who entered each) with month totals.
- Mileage now uses a new Firestore collection, `dailyMileage`. The old per-worker `mileage` collection from 1.8 / 1.9 is no longer read.

### Requires
- Updated `firestore.rules` (new `dailyMileage` rules). **Publish the rules before releasing.**

## [1.9] - 2026-11-18
### Added
- **Cannot access workplace** checkbox on the job report. Workers tick it when a roadblock, locked gate or similar stops them reaching the site, and a text box opens to describe what happened.
- The job stays **pending** (not counted as completed) and is flagged with a red "Cannot access" badge and the reason, who reported it and when.
- The worker can reopen the job later with "Update report" and complete it normally once the site is reachable.
- Admin sees blocked jobs at the top of the list, and the daily summary shows how many jobs cannot be accessed. Admin can also edit or clear the flag from Edit.
- CSV report has three new columns: Cannot Access, Access Issue, Access Reported By.

### Requires
- Updated `firestore.rules` (workers may now update `accessIssue`, `accessNote`, `accessReportedByName`, `accessReportedAt` on a pending job). **Publish the rules before releasing.**

## [1.8] - 2026-11-18
### Added
- Admin must enter the job address when posting a job (also required when editing). Address shows on job cards as a Google Maps link.
- Vehicle mileage section at the bottom of the page: start km / end km per day.
- Monthly kilometre total, with a day-by-day running total.
  - Workers see their own km; admin sees every worker, per-worker totals and the combined total.
- CSV report now has a "Vehicle mileage" section with per-worker and overall totals, plus an Address column in the jobs section.
- Version number shown on the login screen and in the footer.

### Requires
- New Firestore collection `mileage` and matching security rules (see README / rules file).
