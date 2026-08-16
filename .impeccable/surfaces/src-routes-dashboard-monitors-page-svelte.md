---
version: 1
slug: "src-routes-dashboard-monitors-page-svelte"
primary_target: "src/routes/dashboard/monitors/+page.svelte"
related_targets: ["src/routes/dashboard/monitors/new/+page.svelte","src/routes/dashboard/+page.svelte","src/routes/login/+page.svelte","src/routes/+layout.svelte","src/app.css"]
---

---
version: 2
slug: "src-routes-dashboard-monitors-page-svelte"
primary_target: "src/routes/dashboard/monitors/+page.svelte"
related_targets: ["src/routes/dashboard/monitors/new/+page.svelte", "src/routes/dashboard/+page.svelte", "src/routes/login/+page.svelte", "src/routes/+layout.svelte", "src/app.css"]
---

# Monitors

## Scope and mode

- Routes: `/dashboard/monitors` and `/dashboard/monitors/new`
- Visitor mode: interface-only operator monitor list and creation form.
- Audience: operators reviewing monitored services and starting a new monitor configuration.
- Job: scan monitor health quickly, then enter the dedicated creation form when needed.

## Chosen direction

The list surface follows the supplied uptime-console reference at the structural level: page heading with search and create action, a compact introduction banner, and one continuous monitor register with state, uptime context, interval, and row actions. The form lives on its own route so configuration does not compete with list scanning.

## Boundaries

- No persistence, validation, HTTP requests, notifications, proxy setup, or monitoring behavior is implemented.
- Controls and row actions are presentational affordances only.
- Sample monitor names, uptime values, and configuration values are illustrative.
