<script lang="ts">
  import { onMount } from 'svelte';

  type Monitor = { name: string; uptime: string; detail: string; interval: string; state: 'operational' | 'degraded' };

  const monitors: Monitor[] = [
    { name: 'Main website', uptime: '99.98%', detail: 'Up · 2d 54m · Used on 1 status page', interval: '3m', state: 'operational' },
    { name: 'Public API', uptime: '99.97%', detail: 'Up · 1d 12h 55m · Used on 1 status page', interval: '3m', state: 'operational' },
    { name: 'Database', uptime: '100%', detail: 'Up · 10d 22h 40m · Used on 1 status page', interval: '3m', state: 'operational' },
    { name: 'Transactional email', uptime: '99.95%', detail: 'Up · 4d 08h 12m · Used on 1 status page', interval: '5m', state: 'operational' }
  ];
  const notifications = [
    { name: 'Operations Telegram', type: 'Telegram', usage: '3 monitors', state: 'Enabled' },
    { name: 'Incident email', type: 'Email', usage: '1 monitor', state: 'Enabled' }
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
  <title>Monitors — Znayu</title>
  <meta name="description" content="Review monitors in the Znayu admin console." />
</svelte:head>

<main class:sidebar-collapsed={sidebarCollapsed} class="monitor-page">
  <aside class="sidebar" aria-label="Operator navigation">
    <div class="sidebar-top"><a class="wordmark" href="/dashboard" aria-label="Znayu operator dashboard">znayu</a><span class="console-label">Console</span><button class="sidebar-toggle" type="button" aria-label={sidebarCollapsed ? 'Expand sidebar' : 'Collapse sidebar'} title={sidebarCollapsed ? 'Expand sidebar' : 'Collapse sidebar'} onclick={toggleSidebar}><svg aria-hidden="true" viewBox="0 0 20 20"><rect x="3.5" y="3.5" width="13" height="13" rx="2" /><path d="M8 3.5v13" />{#if sidebarCollapsed}<path d="m10.5 7 3 3-3 3" />{:else}<path d="m12.5 7-3 3 3 3" />{/if}</svg></button></div>
    <div class="create-actions">
      <a class="sidebar-create" href="/monitors/new"><span aria-hidden="true">+</span> Create monitor</a>
      <a class="sidebar-create secondary" href="/status-pages/new">Create status page</a>
    </div>
    <nav class="sidebar-nav" aria-label="Console sections">
      <a href="/dashboard"><svg aria-hidden="true" viewBox="0 0 20 20"><path d="M3 10h5V3H3v7Zm9 7h5v-7h-5v7ZM3 17h5v-4H3v4Zm9-14v4h5V3h-5Z" /></svg> Overview</a>
      <a href="/status-pages"><svg aria-hidden="true" viewBox="0 0 20 20"><path d="M3 4.5h14v11H3v-11Zm3 3h8M6 11h5" /></svg> Status pages</a>
      <a class="active" href="/monitors" aria-current="page"><svg aria-hidden="true" viewBox="0 0 20 20"><path d="M3 15.5V4.5h14v11H3Zm3-3 2-2 2 1.5 3-4 2 2.5" /></svg> Monitors</a>
      <a href="/dashboard#activity"><svg aria-hidden="true" viewBox="0 0 20 20"><path d="M10 3v7l4 2M17 10a7 7 0 1 1-14 0 7 7 0 0 1 14 0Z" /></svg> Activity</a>
    </nav>
    <div class="sidebar-bottom"><div class="operator"><span class="operator-avatar" aria-hidden="true">JS</span><span><strong>Jordan Smith</strong><small>Owner</small></span><button type="button">More</button></div></div>
  </aside>

  <section class="monitor-content" aria-labelledby="monitor-title">
    <header class="content-header">
      <div><p class="section-kicker">Admin console</p><h1 id="monitor-title">Monitors <span class="info-dot" title="Monitors check the health of your services.">i</span></h1></div>
      <div class="header-actions"><label class="search-wrap"><span class="search-icon" aria-hidden="true"></span><span class="sr-only">Search monitors</span><input type="search" placeholder="Search monitors" /></label><div class="create-actions-inline"><a class="create-button secondary" href="/notifications/new">Create notification</a><a class="create-button" href="/monitors/new">Create monitor</a></div></div>
    </header>

    <details class="monitor-register" open>
      <summary class="register-header"><h2 id="register-title"><svg class="section-chevron" aria-hidden="true" viewBox="0 0 16 16"><path d="m3.5 6 4.5 4 4.5-4" /></svg><svg class="section-item-icon" aria-hidden="true" viewBox="0 0 20 20"><path d="M3 15.5V4.5h14v11H3Zm3-3 2-2 2 1.5 3-4 2 2.5" /></svg> Monitors</h2><span>{monitors.length} configured</span></summary>
      <div class="register-rows">
        {#each monitors as monitor}
          <article class="monitor-row">
            <span class="state-dot {monitor.state}" aria-label={monitor.state}></span>
            <div class="monitor-identity"><h3>{monitor.name}</h3><p><strong>{monitor.state === 'operational' ? 'Up' : 'Degraded'}</strong> · {monitor.detail.replace('Up · ', '')}</p></div>
            <div class="monitor-interval"><span class="pulse-icon" aria-hidden="true"></span><span>{monitor.interval}</span></div>
            <button class="row-menu" type="button" aria-label={`Actions for ${monitor.name}`}>More</button>
          </article>
        {/each}
      </div>
    </details>

    <details class="notification-register" open>
      <summary class="register-header"><h2 id="notification-register-title"><svg class="section-chevron" aria-hidden="true" viewBox="0 0 16 16"><path d="m3.5 6 4.5 4 4.5-4" /></svg><svg class="section-item-icon" aria-hidden="true" viewBox="0 0 16 16"><path d="M3 5.5h10v7H3v-7Zm2-2h6M5 8.5h6M5 11h4" /></svg> Notifications</h2><span>{notifications.length} configured</span></summary>
      <div class="notification-rows">
        {#each notifications as notification}
          <article class="notification-row"><span class="notification-state" aria-label={notification.state}></span><div class="notification-identity"><h3>{notification.name}</h3><p>{notification.type} · Used by {notification.usage}</p></div><span class="notification-status">{notification.state}</span><a class="notification-edit" href={`/notifications/new?edit=${notification.name.toLowerCase().replaceAll(' ', '-')}`}>Edit</a></article>
        {/each}
      </div>
    </details>
  </section>
</main>

<style>
  .monitor-page { min-height: 100svh; display: grid; grid-template-columns: 232px minmax(0, 1fr); background: var(--page); color: var(--ink); }
  .sidebar { position: sticky; top: 0; display: flex; height: 100svh; min-height: 100svh; padding: 22px 14px 16px; flex-direction: column; background: #141821; border-right: 1px solid var(--border); }.sidebar-top { display: flex; align-items: baseline; justify-content: space-between; padding: 0 10px; }.wordmark { color: var(--ink); font-size: 1.65rem; font-weight: 720; letter-spacing: -0.045em; line-height: 1; text-decoration: none; }.console-label, .section-kicker, .operator small { color: var(--muted); font-size: 0.6875rem; line-height: 1.4; }.console-label, .section-kicker { letter-spacing: 0.02em; text-transform: uppercase; }
  .sidebar-create { display: flex; align-items: center; justify-content: center; gap: 7px; min-height: 38px; margin: 38px 0 28px; border: 1px solid var(--green); border-radius: 5px; color: var(--page); background: var(--green); font-size: 0.8125rem; font-weight: 620; text-decoration: none; }.sidebar-create span { font-size: 1rem; line-height: 0; }.sidebar-nav { display: grid; gap: 3px; }.sidebar-nav a { display: flex; align-items: center; gap: 11px; padding: 10px; border-radius: 5px; color: var(--secondary); font-size: 0.8125rem; text-decoration: none; }.sidebar-nav a:hover, .sidebar-nav a.active { color: var(--ink); background: var(--surface-hover); }.sidebar-nav svg { display: block; width: 16px; height: 16px; flex: 0 0 auto; fill: none; stroke: currentColor; stroke-linecap: round; stroke-linejoin: round; stroke-width: 1.5; }.sidebar-nav a:first-child svg { fill: currentColor; stroke: none; }.sidebar-bottom { display: grid; gap: 20px; margin-top: auto; }.public-link { padding: 0 10px; color: var(--secondary); font-size: 0.6875rem; text-underline-offset: 3px; }.operator { display: flex; align-items: center; gap: 9px; padding: 12px 8px 0; border-top: 1px solid var(--border); }.operator-avatar { display: grid; width: 28px; height: 28px; place-items: center; border-radius: 50%; color: var(--page); background: var(--green); font-size: 0.6875rem; font-weight: 700; }.operator > span:nth-child(2) { display: grid; min-width: 0; gap: 2px; }.operator strong { overflow: hidden; color: var(--secondary); font-size: 0.6875rem; font-weight: 600; text-overflow: ellipsis; white-space: nowrap; }.operator button { margin-left: auto; border: 0; color: var(--muted); background: transparent; font-size: 0.6875rem; cursor: pointer; }
  .monitor-content { width: min(100%, 1080px); margin: 0 auto; padding: 84px 44px 64px; }.content-header { display: flex; align-items: flex-end; justify-content: space-between; gap: 24px; margin-bottom: 32px; }.section-kicker { margin: 0 0 7px; }h1, h2, h3, p { margin: 0; }h1 { font-size: 1.65rem; font-weight: 680; letter-spacing: -0.03em; line-height: 1.25; }.info-dot { display: inline-grid; width: 15px; height: 15px; margin-left: 4px; place-items: center; border-radius: 50%; color: var(--page); background: var(--muted); font-size: 0.6875rem; font-weight: 700; vertical-align: 3px; }.header-actions { display: flex; align-items: center; gap: 10px; }.search-wrap { position: relative; display: block; width: 280px; }.search-wrap input { width: 100%; height: 42px; padding: 0 12px 0 36px; border: 1px solid var(--border-light); border-radius: 5px; color: var(--ink); background: var(--surface); font: inherit; font-size: 0.8125rem; }.search-wrap input::placeholder { color: var(--muted); }.search-icon { position: absolute; top: 13px; left: 13px; width: 13px; height: 13px; border: 2px solid var(--muted); border-radius: 50%; }.search-icon::after { position: absolute; right: -5px; bottom: -3px; width: 5px; height: 2px; background: var(--muted); content: ''; transform: rotate(45deg); }.create-button { display: inline-flex; min-height: 42px; align-items: center; padding: 0 17px; border: 1px solid var(--green); border-radius: 5px; color: var(--page); background: var(--green); font-size: 0.8125rem; font-weight: 650; text-decoration: none; white-space: nowrap; }.create-button:hover, .sidebar-create:hover { filter: brightness(1.08); }
  .monitor-register { overflow: hidden; border: 1px solid var(--border); border-radius: 12px; background: var(--surface); }.register-header { display: flex; align-items: center; justify-content: space-between; min-height: 54px; padding: 0 20px; border-bottom: 1px solid var(--border); }.register-header h2 { display: flex; align-items: center; gap: 6px; color: var(--secondary); font-size: 0.8125rem; font-weight: 600; }.register-header h2 svg { width: 14px; height: 14px; fill: none; stroke: var(--muted); stroke-linecap: round; stroke-linejoin: round; stroke-width: 1.5; }.register-header > span { color: var(--muted); font-size: 0.6875rem; }.monitor-row { display: grid; grid-template-columns: 14px minmax(0, 1fr) auto 42px; align-items: center; gap: 12px; min-height: 74px; padding: 12px 20px; }.monitor-row + .monitor-row { border-top: 1px solid var(--border); }.state-dot { width: 8px; height: 8px; border-radius: 50%; background: var(--green); }.state-dot.degraded { background: var(--amber); }.monitor-identity { min-width: 0; }.monitor-identity h3 { overflow: hidden; font-size: 0.8125rem; font-weight: 620; line-height: 1.4; text-overflow: ellipsis; white-space: nowrap; }.monitor-identity p { margin-top: 3px; overflow: hidden; color: var(--muted); font-size: 0.8125rem; line-height: 1.4; text-overflow: ellipsis; white-space: nowrap; }.monitor-identity strong { color: var(--green); font-weight: 520; }.monitor-interval { display: flex; align-items: center; gap: 8px; color: var(--muted); font-size: 0.8125rem; }.pulse-icon { width: 16px; height: 16px; border: 2px solid var(--muted); border-radius: 50%; box-shadow: inset 0 0 0 3px var(--surface); }.row-menu { border: 0; color: var(--muted); background: transparent; font-size: 0.6875rem; cursor: pointer; }.row-menu:hover { color: var(--ink); }.sr-only { position: absolute; width: 1px; height: 1px; padding: 0; margin: -1px; overflow: hidden; clip: rect(0, 0, 0, 0); white-space: nowrap; border: 0; }:focus-visible { outline: 2px solid var(--green); outline-offset: 3px; }
  @media (max-width: 860px) { .monitor-content { padding: 48px 28px 48px; }.content-header { align-items: flex-start; flex-direction: column; margin-bottom: 32px; }.header-actions { width: 100%; }.search-wrap { flex: 1; width: auto; } }
  @media (max-width: 680px) { .monitor-page { display: block; }.sidebar { position: static; height: auto; min-height: auto; padding: 14px 12px 12px; border-right: 0; border-bottom: 1px solid var(--border); }.sidebar-top { padding: 0 4px; }.sidebar-create { margin: 22px 0 14px; }.sidebar-nav { display: flex; overflow-x: auto; }.sidebar-nav a { flex: 0 0 auto; }.sidebar-bottom { display: none; }.monitor-content { padding: 28px 14px 40px; }.header-actions { align-items: stretch; flex-direction: column; }.search-wrap { width: 100%; }.create-button { justify-content: center; }.monitor-interval { display: none; } }
  @media (max-width: 440px) { .monitor-row { grid-template-columns: 10px minmax(0, 1fr) 42px; padding-right: 14px; padding-left: 14px; }.row-menu { grid-column: 3; grid-row: 1; }.monitor-identity p { font-size: 0.6875rem; } }
  .create-actions { display: grid; gap: 8px; margin: 38px 0 28px; }
  .sidebar-create { margin: 0; }
  .sidebar-create.secondary { border-color: var(--border-light); color: var(--secondary); background: transparent; }
  @media (max-width: 680px) { .create-actions { margin: 22px 0 14px; } }
  .sidebar-toggle { display: grid; width: 24px; height: 24px; flex: 0 0 auto; place-items: center; padding: 0; border: 1px solid var(--border-light); border-radius: 5px; color: var(--muted); background: transparent; cursor: pointer; }
  .sidebar-toggle:hover { color: var(--ink); border-color: var(--secondary); }
  .sidebar-toggle svg { width: 14px; height: 14px; fill: none; stroke: currentColor; stroke-linecap: round; stroke-linejoin: round; stroke-width: 1.5; }
  .sidebar-collapsed.monitor-page { grid-template-columns: 72px minmax(0, 1fr); }
  .sidebar-collapsed .sidebar { padding-right: 10px; padding-left: 10px; }
  .sidebar-collapsed .sidebar-top { align-items: center; flex-direction: column; gap: 12px; padding: 0; }
  .sidebar-collapsed .wordmark { font-size: 0; }
  .sidebar-collapsed .wordmark::after { color: var(--ink); content: 'z'; font-size: 1.65rem; font-weight: 720; letter-spacing: -0.045em; }
  .sidebar-collapsed .console-label { display: none; }
  .sidebar-create span { display: none; }
  .sidebar-collapsed .sidebar-create span { display: inline; font-size: 1rem; line-height: 0; }
  .sidebar-collapsed .sidebar-create span { display: grid; width: 18px; height: 18px; place-items: center; font-size: 1.25rem; line-height: 1; }
  .sidebar-collapsed .sidebar-create span { position: relative; display: block; width: 14px; height: 14px; font-size: 0; }
  .sidebar-collapsed .sidebar-create { gap: 0; }
  .sidebar-collapsed .sidebar-create span::before, .sidebar-collapsed .sidebar-create span::after { position: absolute; top: 6px; left: 1px; width: 12px; height: 2px; border-radius: 1px; background: currentColor; content: ''; }
  .sidebar-collapsed .sidebar-create span::after { transform: rotate(90deg); }
  .sidebar-collapsed .create-actions { justify-items: center; }
  .sidebar-collapsed .sidebar-create { width: 40px; padding: 0; font-size: 0; }
  .sidebar-collapsed .sidebar-create.secondary { display: none; }
  .sidebar-collapsed .sidebar-nav a { justify-content: center; gap: 0; padding-right: 10px; padding-left: 10px; font-size: 0; }
  .sidebar-collapsed .sidebar-nav svg { width: 18px; height: 18px; }
  .sidebar-collapsed .public-link { display: none; }
  .sidebar-collapsed .operator { justify-content: center; padding-right: 0; padding-left: 0; }
  .sidebar-collapsed .operator > span:nth-child(2), .sidebar-collapsed .operator button { display: none; }
  @media (max-width: 680px) { .sidebar-toggle { display: none; }.sidebar-collapsed.monitor-page { display: block; }.sidebar-collapsed .sidebar { padding-right: 12px; padding-left: 12px; }.sidebar-collapsed .sidebar-top { align-items: baseline; flex-direction: row; gap: 0; padding: 0 4px; }.sidebar-collapsed .wordmark { font-size: 1.65rem; }.sidebar-collapsed .wordmark::after, .sidebar-collapsed .console-label { display: none; }.sidebar-collapsed .sidebar-create { width: auto; padding: 0 12px; font-size: 0.8125rem; }.sidebar-collapsed .sidebar-create.secondary { display: flex; }.sidebar-collapsed .sidebar-nav a { justify-content: flex-start; gap: 11px; padding-right: 10px; padding-left: 10px; font-size: 0.8125rem; }.sidebar-collapsed .operator { justify-content: flex-start; padding-right: 8px; padding-left: 8px; }.sidebar-collapsed .operator > span:nth-child(2), .sidebar-collapsed .operator button { display: initial; } }
  .create-actions-inline { display: flex; align-items: center; gap: 8px; }
  .create-button.secondary { border-color: var(--border-light); color: var(--secondary); background: transparent; }
  .create-button.secondary:hover { color: var(--ink); border-color: var(--secondary); filter: none; }
  .notification-register { overflow: hidden; margin-top: 24px; border: 1px solid var(--border); border-radius: 12px; background: var(--surface); }
  .notification-row { display: grid; grid-template-columns: 14px minmax(0, 1fr) auto 42px; align-items: center; gap: 12px; min-height: 68px; padding: 12px 20px; }
  .notification-row + .notification-row { border-top: 1px solid var(--border); }
  .notification-state { width: 8px; height: 8px; border-radius: 50%; background: var(--green); }
  .notification-identity { min-width: 0; }
  .notification-identity h3 { overflow: hidden; font-size: 0.8125rem; font-weight: 620; line-height: 1.4; text-overflow: ellipsis; white-space: nowrap; }
  .notification-identity p { margin-top: 3px; overflow: hidden; color: var(--muted); font-size: 0.75rem; text-overflow: ellipsis; white-space: nowrap; }
  .notification-status { color: var(--green); font-size: 0.75rem; }
  .notification-edit { color: var(--muted); font-size: 0.6875rem; text-decoration: none; }
  .notification-edit:hover { color: var(--ink); }
  .monitor-register > summary, .notification-register > summary { list-style: none; cursor: pointer; }
  .monitor-register > summary::-webkit-details-marker, .notification-register > summary::-webkit-details-marker { display: none; }
  .register-header h2 svg { transition: transform 140ms ease-out; }
  .register-header h2 .section-item-icon { transition: none; }
  .monitor-register:not([open]) .register-header h2 .section-chevron, .notification-register:not([open]) .register-header h2 .section-chevron { transform: rotate(-90deg); }
  .register-header:focus-visible { outline: 2px solid var(--green); outline-offset: -2px; }
  @media (max-width: 440px) { .notification-row { grid-template-columns: 10px minmax(0, 1fr) auto; padding-right: 14px; padding-left: 14px; }.notification-edit { grid-column: 3; grid-row: 1; }.notification-status { grid-column: 2; grid-row: 2; font-size: 0.6875rem; } }
  @media (max-width: 680px) { .create-actions-inline { display: grid; grid-template-columns: 1fr 1fr; } }
</style>
