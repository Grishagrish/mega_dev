<script lang="ts">
 import reports from '#lib/data/reports-4471.json';
 import notes from '#lib/data/desk-notes.json';
 import {uniqueById, groupByCause, readAs, type Report} from '#lib/kb/causes';
 import {tenantOf} from '#lib/kb/locales';
 const raw = reports as Report[];
 const merged = uniqueById(raw).filter(r => r.mergedInto === 4471);
 const groups = groupByCause(merged,tenantOf);
</script>
<p>{raw.length} exported rows; {uniqueById(raw).length} unique report IDs; {merged.length} reports in #4471. Counts below are computed from the supplied export, not live queue totals.</p>
<table><thead><tr><th>Cause</th><th>Reports</th><th>Tenants</th><th>Received</th></tr></thead><tbody>
{#each groups as g}<tr><td>{g.cause}</td><td>{g.reports.length}</td><td>{g.tenants}</td><td>{g.firstSeen} to {g.lastSeen}</td></tr>{/each}
</tbody></table>
{#each groups as g}
<h3>{g.cause}: intake labels and conflicting evidence</h3>
<p>{Object.entries(g.taggedAs).map(([tag,n]) => `${tag}: ${n}`).join('; ')}. Intake labels describe an initial guess, not the tenant configuration.</p>
<p>{readAs(g,notes.oneDayMove)} reports in this group were read by the desk as one-day moves. These remain counted in their tenant-based group; the descriptions alone do not establish a separate timezone defect.</p>
{/each}
