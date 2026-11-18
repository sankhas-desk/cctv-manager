# Changelog

All notable changes to Service Manager. Newest first.

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
