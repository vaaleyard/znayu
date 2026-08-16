---
version: 1
slug: "src-routes-dashboard-page-svelte"
primary_target: "src/routes/dashboard/+page.svelte"
related_targets: ["src/routes/login/+page.svelte","src/routes/+layout.svelte","src/app.css"]
---

---
version: 1
slug: "src-routes-dashboard-page-svelte"
primary_target: "src/routes/dashboard/+page.svelte"
related_targets: ["src/routes/login/+page.svelte", "src/routes/+layout.svelte", "src/app.css"]
---

# Operator dashboard

## Scope and mode

- Route: `/dashboard`
- Visitor mode: interface-only operator workspace.
- Audience: operators who create monitors, publish status pages, and review service health.
- Job: understand workspace health at a glance and find the two primary creation actions immediately.

## Chosen direction

The dashboard extends Znayu's operational register into a restrained console. A compact sidebar anchors operator navigation and the two creation actions. The main workspace leads with aggregate health, then presents status pages, recent activity, and monitor coverage using sample data.

## Boundaries

- No authentication, navigation state, creation flow, persistence, monitoring, or API behavior is implemented.
- Buttons and links are presentational affordances only.
- All values are illustrative and remain clearly within the interface prototype.
