<script lang="ts">
  type Monitor = { name: string; uptime: string; detail: string; interval: string; state: 'operational' | 'degraded' };

  const monitors: Monitor[] = [
    { name: 'Main website', uptime: '99.98%', detail: 'Up · 2d 54m · Used on 1 status page', interval: '3m', state: 'operational' },
    { name: 'Public API', uptime: '99.97%', detail: 'Up · 1d 12h 55m · Used on 1 status page', interval: '3m', state: 'operational' },
    { name: 'Database', uptime: '100%', detail: 'Up · 10d 22h 40m · Used on 1 status page', interval: '3m', state: 'operational' },
    { name: 'Transactional email', uptime: '99.95%', detail: 'Up · 4d 08h 12m · Used on 1 status page', interval: '5m', state: 'operational' }
  ];
</script>

<svelte:head>
  <title>Monitors — Znayu</title>
  <meta name="description" content="Review monitors in the Znayu operator console." />
</svelte:head>

<main class="monitor-page">
  <aside class="sidebar" aria-label="Operator navigation">
    <div class="sidebar-top"><a class="wordmark" href="/" aria-label="Znayu public status page">znayu</a><span class="console-label">Console</span></div>
    <a class="sidebar-create" href="/dashboard/monitors/new"><span aria-hidden="true">+</span> Create monitor</a>
    <nav class="sidebar-nav" aria-label="Console sections">
      <a href="/dashboard"><svg aria-hidden="true" viewBox="0 0 20 20"><path d="M3 10h5V3H3v7Zm9 7h5v-7h-5v7ZM3 17h5v-4H3v4Zm9-14v4h5V3h-5Z" /></svg> Overview</a>
      <a href="/dashboard#status-pages"><svg aria-hidden="true" viewBox="0 0 20 20"><path d="M3 4.5h14v11H3v-11Zm3 3h8M6 11h5" /></svg> Status pages</a>
      <a class="active" href="/dashboard/monitors" aria-current="page"><svg aria-hidden="true" viewBox="0 0 20 20"><path d="M3 15.5V4.5h14v11H3Zm3-3 2-2 2 1.5 3-4 2 2.5" /></svg> Monitors</a>
      <a href="/dashboard#activity"><svg aria-hidden="true" viewBox="0 0 20 20"><path d="M10 3v7l4 2M17 10a7 7 0 1 1-14 0 7 7 0 0 1 14 0Z" /></svg> Activity</a>
    </nav>
    <div class="sidebar-bottom"><a href="/" class="public-link">View public status</a><div class="operator"><span class="operator-avatar" aria-hidden="true">JS</span><span><strong>Jordan Smith</strong><small>Owner</small></span><button type="button">More</button></div></div>
  </aside>

  <section class="monitor-content" aria-labelledby="monitor-title">
    <header class="content-header">
      <div><p class="section-kicker">Operator console</p><h1 id="monitor-title">Monitors <span class="info-dot" title="Monitors check the health of your services.">i</span></h1></div>
      <div class="header-actions"><label class="search-wrap"><span class="search-icon" aria-hidden="true"></span><span class="sr-only">Search monitors</span><input type="search" placeholder="Search monitors" /></label><a class="create-button" href="/dashboard/monitors/new">Create monitor</a></div>
    </header>

    <section class="monitor-register" aria-labelledby="register-title">
      <header class="register-header"><h2 id="register-title"><svg aria-hidden="true" viewBox="0 0 16 16"><path d="m3 6 5 5 5-5" /></svg> Monitors</h2><span>{monitors.length} configured</span></header>
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
    </section>
  </section>
</main>

<style>
  .monitor-page { min-height: 100svh; display: grid; grid-template-columns: 232px minmax(0, 1fr); background: var(--page); color: var(--ink); }
  .sidebar { display: flex; min-height: 100svh; padding: 22px 14px 16px; flex-direction: column; background: #141821; border-right: 1px solid var(--border); }.sidebar-top { display: flex; align-items: baseline; justify-content: space-between; padding: 0 10px; }.wordmark { color: var(--ink); font-size: 1.65rem; font-weight: 720; letter-spacing: -0.045em; line-height: 1; text-decoration: none; }.console-label, .section-kicker, .operator small { color: var(--muted); font-size: 0.6875rem; line-height: 1.4; }.console-label, .section-kicker { letter-spacing: 0.02em; text-transform: uppercase; }
  .sidebar-create { display: flex; align-items: center; justify-content: center; gap: 7px; min-height: 38px; margin: 38px 0 28px; border: 1px solid var(--green); border-radius: 5px; color: var(--page); background: var(--green); font-size: 0.8125rem; font-weight: 620; text-decoration: none; }.sidebar-create span { font-size: 1rem; line-height: 0; }.sidebar-nav { display: grid; gap: 3px; }.sidebar-nav a { display: flex; align-items: center; gap: 11px; padding: 10px; border-radius: 5px; color: var(--secondary); font-size: 0.8125rem; text-decoration: none; }.sidebar-nav a:hover, .sidebar-nav a.active { color: var(--ink); background: var(--surface-hover); }.sidebar-nav svg { width: 16px; height: 16px; flex: 0 0 auto; fill: none; stroke: currentColor; stroke-linecap: round; stroke-linejoin: round; stroke-width: 1.5; }.sidebar-nav a:first-child svg { fill: currentColor; stroke: none; }.sidebar-bottom { display: grid; gap: 20px; margin-top: auto; }.public-link { padding: 0 10px; color: var(--secondary); font-size: 0.6875rem; text-underline-offset: 3px; }.operator { display: flex; align-items: center; gap: 9px; padding: 12px 8px 0; border-top: 1px solid var(--border); }.operator-avatar { display: grid; width: 28px; height: 28px; place-items: center; border-radius: 50%; color: var(--page); background: var(--green); font-size: 0.6875rem; font-weight: 700; }.operator > span:nth-child(2) { display: grid; min-width: 0; gap: 2px; }.operator strong { overflow: hidden; color: var(--secondary); font-size: 0.6875rem; font-weight: 600; text-overflow: ellipsis; white-space: nowrap; }.operator button { margin-left: auto; border: 0; color: var(--muted); background: transparent; font-size: 0.6875rem; cursor: pointer; }
  .monitor-content { width: min(100%, 1080px); margin: 0 auto; padding: 84px 44px 64px; }.content-header { display: flex; align-items: flex-end; justify-content: space-between; gap: 24px; margin-bottom: 32px; }.section-kicker { margin: 0 0 7px; }h1, h2, h3, p { margin: 0; }h1 { font-size: 1.65rem; font-weight: 680; letter-spacing: -0.03em; line-height: 1.25; }.info-dot { display: inline-grid; width: 15px; height: 15px; margin-left: 4px; place-items: center; border-radius: 50%; color: var(--page); background: var(--muted); font-size: 0.6875rem; font-weight: 700; vertical-align: 3px; }.header-actions { display: flex; align-items: center; gap: 10px; }.search-wrap { position: relative; display: block; width: 280px; }.search-wrap input { width: 100%; height: 42px; padding: 0 12px 0 36px; border: 1px solid var(--border-light); border-radius: 5px; color: var(--ink); background: var(--surface); font: inherit; font-size: 0.8125rem; }.search-wrap input::placeholder { color: var(--muted); }.search-icon { position: absolute; top: 13px; left: 13px; width: 13px; height: 13px; border: 2px solid var(--muted); border-radius: 50%; }.search-icon::after { position: absolute; right: -5px; bottom: -3px; width: 5px; height: 2px; background: var(--muted); content: ''; transform: rotate(45deg); }.create-button { display: inline-flex; min-height: 42px; align-items: center; padding: 0 17px; border: 1px solid var(--green); border-radius: 5px; color: var(--page); background: var(--green); font-size: 0.8125rem; font-weight: 650; text-decoration: none; white-space: nowrap; }.create-button:hover, .sidebar-create:hover { filter: brightness(1.08); }
  .monitor-register { overflow: hidden; border: 1px solid var(--border); border-radius: 12px; background: var(--surface); }.register-header { display: flex; align-items: center; justify-content: space-between; min-height: 54px; padding: 0 20px; border-bottom: 1px solid var(--border); }.register-header h2 { display: flex; align-items: center; gap: 6px; color: var(--secondary); font-size: 0.8125rem; font-weight: 600; }.register-header h2 svg { width: 14px; height: 14px; fill: none; stroke: var(--muted); stroke-linecap: round; stroke-linejoin: round; stroke-width: 1.5; }.register-header > span { color: var(--muted); font-size: 0.6875rem; }.monitor-row { display: grid; grid-template-columns: 14px minmax(0, 1fr) auto 42px; align-items: center; gap: 12px; min-height: 74px; padding: 12px 20px; }.monitor-row + .monitor-row { border-top: 1px solid var(--border); }.state-dot { width: 8px; height: 8px; border-radius: 50%; background: var(--green); }.state-dot.degraded { background: var(--amber); }.monitor-identity { min-width: 0; }.monitor-identity h3 { overflow: hidden; font-size: 0.8125rem; font-weight: 620; line-height: 1.4; text-overflow: ellipsis; white-space: nowrap; }.monitor-identity p { margin-top: 3px; overflow: hidden; color: var(--muted); font-size: 0.8125rem; line-height: 1.4; text-overflow: ellipsis; white-space: nowrap; }.monitor-identity strong { color: var(--green); font-weight: 520; }.monitor-interval { display: flex; align-items: center; gap: 8px; color: var(--muted); font-size: 0.8125rem; }.pulse-icon { width: 16px; height: 16px; border: 2px solid var(--muted); border-radius: 50%; box-shadow: inset 0 0 0 3px var(--surface); }.row-menu { border: 0; color: var(--muted); background: transparent; font-size: 0.6875rem; cursor: pointer; }.row-menu:hover { color: var(--ink); }.sr-only { position: absolute; width: 1px; height: 1px; padding: 0; margin: -1px; overflow: hidden; clip: rect(0, 0, 0, 0); white-space: nowrap; border: 0; }:focus-visible { outline: 2px solid var(--green); outline-offset: 3px; }
  @media (max-width: 860px) { .monitor-content { padding: 48px 28px 48px; }.content-header { align-items: flex-start; flex-direction: column; margin-bottom: 32px; }.header-actions { width: 100%; }.search-wrap { flex: 1; width: auto; } }
  @media (max-width: 680px) { .monitor-page { display: block; }.sidebar { min-height: auto; padding: 14px 12px 12px; border-right: 0; border-bottom: 1px solid var(--border); }.sidebar-top { padding: 0 4px; }.sidebar-create { margin: 22px 0 14px; }.sidebar-nav { display: flex; overflow-x: auto; }.sidebar-nav a { flex: 0 0 auto; }.sidebar-bottom { display: none; }.monitor-content { padding: 28px 14px 40px; }.header-actions { align-items: stretch; flex-direction: column; }.search-wrap { width: 100%; }.create-button { justify-content: center; }.monitor-interval { display: none; } }
  @media (max-width: 440px) { .monitor-row { grid-template-columns: 10px minmax(0, 1fr) 42px; padding-right: 14px; padding-left: 14px; }.row-menu { grid-column: 3; grid-row: 1; }.monitor-identity p { font-size: 0.6875rem; } }
</style>
