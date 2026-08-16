---
version: 1
slug: "src-routes-page-svelte"
primary_target: "src/routes/+page.svelte"
related_targets: ["src/routes/+layout.svelte", "src/app.html", "src/app.css"]
---

# Public status page

## Scope and mode

- Route: `/`
- Visitor mode: read-only prototype.
- Audience: visitors checking service health, maintenance, and incident history.
- Job: confirm current availability, scan daily uptime, and understand one resolved API incident without seeing monitor configuration.

## Chosen direction

Znayu uses the finish and density of a modern commercial status page without copying the composition of Better Stack or ReelFlix. The interface is a calm operational register: exact, compact, and intentionally self-explanatory.

The desktop reading column is 800px wide. The wordmark sits left, three navigation items remain exactly centered, and the disabled Subscribe action sits right. A horizontal health masthead leads directly into one unlabelled continuous service surface. Four service rows show status and percentages above 25px uptime rails. Maintenance and one resolved Public API incident appear below the fold as editorial records.

## Design-system reading

- Page: deep blue-black `#0f121a`.
- Primary surface: `#191c24`, 12px radius, 1px border, nearly imperceptible shadow.
- Structure: `#21242d` and `#2d313c` hairlines.
- Semantic colors: emerald `#10b981`, incident red `#f87171`, maintenance blue `#60a5fa`, degraded amber `#f59e0b`.
- Typography: native system sans with tabular numerals and a compact scale.
- Layout: 800px desktop column; 20px, 16px, and 12px responsive gutters.
- Uptime: 90 one-day segments on desktop; latest 30 on screens up to 520px.

## Implementation inventory

| Ingredient | Commitment | Medium |
| --- | --- | --- |
| Header | Text wordmark, centered navigation, disabled Subscribe action | Semantic HTML and CSS |
| Health masthead | Green state dot, overall state, service count, relative update time | Semantic HTML and CSS |
| Service register | Four continuous rows with hairline separation | Svelte, semantic HTML and CSS |
| Uptime history | 90 one-day segments desktop, latest 30 mobile | Svelte data and CSS Grid |
| Maintenance | One scheduled database upgrade below the register | Semantic HTML and CSS |
| Incident | One resolved Public API incident and aligned unavailable day | Semantic HTML and CSS |
| Assets | No raster assets; system type and CSS geometry only | HTML and CSS |

## Boundaries

- UI copy and document language are English.
- Monitoring, persistence, navigation destinations, subscription, and historical pagination are not implemented.
- Public content never exposes protocols, methods, endpoints, or ports.
- Generated comps and question logs remain local and are ignored by Git.
