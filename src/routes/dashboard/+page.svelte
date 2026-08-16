<script lang="ts">
  import { onMount } from 'svelte';

  type Health = 'Operational' | 'Degraded' | 'Investigating';

  type StatusPage = {
    name: string;
    slug: string;
    statusSlug: string;
    services: number;
    uptime: string;
    health: Health;
  };

  const statusPages: StatusPage[] = [
    { name: 'Acme Cloud', slug: 'status.acme.cloud', statusSlug: 'acme-cloud', services: 8, uptime: '99.98%', health: 'Operational' },
    { name: 'Internal Platform', slug: 'platform.internal', statusSlug: 'internal-platform', services: 12, uptime: '99.94%', health: 'Degraded' }
  ];

  const activity = [
    { title: 'Database maintenance scheduled', detail: 'Acme Cloud · 18 Aug 2026', tone: 'blue' },
    { title: 'Public API latency resolved', detail: 'Acme Cloud · 15 Aug 2026', tone: 'green' },
    { title: 'Checkout monitor updated', detail: 'Internal Platform · 14 Aug 2026', tone: 'amber' }
  ];

  let sidebarCollapsed = false;

  onMount(() => {
    sidebarCollapsed = localStorage.getItem('znayu.sidebar.collapsed') === 'true';
  });

  function toggleSidebar() {
    sidebarCollapsed = !sidebarCollapsed;
    localStorage.setItem('znayu.sidebar.collapsed', String(sidebarCollapsed));
  }
</script>

<svelte:head>
  <title>Overview — Znayu</title>
  <meta name="description" content="Znayu operator dashboard overview." />
</svelte:head>

<main class:sidebar-collapsed={sidebarCollapsed} class="dashboard-page">
  <aside class="sidebar" aria-label="Operator navigation">
    <div class="sidebar-top">
      <a class="wordmark" href="/dashboard" aria-label="Znayu operator dashboard">znayu</a>
      <span class="console-label">Console</span>
      <button class="sidebar-toggle" type="button" aria-label={sidebarCollapsed ? 'Expand sidebar' : 'Collapse sidebar'} title={sidebarCollapsed ? 'Expand sidebar' : 'Collapse sidebar'} onclick={toggleSidebar}>
        <svg aria-hidden="true" viewBox="0 0 20 20"><rect x="3.5" y="3.5" width="13" height="13" rx="2" /><path d="M8 3.5v13" />{#if sidebarCollapsed}<path d="m10.5 7 3 3-3 3" />{:else}<path d="m12.5 7-3 3 3 3" />{/if}</svg>
      </button>
    </div>

    <div class="create-actions">
      <a class="create-button primary" href="/monitors/new"><span aria-hidden="true">+</span> Create monitor</a>
      <a class="create-button secondary" href="/status-pages/new">Create status page</a>
    </div>

    <nav class="sidebar-nav" aria-label="Console sections">
      <a class="active" href="/dashboard" aria-current="page">
        <svg aria-hidden="true" viewBox="0 0 20 20"><path d="M3 10h5V3H3v7Zm9 7h5v-7h-5v7ZM3 17h5v-4H3v4Zm9-14v4h5V3h-5Z" /></svg>
        Overview
      </a>
      <a href="/status-pages">
        <svg aria-hidden="true" viewBox="0 0 20 20"><path d="M3 4.5h14v11H3v-11Zm3 3h8M6 11h5" /></svg>
        Status pages
      </a>
      <a href="/monitors">
        <svg aria-hidden="true" viewBox="0 0 20 20"><path d="M3 15.5V4.5h14v11H3Zm3-3 2-2 2 1.5 3-4 2 2.5" /></svg>
        Monitors
      </a>
      <a href="#activity">
        <svg aria-hidden="true" viewBox="0 0 20 20"><path d="M10 3v7l4 2M17 10a7 7 0 1 1-14 0 7 7 0 0 1 14 0Z" /></svg>
        Activity
      </a>
    </nav>

    <div class="sidebar-bottom">
      <a href="/status/acme-cloud" class="public-link">View public status <svg aria-hidden="true" viewBox="0 0 16 16"><path d="M4 12 12 4M6 4h6v6" /></svg></a>
      <div class="operator">
        <span class="operator-avatar" aria-hidden="true">JS</span>
        <span><strong>Jordan Smith</strong><small>Owner</small></span>
        <button type="button" aria-label="Open account menu">More</button>
      </div>
    </div>
  </aside>

  <section class="dashboard-content" aria-labelledby="dashboard-title">
    <header class="content-header">
      <div>
        <p class="section-kicker">Wednesday, August 16, 2026</p>
        <h1 id="dashboard-title">Overview</h1>
      </div>
      <button class="notification-button" type="button" aria-label="Notifications">
        <svg aria-hidden="true" viewBox="0 0 20 20"><path d="M4.5 14.5h11l-1.2-1.8V8a4.3 4.3 0 0 0-8.6 0v4.7l-1.2 1.8ZM8 17h4" /></svg>
        <span aria-hidden="true"></span>
      </button>
    </header>

    <section class="health-summary" aria-labelledby="health-title">
      <div class="health-intro">
        <span class="health-dot" aria-hidden="true"></span>
        <div>
          <p class="summary-label">Workspace health</p>
          <h2 id="health-title">All systems operational</h2>
          <p class="summary-detail">Across 20 monitored services · Updated 1 minute ago</p>
        </div>
      </div>
      <div class="summary-metrics" aria-label="Workspace metrics">
        <div><strong>2</strong><span>Status pages</span></div>
        <div><strong>20</strong><span>Monitors</span></div>
        <div><strong>99.97%</strong><span>30-day uptime</span></div>
      </div>
    </section>

    <div class="dashboard-grid">
      <section class="panel status-pages-panel" id="status-pages" aria-labelledby="status-pages-title">
        <header class="panel-header">
          <div><h2 id="status-pages-title">Status pages</h2><p>Public surfaces connected to this workspace.</p></div>
          <a href="/status-pages">View all</a>
        </header>
        <div class="status-page-list">
          {#each statusPages as page}
            <article class="status-page-row">
              <span class="page-mark {page.health.toLowerCase()}" role="img" aria-label={`${page.name}: ${page.health}`}></span>
                <div class="page-identity"><h3><a href={`/status/${page.statusSlug}`}>{page.name}</a></h3><span>{page.slug}</span></div>
              <div class="page-health {page.health.toLowerCase()}"><span></span>{page.health}</div>
              <div class="page-stat"><strong>{page.uptime}</strong><span>{page.services} services</span></div>
              <button type="button" class="row-menu" aria-label={`Actions for ${page.name}`}>More</button>
            </article>
          {/each}
        </div>
        <button class="panel-add" type="button">+ Create status page</button>
      </section>

      <section class="panel activity-panel" id="activity" aria-labelledby="activity-title">
        <header class="panel-header"><div><h2 id="activity-title">Recent activity</h2><p>Changes across your workspace.</p></div></header>
        <div class="activity-list">
          {#each activity as item}
            <article class="activity-row">
              <span class="activity-dot {item.tone}" aria-hidden="true"></span>
              <div><h3>{item.title}</h3><p>{item.detail}</p></div>
            </article>
          {/each}
        </div>
        <a class="panel-link" href="#activity-log">View activity log</a>
      </section>

      <section class="panel monitors-panel" id="monitors" aria-labelledby="monitors-title">
        <header class="panel-header">
          <div><h2 id="monitors-title">Monitor coverage</h2><p>Service checks across your status pages.</p></div>
          <a class="text-button" href="/monitors">Manage monitors</a>
        </header>
        <div class="coverage-row">
          <div class="coverage-value"><strong>20</strong><span>active monitors</span></div>
          <div class="coverage-rail" role="img" aria-label="18 operational monitors, 1 degraded monitor, and 1 paused monitor">
            <span class="operational" style="width: 90%"></span><span class="degraded" style="width: 5%"></span><span class="paused" style="width: 5%"></span>
          </div>
          <div class="coverage-legend"><span><i class="operational"></i>18 operational</span><span><i class="degraded"></i>1 degraded</span><span><i class="paused"></i>1 paused</span></div>
        </div>
      </section>
    </div>
  </section>
</main>

<style>
  .dashboard-page { min-height: 100svh; display: grid; grid-template-columns: 232px minmax(0, 1fr); background: var(--page); color: var(--ink); }
  .sidebar { position: sticky; top: 0; display: flex; height: 100svh; min-height: 100svh; padding: 22px 14px 16px; flex-direction: column; background: #141821; border-right: 1px solid var(--border); }
  .sidebar-top { display: flex; align-items: baseline; justify-content: space-between; padding: 0 10px; }
  .wordmark { color: var(--ink); font-size: 1.65rem; font-weight: 720; letter-spacing: -0.045em; line-height: 1; text-decoration: none; }
  .console-label, .section-kicker, .summary-label, .page-identity span, .page-stat span, .coverage-legend, .operator small { color: var(--muted); font-size: 0.6875rem; line-height: 1.4; }
  .console-label { letter-spacing: 0.02em; text-transform: uppercase; }
  .create-actions { display: grid; gap: 8px; margin: 38px 0 28px; }
  button, a { font: inherit; }
  .create-button { display: flex; min-height: 38px; align-items: center; justify-content: center; padding: 0 12px; border-radius: 5px; font-size: 0.8125rem; font-weight: 620; cursor: pointer; text-decoration: none; }
  .create-button span { font-size: 1rem; line-height: 0; }
  .create-button.primary { border: 1px solid var(--green); color: var(--page); background: var(--green); }
  .create-button.secondary { border: 1px solid var(--border-light); color: var(--secondary); background: transparent; }
  .create-button:hover, .notification-button:hover { filter: brightness(1.08); }
  .sidebar-nav { display: grid; gap: 3px; }
  .sidebar-nav a { display: flex; align-items: center; gap: 11px; padding: 10px; border-radius: 5px; color: var(--secondary); font-size: 0.8125rem; text-decoration: none; }
  .sidebar-nav a:hover, .sidebar-nav a.active { color: var(--ink); background: var(--surface-hover); }
  .sidebar-nav svg, .notification-button svg { display: block; width: 16px; height: 16px; fill: none; stroke: currentColor; stroke-linecap: round; stroke-linejoin: round; stroke-width: 1.5; }
  .sidebar-nav a:first-child svg { fill: currentColor; stroke: none; }
  .sidebar-bottom { display: grid; gap: 20px; margin-top: auto; }
  .public-link { display: inline-flex; align-items: center; gap: 5px; padding: 0 10px; color: var(--secondary); font-size: 0.6875rem; text-underline-offset: 3px; }
  .public-link svg { width: 12px; height: 12px; fill: none; stroke: currentColor; stroke-linecap: round; stroke-linejoin: round; stroke-width: 1.25; }
  .operator { display: flex; align-items: center; gap: 9px; padding: 12px 8px 0; border-top: 1px solid var(--border); }
  .operator-avatar { display: grid; width: 28px; height: 28px; place-items: center; border-radius: 50%; color: var(--page); background: var(--green); font-size: 0.6875rem; font-weight: 700; }
  .operator span:nth-child(2) { display: grid; min-width: 0; gap: 2px; }
  .operator strong { overflow: hidden; color: var(--secondary); font-size: 0.6875rem; font-weight: 600; text-overflow: ellipsis; white-space: nowrap; }
  .operator button, .row-menu { margin-left: auto; border: 0; color: var(--muted); background: transparent; cursor: pointer; font-size: 0.6875rem; }
  .dashboard-content { width: min(100%, 1080px); margin: 0 auto; padding: 84px 48px 64px; }
  .content-header { display: flex; align-items: flex-end; justify-content: space-between; margin-bottom: 30px; }
  .section-kicker { margin: 0 0 7px; }
  h1, h2, h3, p { margin: 0; }
  h1 { font-size: 1.65rem; font-weight: 680; letter-spacing: -0.03em; line-height: 1.25; }
  .notification-button { position: relative; display: grid; width: 36px; height: 36px; place-items: center; border: 1px solid var(--border-light); border-radius: 5px; color: var(--secondary); background: transparent; cursor: pointer; }
  .notification-button > span { position: absolute; top: 8px; right: 8px; width: 5px; height: 5px; border-radius: 50%; background: var(--green); }
  .health-summary { display: flex; align-items: center; justify-content: space-between; gap: 32px; padding: 25px 28px; border: 1px solid var(--border); border-radius: 12px; background: var(--surface); box-shadow: 0 4px 7px rgb(0 0 0 / 3%); }
  .health-intro { display: flex; align-items: flex-start; gap: 15px; }
  .health-dot { flex: 0 0 auto; width: 12px; height: 12px; margin-top: 5px; border-radius: 50%; background: var(--green); box-shadow: 0 0 0 6px rgb(16 185 129 / 12%); }
  .summary-label { margin-bottom: 5px; text-transform: uppercase; letter-spacing: 0.02em; }
  .health-summary h2 { font-size: 1rem; font-weight: 700; line-height: 1.35; }
  .summary-detail { margin-top: 5px; color: var(--secondary); font-size: 0.8125rem; line-height: 1.4; }
  .summary-metrics { display: flex; align-items: stretch; }
  .summary-metrics div { min-width: 100px; padding-left: 20px; border-left: 1px solid var(--border-light); }
  .summary-metrics strong, .summary-metrics span { display: block; }
  .summary-metrics strong { font-size: 1rem; font-weight: 650; line-height: 1.35; }
  .summary-metrics span { margin-top: 4px; color: var(--muted); font-size: 0.6875rem; line-height: 1.3; }
  .dashboard-grid { display: grid; grid-template-columns: minmax(0, 1.35fr) minmax(260px, 0.75fr); gap: 20px; margin-top: 20px; }
  .panel { min-width: 0; border: 1px solid var(--border); border-radius: 12px; background: var(--surface); }
  .panel-header { display: flex; align-items: flex-start; justify-content: space-between; gap: 16px; padding: 22px 22px 18px; }
  .panel-header h2 { font-size: 1rem; font-weight: 700; line-height: 1.35; }
  .panel-header p { margin-top: 4px; color: var(--secondary); font-size: 0.8125rem; line-height: 1.45; }
  .panel-header a, .text-button, .panel-link { color: var(--secondary); font-size: 0.6875rem; text-underline-offset: 3px; }
  .text-button, .panel-link { border: 0; background: transparent; cursor: pointer; text-decoration: none; }
  .status-page-list { border-top: 1px solid var(--border); }
  .status-page-row { display: grid; grid-template-columns: 10px minmax(120px, 1fr) auto auto 20px; align-items: center; gap: 12px; min-height: 70px; padding: 12px 22px; }
  .status-page-row + .status-page-row { border-top: 1px solid var(--border); }
  .page-mark { width: 10px; height: 10px; border-radius: 50%; background: var(--green); }
  .page-mark:not(.operational) { background: var(--red); }
  .page-identity { min-width: 0; }
  .page-identity h3, .activity-row h3 { overflow: hidden; font-size: 0.8125rem; font-weight: 600; line-height: 1.4; text-overflow: ellipsis; white-space: nowrap; }.page-identity h3 a { color: inherit; text-decoration: none; }.page-identity h3 a:hover { text-decoration: underline; text-underline-offset: 3px; }
  .page-identity span { display: block; overflow: hidden; margin-top: 2px; text-overflow: ellipsis; white-space: nowrap; }
  .page-health { display: flex; align-items: center; gap: 6px; font-size: 0.6875rem; font-weight: 600; white-space: nowrap; }
  .page-health span { width: 6px; height: 6px; border-radius: 50%; background: var(--green); }
  .page-health.degraded { color: var(--amber); }
  .page-health.degraded span { background: var(--amber); }
  .page-health.operational { color: var(--green); }
  .page-stat { display: grid; min-width: 76px; gap: 2px; text-align: right; }
  .page-stat strong { font-size: 0.8125rem; font-weight: 620; }
  .page-stat span { font-size: 0.6875rem; }
  .panel-add { width: 100%; padding: 14px 22px; border: 0; border-top: 1px solid var(--border); color: var(--secondary); background: transparent; font-size: 0.8125rem; text-align: left; cursor: pointer; }
  .panel-add:hover, .panel-link:hover, .text-button:hover { color: var(--ink); }
  .activity-list { padding: 0 22px; border-top: 1px solid var(--border); }
  .activity-row { display: grid; grid-template-columns: 8px minmax(0, 1fr); gap: 12px; padding: 16px 0; }
  .activity-row + .activity-row { border-top: 1px solid var(--border); }
  .activity-dot { width: 7px; height: 7px; margin-top: 4px; border-radius: 50%; background: var(--green); }
  .activity-dot.blue { background: var(--blue); }.activity-dot.amber { background: var(--amber); }
  .activity-row p { margin-top: 4px; color: var(--muted); font-size: 0.6875rem; line-height: 1.4; }
  .panel-link { display: inline-block; margin: 0 22px 19px; }
  .monitors-panel { grid-column: 1 / -1; }
  .coverage-row { display: grid; grid-template-columns: 150px minmax(120px, 1fr) 220px; align-items: center; gap: 24px; padding: 20px 22px 22px; border-top: 1px solid var(--border); }
  .coverage-value strong { display: block; font-size: 1rem; font-weight: 680; line-height: 1.25; }.coverage-value span { display: block; margin-top: 3px; color: var(--muted); font-size: 0.6875rem; }
  .coverage-rail { display: flex; overflow: hidden; height: 8px; gap: 2px; border-radius: 1px; background: var(--border-light); }
  .coverage-rail span { display: block; height: 100%; }.coverage-rail .operational { background: var(--green); }.coverage-rail .degraded { background: var(--amber); }.coverage-rail .paused { background: var(--muted); }
  .coverage-legend { display: grid; gap: 5px; }.coverage-legend span { display: flex; align-items: center; gap: 6px; }.coverage-legend i { width: 6px; height: 6px; border-radius: 50%; background: var(--green); }.coverage-legend i.degraded { background: var(--amber); }.coverage-legend i.paused { background: var(--muted); }
  :focus-visible { outline: 2px solid var(--green); outline-offset: 3px; }
  @media (max-width: 900px) { .dashboard-content { padding-right: 28px; padding-left: 28px; }.health-summary { align-items: flex-start; flex-direction: column; }.summary-metrics { width: 100%; }.summary-metrics div { flex: 1; }.coverage-row { grid-template-columns: 120px minmax(100px, 1fr); }.coverage-legend { grid-column: 1 / -1; grid-template-columns: repeat(3, 1fr); } }
  @media (max-width: 680px) { .dashboard-page { display: block; }.sidebar { position: static; height: auto; min-height: auto; padding: 14px 12px 12px; border-right: 0; border-bottom: 1px solid var(--border); }.sidebar-top { padding: 0 4px; }.create-actions { display: flex; margin: 22px 0 14px; }.create-button { flex: 1; }.sidebar-nav { display: flex; overflow-x: auto; }.sidebar-nav a { flex: 0 0 auto; }.sidebar-bottom { display: none; }.dashboard-content { padding: 28px 16px 44px; }.dashboard-grid { display: block; }.panel { margin-top: 16px; }.monitors-panel { margin-bottom: 0; }.status-page-row { grid-template-columns: 10px minmax(100px, 1fr) auto 20px; }.page-stat { display: none; } }
  @media (max-width: 440px) { .content-header { margin-bottom: 22px; }.summary-metrics div { min-width: 0; padding-left: 12px; }.summary-metrics strong { font-size: 0.9375rem; }.summary-metrics span { font-size: 0.6875rem; }.health-summary { padding: 20px; }.coverage-row { grid-template-columns: 1fr; gap: 14px; }.coverage-legend { grid-template-columns: 1fr; }.panel-header { padding-right: 18px; padding-left: 18px; }.status-page-row { padding-right: 18px; padding-left: 18px; } }
  .sidebar-toggle { display: grid; width: 24px; height: 24px; flex: 0 0 auto; place-items: center; padding: 0; border: 1px solid var(--border-light); border-radius: 5px; color: var(--muted); background: transparent; cursor: pointer; }
  .sidebar-toggle:hover { color: var(--ink); border-color: var(--secondary); }
  .sidebar-toggle svg { width: 14px; height: 14px; fill: none; stroke: currentColor; stroke-linecap: round; stroke-linejoin: round; stroke-width: 1.5; }
  .sidebar-collapsed.dashboard-page { grid-template-columns: 72px minmax(0, 1fr); }
  .sidebar-collapsed .sidebar { padding-right: 10px; padding-left: 10px; }
  .sidebar-collapsed .sidebar-top { align-items: center; flex-direction: column; gap: 12px; padding: 0; }
  .sidebar-collapsed .wordmark { font-size: 0; }
  .sidebar-collapsed .wordmark::after { color: var(--ink); content: 'z'; font-size: 1.65rem; font-weight: 720; letter-spacing: -0.045em; }
  .sidebar-collapsed .console-label { display: none; }
  .create-button span { display: none; }
  .sidebar-collapsed .create-button span { display: inline; font-size: 1rem; line-height: 0; }
  .sidebar-collapsed .create-button span { display: grid; width: 18px; height: 18px; place-items: center; font-size: 1.25rem; line-height: 1; }
  .sidebar-collapsed .create-button span { position: relative; display: block; width: 14px; height: 14px; font-size: 0; }
  .sidebar-collapsed .create-button { gap: 0; }
  .sidebar-collapsed .create-button span::before, .sidebar-collapsed .create-button span::after { position: absolute; top: 6px; left: 1px; width: 12px; height: 2px; border-radius: 1px; background: currentColor; content: ''; }
  .sidebar-collapsed .create-button span::after { transform: rotate(90deg); }
  .sidebar-collapsed .create-actions { justify-items: center; }
  .sidebar-collapsed .create-button { width: 40px; padding: 0; font-size: 0; }
  .sidebar-collapsed .create-button.secondary { display: none; }
  .sidebar-collapsed .sidebar-nav a { justify-content: center; gap: 0; padding-right: 10px; padding-left: 10px; font-size: 0; }
  .sidebar-collapsed .sidebar-nav svg { width: 18px; height: 18px; }
  .sidebar-collapsed .public-link { display: none; }
  .sidebar-collapsed .operator { justify-content: center; padding-right: 0; padding-left: 0; }
  .sidebar-collapsed .operator > span:nth-child(2), .sidebar-collapsed .operator button { display: none; }
  @media (max-width: 680px) { .sidebar-toggle { display: none; }.sidebar-collapsed.dashboard-page { display: block; }.sidebar-collapsed .sidebar { padding-right: 12px; padding-left: 12px; }.sidebar-collapsed .sidebar-top { align-items: baseline; flex-direction: row; gap: 0; padding: 0 4px; }.sidebar-collapsed .wordmark { font-size: 1.65rem; }.sidebar-collapsed .wordmark::after, .sidebar-collapsed .console-label { display: none; }.sidebar-collapsed .create-button { width: auto; padding: 0 12px; font-size: 0.8125rem; }.sidebar-collapsed .create-button.secondary { display: flex; }.sidebar-collapsed .sidebar-nav a { justify-content: flex-start; gap: 11px; padding-right: 10px; padding-left: 10px; font-size: 0.8125rem; }.sidebar-collapsed .operator { justify-content: flex-start; padding-right: 8px; padding-left: 8px; }.sidebar-collapsed .operator > span:nth-child(2), .sidebar-collapsed .operator button { display: initial; } }
</style>
