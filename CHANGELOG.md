# Changelog

All notable changes to Service Manager. Newest first.

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
