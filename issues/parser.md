# Shared parser interprets day-first dates month-first

379 unique reports from #4471 in the 2026-10-04 export; 11 tenants. Received 2026-04-01 through 2026-10-04. Counts deduplicate report IDs and use tenant locale + form + channel via causes.ts, not intake labels or browser locale.

For day-first tenants, the shared parser reads month first on app/email and booking-v2 form paths. R-41200: 7 April reminder becomes 4 July; R-41203: 2 December becomes 12 February. Expected: input follows the tenant/input contract and the resulting invite preserves the intended date.

Fix the parser contract; validate ambiguous/invalid dates. Verify day-first and month-first locales, app/email/form paths, and booking-v2. Test independently of the booking-v3 hint issue.

Five reports (R-41202, R-41209, R-41236, R-41344, R-41620) describe one-day or timing anomalies but remain in this tenant-based group. Obtain original input/timestamps/timezones before claiming a separate cause. Classification is not proof of each individual narrative.

Evidence: src/lib/kb/causes.ts, locales.ts, data/tenants, data/reports-4471.json, data/desk-notes.json. Review: src/routes/kb/incidents/4471-review/+page.svx. Counts are an export snapshot; reconcile future arrivals.
