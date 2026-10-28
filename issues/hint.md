# booking-v3 shows month-first guidance to day-first tenants

33 unique reports from #4471 in the 2026-10-04 export; 3 tenants: copper.studio, alder.physio, pale.harbour. Received 2026-04-17 through 2026-09-22. Deduplicate report IDs; classify by tenant locale + booking-v3 + form channel, not browser locale or intake label.

booking-v3 prints MM/DD/YYYY while the form reader follows a day-first tenant's locale. People following the hint get the wrong date. R-41230 (copper.studio) reports 1 June versus 6 January; R-41241 (alder.physio) reports 8 June versus 6 August. Expected: hint, validation and interpretation agree on the tenant's date format.

Use a common locale-aware contract for the hint and parser. Verify day-first booking-v3 forms with ambiguous dates, invalid dates, and month-first controls. App/email from these same tenants use the parser group, not this group. Fixing the shared parser alone does not fix the conflicting form guidance.

Evidence: src/lib/kb/causes.ts (DATE_HINT and causeOf), locales.ts, tenant records and reports-4471.json. Review: src/routes/kb/incidents/4471-review/+page.svx. Counts are an export snapshot; reconcile future arrivals.
