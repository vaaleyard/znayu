<svelte:head>
  <title>Create monitor — Znayu</title>
  <meta name="description" content="Create a monitor in the Znayu operator console." />
</svelte:head>

<script lang="ts">
  import { onMount } from 'svelte';

  let sidebarCollapsed = false;
  let activeTab: 'general' | 'notifications' = 'general';
  let notificationType = 'Telegram';
  let showBotToken = false;

  onMount(() => {
    sidebarCollapsed = localStorage.getItem('znayu.sidebar.collapsed') === 'true';
  });

  function toggleSidebar() {
    sidebarCollapsed = !sidebarCollapsed;
    localStorage.setItem('znayu.sidebar.collapsed', String(sidebarCollapsed));
  }
</script>

<main class:sidebar-collapsed={sidebarCollapsed} class="create-page">
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
    <div class="sidebar-bottom"><a href="/status/acme-cloud" class="public-link">View public status</a><div class="operator"><span class="operator-avatar" aria-hidden="true">JS</span><span><strong>Jordan Smith</strong><small>Owner</small></span><button type="button">More</button></div></div>
  </aside>
  <header class="create-header">
    <a class="back-link" href="/monitors"><svg aria-hidden="true" viewBox="0 0 16 16"><path d="M10.5 3.5 6 8l4.5 4.5M6.5 8H13" /></svg> Back to monitors</a>
    <div>
      <p class="section-kicker section-kicker-spacer" aria-hidden="true">Operator console</p>
      <h1 id="create-title">Add new monitor <span class="info-dot" title="Configure the check that will represent one service in your status pages." aria-label="Configure the check that will represent one service in your status pages.">i</span></h1>
    </div>
  </header>

  <section class="create-shell" aria-labelledby="create-title">
    <div class="create-heading" hidden><p>Operator console</p><h1>New monitor</h1><span>Configuration</span></div>
    <form onsubmit={(event) => event.preventDefault()}>
      <div class="form-tabs" role="tablist" aria-label="Monitor sections">
        <button class:active={activeTab === 'general'} type="button" role="tab" aria-selected={activeTab === 'general'} onclick={() => activeTab = 'general'}>General</button>
        <button class:active={activeTab === 'notifications'} type="button" role="tab" aria-selected={activeTab === 'notifications'} onclick={() => activeTab = 'notifications'}>Notifications</button>
      </div>
      {#if activeTab === 'general'}
      <div class="form-columns">
        <section class="form-section" aria-labelledby="general-title">
          <h2 id="general-title">General</h2>
          <label for="monitor-type">Monitor type</label><select id="monitor-type"><option>HTTP(s)</option><option>TCP</option><option>Ping</option></select>
          <label for="monitor-name">Name</label><input id="monitor-name" type="text" placeholder="New monitor" />
          <label for="monitor-url">URL</label><input id="monitor-url" type="url" placeholder="https://" />
          <label for="heartbeat">Heartbeat interval <span>(seconds)</span></label><input id="heartbeat" type="number" value="60" /><small>Checks once every minute.</small>
          <label for="retries">Retry attempts</label><input id="retries" type="number" value="0" /><small>Attempts before the service is marked inactive.</small>
          <label for="timeout">Request timeout <span>(seconds)</span></label><input id="timeout" type="number" value="48" />
        </section>
        <div class="right-column">
          <section class="form-section" aria-labelledby="http-title"><h2 id="http-title">HTTP options</h2><label for="method">Method</label><select id="method"><option>GET</option><option>POST</option><option>HEAD</option></select><label for="encoding">Body encoding</label><select id="encoding"><option>JSON</option><option>Form data</option><option>Plain text</option></select><label for="body">Body <span>(optional)</span></label><textarea id="body" rows="6" placeholder={'Example:\n{\n  "key": "value"\n}'}></textarea></section>
        </div>
      </div>
      {:else}
      <section class="notifications-panel" aria-labelledby="notifications-title"><div><h2 id="notifications-title">Configure notification</h2><p>Choose where Znayu should send alerts when this monitor changes state.</p></div><div class="notification-fields"><label for="notification-type">Notification type<select id="notification-type" bind:value={notificationType}><option>Telegram</option><option>Discord</option><option>Email</option><option>Webhook</option></select></label><label for="notification-alias">Alias<input id="notification-alias" type="text" placeholder="My Telegram Alert" /></label><label for="bot-token">Bot token<div class="secret-input"><input id="bot-token" type={showBotToken ? 'text' : 'password'} /><button type="button" aria-label={showBotToken ? 'Hide bot token' : 'Show bot token'} onclick={() => showBotToken = !showBotToken}>{showBotToken ? 'Hide' : 'Show'}</button></div></label><small>You can get the bot token from <a href="https://t.me/BotFather" target="_blank" rel="noreferrer">BotFather</a>.</small><label for="chat-id">Chat ID<input id="chat-id" type="text" /></label><small>Supports a direct chat, group, or channel ID.</small><label for="thread-id">(Optional) Thread ID<input id="thread-id" type="text" /></label><small>Only needed for forum topics.</small><label for="server-url">(Optional) Server URL<input id="server-url" type="url" value="https://api.telegram.org" /></label></div><div class="notification-toggles"><label><input type="checkbox" /><span>Send silently</span></label><small>Users will not receive a sound notification.</small><label><input type="checkbox" /><span>Protect against forwarding and saving</span></label><small>Protect messages sent by the Telegram bot.</small><hr /><label><input type="checkbox" checked /><span>Enabled by default</span></label><small>Enable this notification for new monitors automatically.</small><label><input type="checkbox" /><span>Apply to all existing monitors</span></label></div><div class="notification-actions"><button type="button" class="test-action">Test</button><button type="button" class="notification-save">Save notification</button></div></section>
      {/if}
      {#if activeTab === 'general'}<footer class="form-footer"><p>Configuration is only previewed in this prototype.</p><button class="save-button" type="submit">Save monitor</button></footer>{/if}
    </form>
  </section>
</main>

<style>
  .create-page { min-height: 100svh; display: grid; grid-template-columns: 232px minmax(0, 1fr); padding: 24px 0 48px; background: var(--page); color: var(--ink); }.sidebar { position: sticky; top: -24px; display: flex; height: calc(100svh + 24px); min-height: 100svh; margin-top: -24px; padding: 22px 14px 16px; flex-direction: column; background: #141821; border-right: 1px solid var(--border); }.sidebar-top { display: flex; align-items: baseline; justify-content: space-between; padding: 0 10px; }.wordmark { color: var(--ink); font-size: 1.65rem; font-weight: 720; letter-spacing: -0.045em; line-height: 1; text-decoration: none; }.console-label, .section-kicker, .operator small { color: var(--muted); font-size: 0.6875rem; line-height: 1.4; }.console-label, .section-kicker { letter-spacing: 0.02em; text-transform: uppercase; }.sidebar-create { display: flex; align-items: center; justify-content: center; gap: 7px; min-height: 38px; margin: 38px 0 28px; border: 1px solid var(--green); border-radius: 5px; color: var(--page); background: var(--green); font-size: 0.8125rem; font-weight: 620; text-decoration: none; }.sidebar-create span { font-size: 1rem; line-height: 0; }.sidebar-nav { display: grid; gap: 3px; }.sidebar-nav a { display: flex; align-items: center; gap: 11px; padding: 10px; border-radius: 5px; color: var(--secondary); font-size: 0.8125rem; text-decoration: none; }.sidebar-nav a:hover, .sidebar-nav a.active { color: var(--ink); background: var(--surface-hover); }.sidebar-nav svg { display: block; width: 16px; height: 16px; flex: 0 0 auto; fill: none; stroke: currentColor; stroke-linecap: round; stroke-linejoin: round; stroke-width: 1.5; }.sidebar-nav a:first-child svg { fill: currentColor; stroke: none; }.sidebar-bottom { display: grid; gap: 20px; margin-top: auto; }.public-link { padding: 0 10px; color: var(--secondary); font-size: 0.6875rem; text-underline-offset: 3px; }.operator { display: flex; align-items: center; gap: 9px; padding: 12px 8px 0; border-top: 1px solid var(--border); }.operator-avatar { display: grid; width: 28px; height: 28px; place-items: center; border-radius: 50%; color: var(--page); background: var(--green); font-size: 0.6875rem; font-weight: 700; }.operator > span:nth-child(2) { display: grid; min-width: 0; gap: 2px; }.operator strong { overflow: hidden; color: var(--secondary); font-size: 0.6875rem; font-weight: 600; text-overflow: ellipsis; white-space: nowrap; }.operator button { margin-left: auto; border: 0; color: var(--muted); background: transparent; font-size: 0.6875rem; cursor: pointer; }.create-header, .create-shell { width: min(100%, 1080px); margin: 0 auto; }.create-header { padding-top: 60px; }.create-header h1 { margin: 0; font-size: 1.65rem; font-weight: 680; letter-spacing: -0.03em; line-height: 1.25; }.create-header .section-kicker { margin: 0 0 7px; }.create-shell { margin-top: 32px; overflow: hidden; border: 1px solid var(--border); border-radius: 12px; background: var(--surface); box-shadow: 0 4px 7px rgb(0 0 0 / 3%); }.create-heading { padding: 30px 30px 26px; border-bottom: 1px solid var(--border); }.create-heading p { margin-bottom: 7px; color: var(--muted); font-size: 0.6875rem; letter-spacing: 0.02em; text-transform: uppercase; }.create-heading h1 { font-size: 1.65rem; font-weight: 680; letter-spacing: -0.03em; line-height: 1.25; }.create-heading span { display: block; margin-top: 8px; color: var(--secondary); font-size: 0.8125rem; line-height: 1.5; }.form-columns { display: grid; grid-template-columns: minmax(0, 1fr) minmax(0, 0.9fr); }.form-section { padding: 28px 30px 34px; }.right-column { border-left: 1px solid var(--border); }.right-column .form-section + .form-section { border-top: 1px solid var(--border); }.form-section h2 { margin-bottom: 24px; font-size: 1rem; font-weight: 700; line-height: 1.35; }.form-section label { display: block; margin-top: 18px; color: var(--secondary); font-size: 0.8125rem; font-weight: 600; line-height: 1.4; }.form-section label:first-of-type { margin-top: 0; }.form-section label span { color: var(--muted); font-weight: 400; }.form-section input, .form-section select, .form-section textarea { display: block; width: 100%; margin-top: 7px; border: 1px solid var(--border-light); border-radius: 5px; color: var(--ink); background: var(--page); font: inherit; font-size: 0.8125rem; }.form-section input, .form-section select { height: 42px; padding: 0 12px; }.form-section textarea { min-height: 128px; padding: 12px; resize: vertical; line-height: 1.5; }.form-section input::placeholder, .form-section textarea::placeholder { color: var(--muted); }.form-section input:focus, .form-section select:focus, .form-section textarea:focus { border-color: var(--green); outline: 2px solid rgb(16 185 129 / 18%); outline-offset: 1px; }.form-section small { display: block; margin-top: 6px; color: var(--muted); font-size: 0.6875rem; line-height: 1.4; }.empty-setting { color: var(--secondary); font-size: 0.8125rem; line-height: 1.5; }.secondary-action { min-height: 36px; margin-top: 16px; padding: 0 14px; border: 1px solid var(--border-light); border-radius: 5px; color: var(--secondary); background: transparent; font-size: 0.8125rem; cursor: pointer; }.secondary-action:hover { color: var(--ink); border-color: var(--secondary); }.form-footer { display: flex; align-items: center; justify-content: space-between; gap: 20px; padding: 14px 18px; border-top: 1px solid var(--border); background: rgb(15 18 26 / 50%); }.form-footer p { color: var(--muted); font-size: 0.6875rem; line-height: 1.4; }.save-button { min-height: 38px; padding: 0 18px; border: 1px solid var(--green); border-radius: 5px; color: var(--page); background: var(--green); font-size: 0.8125rem; font-weight: 650; cursor: pointer; }.save-button:hover { filter: brightness(1.08); }:focus-visible { outline: 2px solid var(--green); outline-offset: 3px; }
  .create-page > .sidebar { top: 0; }
  .create-page > .sidebar { grid-row: 1 / span 2; }
  .create-header, .create-shell { grid-column: 2; }
  @media (max-width: 760px) { .create-page { display: block; padding: 18px 14px 36px; }.sidebar { position: static; height: auto; min-height: auto; margin: -18px -14px 0; padding: 14px 12px 12px; border-right: 0; border-bottom: 1px solid var(--border); }.sidebar-top { padding: 0 4px; }.sidebar-create { margin: 22px 0 14px; }.sidebar-nav { display: flex; overflow-x: auto; }.sidebar-nav a { flex: 0 0 auto; }.sidebar-bottom { display: none; }.create-header { padding-top: 0; }.create-shell { margin-top: 36px; }.form-columns { display: block; }.right-column { border-top: 1px solid var(--border); border-left: 0; }.form-section { padding: 24px 20px 28px; } }
  @media (max-width: 440px) { .create-header { align-items: flex-start; flex-direction: column; gap: 12px; }.create-heading { padding: 24px 20px 22px; }.create-heading h1 { font-size: 1rem; }.form-footer { align-items: flex-start; flex-direction: column-reverse; }.save-button { width: 100%; } }
  .create-actions { display: grid; gap: 8px; margin: 38px 0 28px; }
  .sidebar-create { margin: 0; }
  .sidebar-create.secondary { border-color: var(--border-light); color: var(--secondary); background: transparent; }
  @media (max-width: 760px) { .create-actions { margin: 22px 0 14px; } }
  .create-header, .create-shell { width: min(100%, 992px); }
  .create-header .info-dot { display: inline-grid; width: 15px; height: 15px; margin-left: 4px; place-items: center; border-radius: 50%; color: var(--page); background: var(--muted); font-size: 0.6875rem; font-weight: 700; vertical-align: 3px; }
  .section-kicker-spacer { visibility: hidden; }
  .create-shell { margin-top: 32px; }
  @media (max-width: 760px) { .create-shell { margin-top: 32px; } }
  .form-section select { appearance: none; padding-right: 38px; background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='14' height='14' viewBox='0 0 14 14'%3E%3Cpath d='m3 5 4 4 4-4' fill='none' stroke='%23a7acbb' stroke-linecap='round' stroke-linejoin='round' stroke-width='1.5'/%3E%3C/svg%3E"); background-position: right 12px center; background-repeat: no-repeat; background-size: 14px 14px; }
  .sidebar-toggle { display: grid; width: 24px; height: 24px; flex: 0 0 auto; place-items: center; padding: 0; border: 1px solid var(--border-light); border-radius: 5px; color: var(--muted); background: transparent; cursor: pointer; }
  .sidebar-toggle:hover { color: var(--ink); border-color: var(--secondary); }
  .sidebar-toggle svg { width: 14px; height: 14px; fill: none; stroke: currentColor; stroke-linecap: round; stroke-linejoin: round; stroke-width: 1.5; }
  .sidebar-collapsed.create-page { grid-template-columns: 72px minmax(0, 1fr); }
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
  @media (max-width: 760px) { .sidebar-toggle { display: none; }.sidebar-collapsed.create-page { display: block; }.sidebar-collapsed .sidebar { padding-right: 12px; padding-left: 12px; }.sidebar-collapsed .sidebar-top { align-items: baseline; flex-direction: row; gap: 0; padding: 0 4px; }.sidebar-collapsed .wordmark { font-size: 1.65rem; }.sidebar-collapsed .wordmark::after, .sidebar-collapsed .console-label { display: none; }.sidebar-collapsed .sidebar-create { width: auto; padding: 0 12px; font-size: 0.8125rem; }.sidebar-collapsed .sidebar-create.secondary { display: flex; }.sidebar-collapsed .sidebar-nav a { justify-content: flex-start; gap: 11px; padding-right: 10px; padding-left: 10px; font-size: 0.8125rem; }.sidebar-collapsed .operator { justify-content: flex-start; padding-right: 8px; padding-left: 8px; }.sidebar-collapsed .operator > span:nth-child(2), .sidebar-collapsed .operator button { display: initial; } }
  .create-page > .sidebar { height: 100svh; }
  .create-page > .sidebar { position: fixed; top: 0; left: 0; z-index: 10; width: 232px; margin-top: 0; }
  .create-header { display: flex; align-items: flex-start; justify-content: space-between; gap: 24px; }
  .back-link { display: inline-flex; min-height: 34px; align-items: center; gap: 7px; padding: 0 11px; border: 1px solid var(--border-light); border-radius: 5px; color: var(--secondary); background: var(--surface); font-size: 0.8125rem; text-decoration: none; }
  .back-link:hover { color: var(--ink); border-color: var(--secondary); background: var(--surface-hover); }
  .back-link svg { width: 15px; height: 15px; fill: none; stroke: currentColor; stroke-linecap: round; stroke-linejoin: round; stroke-width: 1.5; }
  .create-header { position: relative; display: block; }
  .create-header .back-link { position: absolute; top: 24px; left: 0; }
  @media (max-width: 440px) { .create-header { display: flex; }.create-header .back-link { position: static; } }
  .sidebar-collapsed.create-page > .sidebar { width: 72px; }
  @media (max-width: 760px) { .create-page > .sidebar, .sidebar-collapsed.create-page > .sidebar { position: static; width: auto; margin: -18px -14px 0; } }
  .form-tabs { display: flex; gap: 4px; padding: 0 24px; border-bottom: 1px solid var(--border); background: rgb(15 18 26 / 28%); }
  .form-tabs button { position: relative; min-height: 48px; padding: 0 12px; border: 0; color: var(--muted); background: transparent; font: inherit; font-size: 0.8125rem; font-weight: 620; cursor: pointer; }
  .form-tabs button::after { position: absolute; right: 10px; bottom: -1px; left: 10px; height: 2px; border-radius: 2px; background: transparent; content: ''; }
  .form-tabs button:hover, .form-tabs button.active { color: var(--ink); }
  .form-tabs button.active::after { background: var(--green); }
  .notifications-panel { display: grid; gap: 24px; min-height: 280px; padding: 30px; }
  .notifications-panel h2 { margin: 0; font-size: 1rem; font-weight: 700; }
  .notifications-panel p { margin: 8px 0 0; color: var(--secondary); font-size: 0.8125rem; }
  .notification-fields { display: grid; gap: 15px; width: 100%; max-width: none; }
  .notification-fields label { display: grid; gap: 7px; color: var(--secondary); font-size: 0.8125rem; font-weight: 600; }
  .notification-fields input, .notification-fields select { width: 100%; height: 42px; padding: 0 12px; border: 1px solid var(--border-light); border-radius: 5px; color: var(--ink); background: var(--page); font: inherit; font-size: 0.8125rem; }
  .notification-fields select { appearance: none; padding-right: 38px; background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='14' height='14' viewBox='0 0 14 14'%3E%3Cpath d='m3 5 4 4 4-4' fill='none' stroke='%238a91a5' stroke-linecap='round' stroke-linejoin='round' stroke-width='1.5'/%3E%3C/svg%3E"); background-position: right 12px center; background-repeat: no-repeat; background-size: 14px 14px; }
  .notification-fields input:focus, .notification-fields select:focus { border-color: var(--green); outline: 2px solid rgb(16 185 129 / 18%); outline-offset: 1px; }
  .notification-fields small, .notification-toggles small { color: var(--muted); font-size: 0.6875rem; line-height: 1.45; }
  .notification-fields a { color: var(--secondary); text-underline-offset: 3px; }
  .secret-input { display: flex; }
  .secret-input input { border-radius: 5px 0 0 5px; }
  .secret-input button { width: 72px; height: 42px; border: 1px solid var(--border-light); border-left: 0; border-radius: 0 5px 5px 0; color: var(--secondary); background: var(--surface-hover); font: inherit; font-size: 0.6875rem; font-weight: 650; cursor: pointer; }
  .secret-input button:hover { color: var(--ink); background: var(--border); }
  .notification-toggles { display: grid; gap: 8px; width: 100%; max-width: none; margin-top: 22px; }
  .notification-toggles label { display: flex; align-items: center; gap: 10px; color: var(--secondary); font-size: 0.8125rem; cursor: pointer; }
  .notification-toggles input { width: 16px; height: 16px; margin: 0; accent-color: var(--green); }
  .notification-toggles label + small { margin-left: 26px; margin-top: -3px; }
  .notification-toggles hr { width: 100%; margin: 12px 0; border: 0; border-top: 1px solid var(--border); }
  .notification-actions { display: flex; justify-content: flex-end; gap: 10px; width: 100%; margin-top: 24px; }
  .notification-actions button { min-width: 136px; min-height: 38px; padding: 0 16px; border-radius: 5px; font: inherit; font-size: 0.8125rem; font-weight: 650; cursor: pointer; }
  .test-action { min-width: 88px !important; border: 1px solid var(--amber, #f59e0b); color: var(--page); background: var(--amber, #f59e0b); }
  .notification-save { border: 1px solid var(--green); color: var(--page); background: var(--green); }
  @media (max-width: 760px) { .form-tabs { flex-wrap: wrap; padding: 0 16px; }.notifications-panel { min-height: 240px; padding: 24px 20px 28px; }.notification-actions { justify-content: stretch; }.notification-actions button { flex: 1; } }
</style>
