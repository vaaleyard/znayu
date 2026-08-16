<script lang="ts">
  type ActivityTone = 'green' | 'blue' | 'amber' | 'red';

  type ActivityItem = {
    title: string;
    detail: string;
    actor: string;
    time: string;
    tone: ActivityTone;
    action: string;
  };

  const groups: { label: string; items: ActivityItem[] }[] = [
    {
      label: 'Today',
      items: [
        { title: 'Monitor entered degraded state', detail: 'API · Response time above 800 ms', actor: 'Automated check', time: '2 min ago', tone: 'amber', action: 'View monitor' },
        { title: 'Status page updated', detail: 'Added “Checkout” service to Public status page', actor: 'Jordan Smith', time: '34 min ago', tone: 'blue', action: 'View status page' },
        { title: 'Maintenance scheduled', detail: 'Database migration · 18 Aug, 02:00–03:00 UTC', actor: 'Jordan Smith', time: '1 h ago', tone: 'blue', action: 'View maintenance' }
      ]
    },
    {
      label: 'Yesterday',
      items: [
        { title: 'Monitor created', detail: 'Web application · HTTPS', actor: 'Jordan Smith', time: 'Yesterday, 16:42', tone: 'green', action: 'View monitor' },
        { title: 'Incident resolved', detail: 'Elevated latency on API · Resolved after 18 minutes', actor: 'Automated check', time: 'Yesterday, 11:08', tone: 'green', action: 'View incident' }
      ]
    },
    {
      label: '14 August 2026',
      items: [
        { title: 'Monitor paused', detail: 'Checkout worker · Internal Platform', actor: 'Jordan Smith', time: '14 Aug, 09:20', tone: 'red', action: 'View monitor' }
      ]
    }
  ];

  let activeFilter = 'All activity';
  let sidebarCollapsed = false;
  const filters = ['All activity', 'Incidents', 'Monitors', 'Status pages', 'Team'];
</script>

<svelte:head>
  <title>Activity — Znayu</title>
  <meta name="description" content="Znayu workspace activity log." />
</svelte:head>

<main class:sidebar-collapsed={sidebarCollapsed} class="activity-page">
  <aside class="sidebar" aria-label="Operator navigation">
    <div class="sidebar-top">
      <a class="wordmark" href="/dashboard" aria-label="Znayu operator dashboard">znayu</a>
      <span class="console-label">Console</span>
      <button class="sidebar-toggle" type="button" aria-label="Collapse sidebar" onclick={() => (sidebarCollapsed = !sidebarCollapsed)}>
        <svg aria-hidden="true" viewBox="0 0 20 20"><rect x="3.5" y="3.5" width="13" height="13" rx="2" /><path d="M8 3.5v13m4.5-9.5-3 3 3 3" /></svg>
      </button>
    </div>
    <div class="create-actions">
      <a class="create-button primary" href="/monitors/new"><span aria-hidden="true">+</span> Create monitor</a>
      <a class="create-button secondary" href="/status-pages/new">Create status page</a>
    </div>
    <nav class="sidebar-nav" aria-label="Console sections">
      <a href="/dashboard" aria-label="Overview"><svg aria-hidden="true" viewBox="0 0 20 20"><path d="M3 10h5V3H3v7Zm9 7h5v-7h-5v7ZM3 17h5v-4H3v4Zm9-14v4h5V3h-5Z" /></svg>Overview</a>
      <a href="/status-pages" aria-label="Status pages"><svg aria-hidden="true" viewBox="0 0 20 20"><path d="M3 4.5h14v11H3v-11Zm3 3h8M6 11h5" /></svg>Status pages</a>
      <a href="/monitors" aria-label="Monitors"><svg aria-hidden="true" viewBox="0 0 20 20"><path d="M3 15.5V4.5h14v11H3Zm3-3 2-2 2 1.5 3-4 2 2.5" /></svg>Monitors</a>
      <a class="active" href="/activity" aria-current="page" aria-label="Activity"><svg aria-hidden="true" viewBox="0 0 20 20"><path d="M10 3v7l4 2M17 10a7 7 0 1 1-14 0 7 7 0 0 1 14 0Z" /></svg>Activity</a>
    </nav>
    <div class="sidebar-bottom">
      <div class="operator"><span class="operator-avatar">JS</span><span><strong>Jordan Smith</strong><small>Owner</small></span><button type="button">More</button></div>
    </div>
  </aside>

  <section class="activity-content" aria-labelledby="activity-title">
    <header class="content-header">
      <div><p class="section-kicker">Workspace record</p><h1 id="activity-title">Activity</h1></div>
      <button class="export-button" type="button">Export log</button>
    </header>

    <section class="activity-summary" aria-label="Activity summary">
      <div><strong>128</strong><span>events in the last 30 days</span></div>
      <div><strong>4</strong><span>active incidents</span></div>
      <div><strong>20</strong><span>monitors tracked</span></div>
    </section>

    <div class="toolbar">
      <div class="filters" role="group" aria-label="Filter activity">
        {#each filters as filter}
          <button class:active={activeFilter === filter} type="button" onclick={() => (activeFilter = filter)}>{filter}</button>
        {/each}
      </div>
      <button class="date-filter" type="button">Last 30 days <span aria-hidden="true">⌄</span></button>
    </div>

    <div class="activity-log">
      {#each groups as group}
        <section class="day-group" aria-labelledby={group.label.replaceAll(' ', '-')}>
          <h2 id={group.label.replaceAll(' ', '-')}>{group.label}</h2>
          <div class="timeline">
            {#each group.items as item}
              <article class="event-row">
                <span class="event-marker {item.tone}" aria-hidden="true"></span>
                <div class="event-copy"><h3>{item.title}</h3><p>{item.detail}</p><span class="event-actor">{item.actor} · {item.time}</span></div>
                <a class="event-action" href="/dashboard">{item.action} <span aria-hidden="true">↗</span></a>
              </article>
            {/each}
          </div>
        </section>
      {/each}
    </div>
  </section>
</main>

<style>
  .activity-page { min-height: 100svh; display: grid; grid-template-columns: 232px minmax(0, 1fr); background: var(--page); color: var(--ink); }
  .sidebar { position: sticky; top: 0; display: flex; height: 100svh; min-height: 100svh; padding: 22px 14px 16px; flex-direction: column; background: #141821; border-right: 1px solid var(--border); }
  .sidebar-top { display: flex; align-items: baseline; justify-content: space-between; padding: 0 10px; }
  .wordmark { color: var(--ink); font-size: 1.65rem; font-weight: 720; letter-spacing: -0.045em; line-height: 1; text-decoration: none; }
  .console-label, .section-kicker, .operator small { color: var(--muted); font-size: .6875rem; line-height: 1.4; }
  .console-label { letter-spacing: .02em; text-transform: uppercase; }
  .create-actions { display: grid; gap: 8px; margin: 38px 0 28px; }
  .create-button { display: flex; min-height: 38px; align-items: center; justify-content: center; padding: 0 12px; border-radius: 5px; font-size: .8125rem; font-weight: 620; text-decoration: none; }
  .create-button span { margin-right: 4px; font-size: 1rem; }.create-button.primary { border: 1px solid var(--green); color: var(--page); background: var(--green); }.create-button.secondary { border: 1px solid var(--border-light); color: var(--secondary); background: transparent; }
  .sidebar-nav { display: grid; gap: 3px; }.sidebar-nav a { display: flex; align-items: center; gap: 11px; padding: 10px; border-radius: 5px; color: var(--secondary); font-size: .8125rem; text-decoration: none; }.sidebar-nav a:hover, .sidebar-nav a.active { color: var(--ink); background: var(--surface-hover); }.sidebar-nav svg { display: block; width: 16px; height: 16px; flex: 0 0 auto; fill: none; stroke: currentColor; stroke-linecap: round; stroke-linejoin: round; stroke-width: 1.5; }.sidebar-nav a:first-child svg { fill: currentColor; stroke: none; }
  .sidebar-bottom { display: grid; gap: 20px; margin-top: auto; }.public-link { padding: 0 10px; color: var(--secondary); font-size: .6875rem; }.operator { display: flex; align-items: center; gap: 9px; padding: 12px 8px 0; border-top: 1px solid var(--border); }.operator-avatar { display: grid; width: 28px; height: 28px; place-items: center; border-radius: 50%; color: var(--page); background: var(--green); font-size: .6875rem; font-weight: 700; }.operator strong, .operator small { display: block; }.operator strong { color: var(--secondary); font-size: .6875rem; }.operator button { margin-left: auto; border: 0; color: var(--muted); background: transparent; font-size: .6875rem; }
  .sidebar-toggle { display: grid; width: 24px; height: 24px; place-items: center; padding: 0; border: 1px solid var(--border-light); border-radius: 5px; color: var(--muted); background: transparent; }.sidebar-toggle svg { width: 14px; height: 14px; fill: none; stroke: currentColor; stroke-linecap: round; stroke-linejoin: round; stroke-width: 1.5; }
  .sidebar-collapsed.activity-page { grid-template-columns: 72px minmax(0, 1fr); }.sidebar-collapsed .sidebar { padding-right: 10px; padding-left: 10px; }.sidebar-collapsed .sidebar-top { align-items: center; flex-direction: column; gap: 12px; padding: 0; }.sidebar-collapsed .wordmark { font-size: 0; }.sidebar-collapsed .wordmark::after { color: var(--ink); content: 'z'; font-size: 1.65rem; font-weight: 720; letter-spacing: -.045em; }.sidebar-collapsed .console-label { display: none; }.sidebar-collapsed .create-actions { justify-items: center; }.sidebar-collapsed .create-button { width: 40px; padding: 0; font-size: 0; }.sidebar-collapsed .create-button span { margin: 0; font-size: 0; }.sidebar-collapsed .create-button span::before { color: var(--page); content: '+'; font-size: 1rem; }.sidebar-collapsed .create-button.secondary { display: none; }.sidebar-collapsed .sidebar-nav a { position: relative; justify-content: center; gap: 0; padding-right: 10px; padding-left: 10px; font-size: 0; }.sidebar-collapsed .sidebar-nav a:hover, .sidebar-collapsed .sidebar-nav a:focus-visible { background: var(--surface-hover); box-shadow: inset 2px 0 0 var(--green); }.sidebar-collapsed .sidebar-nav a.active { color: var(--ink); background: var(--surface-hover); box-shadow: inset 2px 0 0 var(--green); }.sidebar-collapsed .sidebar-nav a::after { position: absolute; z-index: 2; top: 50%; left: calc(100% + 10px); display: none; padding: 7px 9px; border: 1px solid var(--border-light); border-radius: 5px; color: var(--ink); background: var(--surface); box-shadow: 0 6px 14px rgb(0 0 0 / 18%); content: attr(aria-label); font-size: .6875rem; line-height: 1; white-space: nowrap; transform: translateY(-50%); }.sidebar-collapsed .sidebar-nav a:hover::after, .sidebar-collapsed .sidebar-nav a:focus-visible::after { display: block; }.sidebar-collapsed .operator { justify-content: center; padding-right: 0; padding-left: 0; }.sidebar-collapsed .operator > span:nth-child(2), .sidebar-collapsed .operator button { display: none; }
  .activity-content { width: min(100%, 1000px); margin: 0 auto; padding: 84px 48px 64px; }.content-header { display: flex; align-items: flex-end; justify-content: space-between; margin-bottom: 30px; }.section-kicker { margin: 0 0 7px; }.content-header h1 { margin: 0; font-size: 1.65rem; font-weight: 680; letter-spacing: -.03em; line-height: 1.25; }.export-button, .date-filter, .filters button { border: 1px solid var(--border-light); border-radius: 5px; color: var(--secondary); background: transparent; font-size: .75rem; cursor: pointer; }.export-button { min-height: 36px; padding: 0 13px; }.activity-summary { display: grid; grid-template-columns: repeat(3, 1fr); border: 1px solid var(--border); border-radius: 12px; background: var(--surface); }.activity-summary div { padding: 21px 24px; }.activity-summary div + div { border-left: 1px solid var(--border); }.activity-summary strong, .activity-summary span { display: block; }.activity-summary strong { font-size: 1.1rem; font-weight: 680; }.activity-summary span { margin-top: 4px; color: var(--muted); font-size: .6875rem; }.toolbar { display: flex; align-items: center; justify-content: space-between; gap: 16px; margin: 28px 0 34px; }.filters { display: flex; flex-wrap: wrap; gap: 5px; }.filters button { padding: 8px 11px; border-color: transparent; }.filters button:hover, .filters button.active { color: var(--ink); background: var(--surface-hover); }.date-filter { min-height: 34px; padding: 0 11px; white-space: nowrap; }.date-filter span { margin-left: 7px; color: var(--muted); }
  .day-group + .day-group { margin-top: 37px; }.day-group h2 { margin: 0 0 10px; color: var(--muted); font-size: .6875rem; font-weight: 650; letter-spacing: .02em; text-transform: uppercase; }.timeline { border-top: 1px solid var(--border); }.event-row { display: grid; grid-template-columns: 10px minmax(0, 1fr) auto; align-items: start; gap: 15px; padding: 19px 4px; border-bottom: 1px solid var(--border); }.event-marker { width: 8px; height: 8px; margin-top: 5px; border-radius: 50%; background: var(--green); box-shadow: 0 0 0 4px rgb(16 185 129 / 9%); }.event-marker.blue { background: var(--blue); box-shadow: 0 0 0 4px rgb(96 165 250 / 9%); }.event-marker.amber { background: var(--amber); box-shadow: 0 0 0 4px rgb(245 158 11 / 9%); }.event-marker.red { background: var(--red); box-shadow: 0 0 0 4px rgb(248 113 113 / 9%); }.event-copy h3 { margin: 0; font-size: .8125rem; font-weight: 620; line-height: 1.4; }.event-copy p { margin: 4px 0 0; color: var(--secondary); font-size: .8125rem; line-height: 1.45; }.event-actor { display: block; margin-top: 7px; color: var(--muted); font-size: .6875rem; }.event-action { align-self: center; color: var(--secondary); font-size: .6875rem; text-decoration: none; white-space: nowrap; }.event-action:hover { color: var(--ink); }.event-action span { margin-left: 4px; }
  :global(:focus-visible) { outline: 2px solid var(--green); outline-offset: 3px; }
  @media (max-width: 760px) { .activity-content { padding: 54px 28px 48px; }.event-row { grid-template-columns: 10px minmax(0, 1fr); }.event-action { grid-column: 2; justify-self: start; margin-top: 4px; }.toolbar { align-items: flex-start; flex-direction: column; }.date-filter { align-self: flex-end; } }
  @media (max-width: 680px) { .activity-page { display: block; }.sidebar { position: static; height: auto; min-height: auto; padding: 14px 12px 12px; border-right: 0; border-bottom: 1px solid var(--border); }.sidebar-top { padding: 0 4px; }.create-actions { display: flex; margin: 22px 0 14px; }.create-button { flex: 1; }.sidebar-nav { display: flex; overflow-x: auto; }.sidebar-nav a { flex: 0 0 auto; }.sidebar-bottom { display: none; }.activity-content { padding: 28px 16px 44px; }.activity-summary div { padding: 17px 14px; }.activity-summary span { line-height: 1.35; }.content-header { margin-bottom: 22px; } }
  @media (max-width: 440px) { .activity-summary strong { font-size: .95rem; }.activity-summary span { font-size: .625rem; }.filters { gap: 2px; }.filters button { padding: 7px 8px; font-size: .6875rem; } }
  .sidebar-collapsed .sidebar-nav a:hover, .sidebar-collapsed .sidebar-nav a:focus-visible { background: #1a1d25; box-shadow: inset 2px 0 0 var(--border-light); }
  .sidebar-collapsed .sidebar-nav a:hover, .sidebar-collapsed .sidebar-nav a:focus-visible { color: #e3e6ee; background: #1e222b; box-shadow: none; }.sidebar-collapsed .sidebar-nav a.active { color: var(--ink); background: #282c36; box-shadow: none; }.sidebar-collapsed .sidebar-nav a::after { left: calc(100% + 16px); }
</style>
