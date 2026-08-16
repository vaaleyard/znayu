<script lang="ts">
  import { page } from '$app/state';
  import { onMount } from 'svelte';

  let sidebarCollapsed = false;
  let activeTab: 'settings' | 'monitors' | 'maintenance' | 'status-updates' = 'settings';
  let selectedMonitors = ['acme-api', 'billing-api'];
  let affectedMonitors = ['acme-api'];
  let showMaintenanceForm = false;
  let maintenanceHistoryDays = '7';
  let showUpdateForm = false;
  let notifyUpdateSubscribers = true;
  $: editing = page.url.searchParams.has('edit');
  let maintenances = [
    { id: 'database', title: 'Database maintenance', services: 'Billing API · Acme Cloud API', status: 'Scheduled', tone: 'scheduled', time: 'Aug 24, 02:00–03:00 UTC' },
    { id: 'edge', title: 'Edge network upgrade', services: 'CDN edge', status: 'Completed', tone: 'completed', time: 'Aug 12, 01:00–01:30 UTC' }
  ];

  onMount(() => {
    sidebarCollapsed = localStorage.getItem('znayu.sidebar.collapsed') === 'true';
  });

  function toggleSidebar() {
    sidebarCollapsed = !sidebarCollapsed;
    localStorage.setItem('znayu.sidebar.collapsed', String(sidebarCollapsed));
  }

  function removeMaintenance(id: string) {
    maintenances = maintenances.filter((maintenance) => maintenance.id !== id);
  }
</script>

<svelte:head><title>{editing ? 'Edit status page' : 'Create status page'} — Znayu</title><meta name="description" content="Manage a public status page in the Znayu admin console." /></svelte:head>

<main class:sidebar-collapsed={sidebarCollapsed} class="status-create-page">
  <aside class="sidebar" aria-label="Operator navigation">
    <div class="sidebar-top"><a class="wordmark" href="/dashboard" aria-label="Znayu operator dashboard">znayu</a><span class="console-label">Console</span><button class="sidebar-toggle" type="button" aria-label={sidebarCollapsed ? 'Expand sidebar' : 'Collapse sidebar'} title={sidebarCollapsed ? 'Expand sidebar' : 'Collapse sidebar'} onclick={toggleSidebar}><svg aria-hidden="true" viewBox="0 0 20 20"><rect x="3.5" y="3.5" width="13" height="13" rx="2" /><path d="M8 3.5v13" />{#if sidebarCollapsed}<path d="m10.5 7 3 3-3 3" />{:else}<path d="m12.5 7-3 3 3 3" />{/if}</svg></button></div>
    <div class="create-actions"><a class="sidebar-create" href="/monitors/new"><span aria-hidden="true">+</span> Create monitor</a><a class="sidebar-create secondary" href="/status-pages/new">Create status page</a></div>
    <nav class="sidebar-nav" aria-label="Console sections"><a href="/dashboard"><svg aria-hidden="true" viewBox="0 0 20 20"><path d="M3 10h5V3H3v7Zm9 7h5v-7h-5v7ZM3 17h5v-4H3v4Zm9-14v4h5V3h-5Z" /></svg> Overview</a><a class="active" href="/status-pages" aria-current="page"><svg aria-hidden="true" viewBox="0 0 20 20"><path d="M3 4.5h14v11H3v-11Zm3 3h8M6 11h5" /></svg> Status pages</a><a href="/monitors"><svg aria-hidden="true" viewBox="0 0 20 20"><path d="M3 15.5V4.5h14v11H3Zm3-3 2-2 2 1.5 3-4 2 2.5" /></svg> Monitors</a><a href="/activity"><svg aria-hidden="true" viewBox="0 0 20 20"><path d="M10 3v7l4 2M17 10a7 7 0 1 1-14 0 7 7 0 0 1 14 0Z" /></svg> Activity</a></nav>
    <div class="sidebar-bottom"><div class="operator"><span class="operator-avatar" aria-hidden="true">JS</span><span><strong>Jordan Smith</strong><small>Owner</small></span><button type="button">More</button></div></div>
  </aside>

  <header class="create-header"><a class="back-link" href="/status-pages"><svg aria-hidden="true" viewBox="0 0 16 16"><path d="M10.5 3.5 6 8l4.5 4.5M6.5 8H13" /></svg> Back to status pages</a><div><p class="section-kicker section-kicker-spacer" aria-hidden="true">Admin console</p><h1 id="create-status-title">{editing ? 'Edit status page' : 'Add new status page'} <span class="info-dot" title="Configure the public status page appearance and identity.">i</span></h1></div></header>

  <section class="create-shell" aria-labelledby="create-status-title">
    <form onsubmit={(event) => event.preventDefault()}>
      <div class="form-tabs" role="tablist" aria-label="Status page sections">
        <button class:active={activeTab === 'settings'} type="button" role="tab" aria-selected={activeTab === 'settings'} onclick={() => activeTab = 'settings'}>Settings</button>
        <button class:active={activeTab === 'monitors'} type="button" role="tab" aria-selected={activeTab === 'monitors'} onclick={() => activeTab = 'monitors'}>Monitors</button>
        <button class:active={activeTab === 'maintenance'} type="button" role="tab" aria-selected={activeTab === 'maintenance'} onclick={() => activeTab = 'maintenance'}>Maintenance</button>
        <button class:active={activeTab === 'status-updates'} type="button" role="tab" aria-selected={activeTab === 'status-updates'} onclick={() => activeTab = 'status-updates'}>Status updates</button>
      </div>
      {#if activeTab === 'settings'}
      <div class="personalization"><div class="form-columns">
        <section class="form-section" aria-labelledby="basic-title"><h2 id="basic-title">Basic information</h2><label for="status-name">Name <span aria-hidden="true" hidden>required</span></label><input id="status-name" type="text" placeholder="Acme Cloud" /><label for="status-slug">Slug</label><input id="status-slug" type="text" placeholder="acme-cloud" /><small>Used in the public URL: /status/acme-cloud</small></section>
        <section class="form-section" aria-labelledby="logo-title"><h2 id="logo-title">Logo</h2><label for="logo-upload">Status page icon</label><label class="upload-button" for="logo-upload"><svg aria-hidden="true" viewBox="0 0 20 20"><path d="M10 13V4m0 0L6.5 7.5M10 4l3.5 3.5M4 12.5v2A1.5 1.5 0 0 0 5.5 16h9a1.5 1.5 0 0 0 1.5-1.5v-2" /></svg> Upload image</label><input id="logo-upload" class="file-input" type="file" accept="image/*" /><small>PNG, JPG or SVG. This image appears in the public status page header.</small><label for="logo-click-url">Logo click URL</label><input id="logo-click-url" type="url" placeholder="https://example.com" /><small>Visitors will open this URL when they click the image.</small></section>
      </div></div>
      <details class="settings-section advanced-settings"><summary class="settings-section-heading"><svg aria-hidden="true" viewBox="0 0 16 16"><path d="m3 6 5 5 5-5" /></svg><span>Advanced settings</span></summary><div class="setting-row"><div class="setting-copy"><h3>Share significant news</h3><p>Let your customers know about news they can't miss by showing a short announcement on top of your status page.</p></div><div class="setting-control"><label for="announcement">Announcement</label><textarea id="announcement" rows="3" placeholder="Share an important update with your visitors..."></textarea><small>You can use markdown in this announcement.</small><label class="check-row"><input type="checkbox" checked /> <span>Notify status page subscribers about the change</span></label></div></div><div class="setting-row"><div class="setting-copy"><h3>Status history</h3><p>Choose how much uptime history to report on your status page.</p></div><div class="setting-control compact-controls"><label for="history-days">How many days to display?</label><select id="history-days"><option>90 days</option><option>60 days</option><option>30 days</option></select><label for="decimal-places">Uptime % decimal places <span class="info-dot" title="Controls how many decimal places appear in uptime values.">i</span></label><select id="decimal-places"><option>3 decimals</option><option>2 decimals</option><option>1 decimal</option></select></div></div><div class="access-card"><div><p class="access-kicker">Visibility</p><h3 id="access-title">Status page access</h3><span>Control whether visitors can see this page.</span></div><label class="toggle-row"><input type="checkbox" checked /><span class="toggle" aria-hidden="true"></span><span>Published</span></label></div><div class="setting-row danger-row"><div class="setting-copy"><h3 id="remove-title">Remove status page</h3></div><div class="setting-control"><button class="danger-button" type="button">Remove status page</button><p class="warning-copy">Warning: This is a destructive action that cannot be reversed.</p></div></div></details>
      {:else if activeTab === 'monitors'}
      <section class="monitor-picker" aria-labelledby="monitor-picker-title">
        <div class="monitor-picker-header"><div><h2 id="monitor-picker-title">Monitors</h2><p>Choose which monitors will appear on this public status page.</p></div><span class="selection-count">{selectedMonitors.length} selected</span></div>
        <div class="monitor-options">
          <label class="monitor-option"><input type="checkbox" value="acme-api" bind:group={selectedMonitors} /><span class="monitor-state operational"></span><span class="monitor-copy"><strong>Acme Cloud API</strong><small>api.acme.cloud</small></span><span class="monitor-uptime">99.98%</span></label>
          <label class="monitor-option"><input type="checkbox" value="billing-api" bind:group={selectedMonitors} /><span class="monitor-state operational"></span><span class="monitor-copy"><strong>Billing API</strong><small>billing.acme.cloud</small></span><span class="monitor-uptime">99.95%</span></label>
          <label class="monitor-option"><input type="checkbox" value="cdn-edge" bind:group={selectedMonitors} /><span class="monitor-state operational"></span><span class="monitor-copy"><strong>CDN edge</strong><small>cdn.acme.cloud</small></span><span class="monitor-uptime">100%</span></label>
          <label class="monitor-option"><input type="checkbox" value="auth-service" bind:group={selectedMonitors} /><span class="monitor-state operational"></span><span class="monitor-copy"><strong>Authentication</strong><small>auth.acme.cloud</small></span><span class="monitor-uptime">99.91%</span></label>
        </div>
      </section>
      {:else if activeTab === 'maintenance'}
      {#if !showMaintenanceForm}
      <section class="maintenance-list" aria-labelledby="maintenance-list-title">
        <div class="maintenance-list-header"><div><h2 id="maintenance-list-title">Maintenance</h2><p>Keep visitors informed about planned service interruptions.</p></div><button class="schedule-button" type="button" onclick={() => showMaintenanceForm = true}><span aria-hidden="true">+</span> Schedule maintenance</button></div>
        <div class="maintenance-items">
          {#each maintenances as maintenance (maintenance.id)}
          <article class="maintenance-item"><div class="maintenance-item-main"><span class="maintenance-dot {maintenance.tone}"></span><div><h3>{maintenance.title}</h3><p>{maintenance.services}</p></div></div><div class="maintenance-item-meta"><span class="maintenance-status {maintenance.tone}-label">{maintenance.status}</span><time>{maintenance.time}</time></div><button class="remove-maintenance" type="button" aria-label={`Remove ${maintenance.title}`} onclick={() => removeMaintenance(maintenance.id)}>Remove</button></article>
          {:else}
          <p class="maintenance-empty">No maintenance events yet.</p>
          {/each}
        </div>
        <details class="maintenance-advanced"><summary><svg aria-hidden="true" viewBox="0 0 16 16"><path d="m3 6 5 5 5-5" /></svg><span>Advanced settings</span></summary><div class="maintenance-retention"><div><h3>Maintenance history</h3><p>Choose how long completed maintenance events remain visible on the status page.</p></div><label for="maintenance-history-days">Keep history for<select id="maintenance-history-days" bind:value={maintenanceHistoryDays}><option value="3">3 days</option><option value="7">7 days</option><option value="15">15 days</option><option value="30">30 days</option></select></label></div></details>
      </section>
      {:else}
      <section class="maintenance-form" aria-labelledby="maintenance-title">
        <div class="maintenance-form-header"><div><h2 id="maintenance-title">Schedule maintenance</h2><p>Let visitors know when selected services will be temporarily unavailable.</p></div><span class="maintenance-badge">Scheduled</span></div>
        <button class="back-to-list" type="button" onclick={() => showMaintenanceForm = false}>← Back to maintenance</button>
        <div class="maintenance-fields">
          <label for="maintenance-title-input">Title<input id="maintenance-title-input" type="text" placeholder="Database maintenance" /></label>
          <label for="maintenance-description">Description<textarea id="maintenance-description" rows="4" placeholder="We will be performing planned maintenance on our database cluster."></textarea></label>
          <div class="maintenance-dates"><label for="maintenance-from">From<input id="maintenance-from" type="datetime-local" /></label><label for="maintenance-to">To<input id="maintenance-to" type="datetime-local" /></label></div>
        </div>
        <div class="affected-monitors"><div class="affected-header"><div><h3>Affected monitors</h3><p>Select the services that will be impacted by this maintenance.</p></div><span>{affectedMonitors.length} selected</span></div><div class="monitor-options">
          <label class="monitor-option"><input type="checkbox" value="acme-api" bind:group={affectedMonitors} /><span class="monitor-state operational"></span><span class="monitor-copy"><strong>Acme Cloud API</strong><small>api.acme.cloud</small></span></label>
          <label class="monitor-option"><input type="checkbox" value="billing-api" bind:group={affectedMonitors} /><span class="monitor-state operational"></span><span class="monitor-copy"><strong>Billing API</strong><small>billing.acme.cloud</small></span></label>
          <label class="monitor-option"><input type="checkbox" value="cdn-edge" bind:group={affectedMonitors} /><span class="monitor-state operational"></span><span class="monitor-copy"><strong>CDN edge</strong><small>cdn.acme.cloud</small></span></label>
          <label class="monitor-option"><input type="checkbox" value="auth-service" bind:group={affectedMonitors} /><span class="monitor-state operational"></span><span class="monitor-copy"><strong>Authentication</strong><small>auth.acme.cloud</small></span></label>
        </div></div>
      </section>
      {/if}
      {:else}
      {#if !showUpdateForm}
      <section class="updates-list" aria-labelledby="updates-list-title">
        <div class="updates-list-header"><div><h2 id="updates-list-title">Status updates</h2><p>Share incident progress with visitors on your public status page.</p></div><button class="schedule-button" type="button" onclick={() => showUpdateForm = true}><span aria-hidden="true">+</span> Post status update</button></div>
        <div class="update-items">
          <article class="update-item"><div><h3>Investigating elevated API errors</h3><p>We are investigating increased error rates affecting the API.</p></div><time>Today, 02:17 UTC</time></article>
          <article class="update-item"><div><h3>Issue resolved</h3><p>API response times have returned to normal.</p></div><time>Yesterday, 18:42 UTC</time></article>
        </div>
      </section>
      {:else}
      <section class="update-form" aria-labelledby="update-title">
        <div class="update-form-header"><div><h2 id="update-title">Post status update</h2><p>Keep subscribers informed as the incident progresses.</p></div><span class="update-badge">Incident update</span></div>
        <div class="update-fields"><label for="update-summary">What's going on?<input id="update-summary" type="text" placeholder="Dashboard is unavailable" /></label><small>Concise summary of the incident.</small><label for="update-description">Description<textarea id="update-description" rows="4" placeholder="Dashboard is currently unavailable for a small percentage of our users."></textarea></label><small>You can use markdown in this update.</small><label for="update-published-at">Published at<input id="update-published-at" type="datetime-local" /></label><label class="check-row update-notify"><input type="checkbox" bind:checked={notifyUpdateSubscribers} /> <span>Notify status page subscribers</span></label></div>
        <div class="service-statuses"><div class="service-statuses-header"><div><h3>Current status by service</h3><p>Set the impact of this update for each affected service.</p></div></div><div class="service-status-list"><label><span>Acme Cloud API</span><select><option>Not affected</option><option>Degraded performance</option><option>Major outage</option><option>Operational</option></select></label><label><span>Billing API</span><select><option>Not affected</option><option>Degraded performance</option><option>Major outage</option><option>Operational</option></select></label><label><span>CDN edge</span><select><option>Not affected</option><option>Degraded performance</option><option>Major outage</option><option>Operational</option></select></label></div></div>
      </section>
      {/if}
      {/if}
      {#if activeTab !== 'maintenance' && activeTab !== 'status-updates'}
      <footer class="form-footer"><p>Configuration is only previewed in this prototype.</p><button class="save-button" type="submit">Save status page</button></footer>
      {:else if showMaintenanceForm}
      <footer class="form-footer maintenance-footer"><p>Maintenance is only previewed in this prototype.</p><div class="maintenance-actions"><button class="cancel-button" type="button" onclick={() => showMaintenanceForm = false}>Cancel</button><button class="save-button" type="submit">Schedule maintenance</button></div></footer>
      {:else if showUpdateForm}
      <footer class="form-footer maintenance-footer"><p>Status update is only previewed in this prototype.</p><div class="maintenance-actions"><button class="cancel-button" type="button" onclick={() => showUpdateForm = false}>Cancel</button><button class="save-button" type="submit">Post status update</button></div></footer>
      {/if}
    </form>
  </section>
</main>

<style>
  :global(html), :global(body) { overflow-x: hidden; }
  .status-create-page { min-height: 100svh; display: grid; grid-template-columns: 232px minmax(0, 1fr); padding: 24px 0 48px; background: var(--page); color: var(--ink); }.sidebar { position: fixed; top: 0; left: 0; z-index: 10; display: flex; width: 232px; height: 100svh; min-height: 100svh; padding: 22px 14px 16px; flex-direction: column; background: #141821; border-right: 1px solid var(--border); }.sidebar-top { display: flex; align-items: baseline; justify-content: space-between; padding: 0 10px; }.wordmark { color: var(--ink); font-size: 1.65rem; font-weight: 720; letter-spacing: -0.045em; line-height: 1; text-decoration: none; }.console-label, .section-kicker, .operator small { color: var(--muted); font-size: 0.6875rem; line-height: 1.4; }.console-label, .section-kicker { letter-spacing: 0.02em; text-transform: uppercase; }.create-actions { display: grid; gap: 8px; margin: 38px 0 28px; }.sidebar-create { display: flex; min-height: 38px; align-items: center; justify-content: center; gap: 7px; border: 1px solid var(--green); border-radius: 5px; color: var(--page); background: var(--green); font-size: 0.8125rem; font-weight: 620; text-decoration: none; }.sidebar-create.secondary { border-color: var(--border-light); color: var(--secondary); background: transparent; }.sidebar-nav { display: grid; gap: 3px; }.sidebar-nav a { display: flex; align-items: center; gap: 11px; padding: 10px; border-radius: 5px; color: var(--secondary); font-size: 0.8125rem; text-decoration: none; }.sidebar-nav a:hover, .sidebar-nav a.active { color: var(--ink); background: var(--surface-hover); }.sidebar-nav svg { display: block; width: 16px; height: 16px; flex: 0 0 auto; fill: none; stroke: currentColor; stroke-linecap: round; stroke-linejoin: round; stroke-width: 1.5; }.sidebar-nav a:first-child svg { fill: currentColor; stroke: none; }.sidebar-bottom { display: grid; gap: 20px; margin-top: auto; }.public-link { padding: 0 10px; color: var(--secondary); font-size: 0.6875rem; text-underline-offset: 3px; }.operator { display: flex; align-items: center; gap: 9px; padding: 12px 8px 0; border-top: 1px solid var(--border); }.operator-avatar { display: grid; width: 28px; height: 28px; place-items: center; border-radius: 50%; color: var(--page); background: var(--green); font-size: 0.6875rem; font-weight: 700; }.operator > span:nth-child(2) { display: grid; min-width: 0; gap: 2px; }.operator strong { overflow: hidden; color: var(--secondary); font-size: 0.6875rem; font-weight: 600; text-overflow: ellipsis; white-space: nowrap; }.operator button { margin-left: auto; border: 0; color: var(--muted); background: transparent; font-size: 0.6875rem; cursor: pointer; }.sidebar-toggle { display: grid; width: 24px; height: 24px; flex: 0 0 auto; place-items: center; padding: 0; border: 1px solid var(--border-light); border-radius: 5px; color: var(--muted); background: transparent; cursor: pointer; }.sidebar-toggle svg { width: 14px; height: 14px; fill: none; stroke: currentColor; stroke-linecap: round; stroke-linejoin: round; stroke-width: 1.5; }
  .sidebar-collapsed.status-create-page { grid-template-columns: 72px minmax(0, 1fr); }.sidebar-collapsed.status-create-page > .sidebar { width: 72px; padding-right: 10px; padding-left: 10px; }.sidebar-collapsed .sidebar-top { align-items: center; flex-direction: column; gap: 12px; padding: 0; }.sidebar-collapsed .wordmark { font-size: 0; }.sidebar-collapsed .wordmark::after { color: var(--ink); content: 'z'; font-size: 1.65rem; font-weight: 720; }.sidebar-collapsed .console-label { display: none; }.sidebar-collapsed .create-actions { justify-items: center; }.sidebar-collapsed .sidebar-create { width: 40px; padding: 0; font-size: 0; gap: 0; }.sidebar-collapsed .sidebar-create.secondary { display: none; }.sidebar-collapsed .sidebar-create span { position: relative; display: block; width: 14px; height: 14px; font-size: 0; }.sidebar-collapsed .sidebar-create span::before, .sidebar-collapsed .sidebar-create span::after { position: absolute; top: 6px; left: 1px; width: 12px; height: 2px; border-radius: 1px; background: currentColor; content: ''; }.sidebar-collapsed .sidebar-create span::after { transform: rotate(90deg); }.sidebar-collapsed .sidebar-nav a { justify-content: center; gap: 0; padding-right: 10px; padding-left: 10px; font-size: 0; }.sidebar-collapsed .sidebar-nav svg { width: 18px; height: 18px; }.sidebar-collapsed .public-link, .sidebar-collapsed .operator > span:nth-child(2), .sidebar-collapsed .operator button { display: none; }.sidebar-collapsed .operator { justify-content: center; padding-right: 0; padding-left: 0; }
  .create-header, .create-shell { grid-column: 2; width: min(100%, 992px); margin: 0 auto; }.create-header { position: relative; display: block; padding-top: 60px; }.create-header .back-link { position: absolute; top: 24px; left: 0; }.back-link { display: inline-flex; min-height: 34px; align-items: center; gap: 7px; padding: 0 11px; border: 1px solid var(--border-light); border-radius: 5px; color: var(--secondary); background: var(--surface); font-size: 0.8125rem; text-decoration: none; }.back-link:hover { color: var(--ink); border-color: var(--secondary); background: var(--surface-hover); }.back-link svg { width: 15px; height: 15px; fill: none; stroke: currentColor; stroke-linecap: round; stroke-linejoin: round; stroke-width: 1.5; }.create-header h1 { margin: 0; font-size: 1.65rem; font-weight: 680; letter-spacing: -0.03em; line-height: 1.25; }.create-header .section-kicker { margin: 0 0 7px; }.info-dot { display: inline-grid; width: 15px; height: 15px; margin-left: 4px; place-items: center; border-radius: 50%; color: var(--page); background: var(--muted); font-size: 0.6875rem; font-weight: 700; vertical-align: 3px; }.create-shell { margin-top: 32px; overflow: hidden; border: 1px solid var(--border); border-radius: 12px; background: var(--surface); box-shadow: 0 4px 7px rgb(0 0 0 / 3%); }.form-columns { display: grid; grid-template-columns: minmax(0, 1fr) minmax(0, 1fr); }.form-section { padding: 28px 30px 34px; }.form-section + .form-section, .personalization { border-top: 1px solid var(--border); }.form-section h2 { margin-bottom: 24px; font-size: 1rem; font-weight: 700; line-height: 1.35; }.form-section label { display: block; margin-top: 18px; color: var(--secondary); font-size: 0.8125rem; font-weight: 600; line-height: 1.4; }.form-section label:first-of-type { margin-top: 0; }.form-section label span { color: var(--muted); font-weight: 400; }.form-section input, .form-section select, .form-section textarea { display: block; width: 100%; margin-top: 7px; border: 1px solid var(--border-light); border-radius: 5px; color: var(--ink); background: var(--page); font: inherit; font-size: 0.8125rem; }.form-section input, .form-section select { height: 42px; padding: 0 12px; }.form-section select { appearance: none; padding-right: 38px; background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='14' height='14' viewBox='0 0 14 14'%3E%3Cpath d='m3 5 4 4 4-4' fill='none' stroke='%23a7acbb' stroke-linecap='round' stroke-linejoin='round' stroke-width='1.5'/%3E%3C/svg%3E"); background-position: right 12px center; background-repeat: no-repeat; background-size: 14px 14px; }.form-section textarea { min-height: 108px; padding: 12px; resize: vertical; line-height: 1.5; }.form-section input::placeholder, .form-section textarea::placeholder { color: var(--muted); }.form-section input:focus, .form-section select:focus, .form-section textarea:focus { border-color: var(--green); outline: 2px solid rgb(16 185 129 / 18%); outline-offset: 1px; }.form-section small { display: block; margin-top: 6px; color: var(--muted); font-size: 0.6875rem; line-height: 1.4; }.personalization .form-columns { gap: 20px; }.form-footer { display: flex; align-items: center; justify-content: space-between; gap: 20px; padding: 14px 18px; border-top: 1px solid var(--border); background: rgb(15 18 26 / 50%); }.form-footer p { color: var(--muted); font-size: 0.6875rem; line-height: 1.4; }.save-button { min-height: 38px; padding: 0 18px; border: 1px solid var(--green); border-radius: 5px; color: var(--page); background: var(--green); font-size: 0.8125rem; font-weight: 650; cursor: pointer; }.save-button:hover { filter: brightness(1.08); }:focus-visible { outline: 2px solid var(--green); outline-offset: 3px; }
  .file-input { position: absolute; width: 1px; height: 1px; overflow: hidden; clip: rect(0 0 0 0); white-space: nowrap; }
  .upload-button { display: inline-flex !important; width: fit-content; min-height: 38px; align-items: center; gap: 8px; margin-top: 7px !important; padding: 0 13px; border: 1px solid var(--border-light); border-radius: 5px; color: var(--secondary); background: transparent; cursor: pointer; font-weight: 600; }
  .upload-button:hover { color: var(--ink); border-color: var(--secondary); background: var(--surface-hover); }
  .upload-button svg { width: 16px; height: 16px; fill: none; stroke: currentColor; stroke-linecap: round; stroke-linejoin: round; stroke-width: 1.5; }
  .settings-section { border-top: 1px solid var(--border); }.settings-section-heading { display: flex; align-items: center; gap: 10px; min-height: 58px; padding: 0 30px; color: var(--muted); }.settings-section-heading svg { width: 16px; height: 16px; fill: none; stroke: currentColor; stroke-linecap: round; stroke-linejoin: round; stroke-width: 1.5; }.settings-section-heading h2 { margin: 0; font-size: 1rem; font-weight: 600; }.setting-row { display: grid; grid-template-columns: minmax(220px, 0.72fr) minmax(0, 1.28fr); gap: 28px; padding: 30px; border-top: 1px solid var(--border); }.setting-copy h3 { font-size: 1rem; font-weight: 700; line-height: 1.35; }.setting-copy p { max-width: 36ch; margin-top: 12px; color: var(--secondary); font-size: 0.9375rem; line-height: 1.55; }.setting-control label { display: block; margin: 0 0 7px; color: var(--secondary); font-size: 0.8125rem; font-weight: 600; }.setting-control textarea, .setting-control select { display: block; width: 100%; border: 1px solid var(--border-light); border-radius: 5px; color: var(--ink); background: var(--page); font: inherit; font-size: 0.8125rem; }.setting-control textarea { min-height: 100px; padding: 12px; resize: vertical; }.setting-control select { height: 42px; padding: 0 12px; }.setting-control small { display: block; margin-top: 7px; color: var(--muted); font-size: 0.6875rem; }.check-row { display: flex !important; align-items: center; gap: 10px; margin-top: 24px !important; color: var(--ink) !important; font-size: 0.9375rem !important; font-weight: 400 !important; }.check-row input { width: 18px; height: 18px; margin: 0; accent-color: var(--blue); }.compact-controls { max-width: 560px; }.compact-controls .check-row + label { margin-top: 26px; }.info-dot { display: inline-grid; width: 15px; height: 15px; margin-left: 3px; place-items: center; border-radius: 50%; color: var(--page); background: var(--muted); font-size: 0.6875rem; font-weight: 700; vertical-align: 2px; }.toggle-row { display: flex !important; align-items: center; gap: 12px; margin: 0 !important; color: var(--ink) !important; font-size: 0.9375rem !important; font-weight: 400 !important; }.toggle-row input { position: absolute; opacity: 0; }.toggle { position: relative; display: inline-block; width: 42px; height: 24px; flex: 0 0 auto; border-radius: 999px; background: var(--muted); }.toggle::after { position: absolute; top: 3px; left: 3px; width: 18px; height: 18px; border-radius: 50%; background: var(--ink); content: ''; transition: transform 140ms ease-out; }.toggle-row input:checked + .toggle { background: var(--blue); }.toggle-row input:checked + .toggle::after { transform: translateX(18px); }.danger-section .setting-row { padding-bottom: 34px; }.danger-button { min-height: 38px; padding: 0 16px; border: 1px solid var(--red); border-radius: 5px; color: var(--ink); background: var(--red); font: inherit; font-size: 0.8125rem; font-weight: 650; cursor: pointer; }.warning-copy { margin-top: 12px !important; color: var(--secondary) !important; font-size: 0.8125rem !important; }
  @media (max-width: 760px) { .status-create-page { display: block; padding: 18px 14px 36px; }.status-create-page > .sidebar, .sidebar-collapsed.status-create-page > .sidebar { position: static; width: auto; height: auto; min-height: auto; margin: -18px -14px 0; padding: 14px 12px 12px; border-right: 0; border-bottom: 1px solid var(--border); }.sidebar-toggle { display: none; }.sidebar-top { padding: 0 4px; }.create-actions { margin: 22px 0 14px; }.sidebar-nav { display: flex; overflow-x: auto; }.sidebar-nav a { flex: 0 0 auto; }.sidebar-bottom { display: none; }.create-header, .create-shell { width: 100%; }.create-header { padding-top: 0; }.create-header .back-link { position: static; }.form-columns { display: block; }.form-section { padding: 24px 20px 28px; }.personalization .form-columns { display: block; }.form-footer { align-items: flex-start; flex-direction: column-reverse; }.save-button { width: 100%; } }
  .setting-copy p { display: none; }
  .setting-row { grid-template-columns: minmax(160px, 0.55fr) minmax(0, 1.45fr); gap: 22px; padding: 22px 24px; }
  .setting-copy h3 { font-size: 0.9375rem; }
  .setting-control textarea { min-height: 76px; }
  .compact-controls select + label { margin-top: 24px; }
  .advanced-settings .access-card { border-top: 1px solid var(--border); }
  .advanced-settings .danger-row { border-top: 1px solid var(--border); }
  .compact-controls select { width: 280px; }
  .setting-control select { appearance: none; padding-right: 38px; background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='14' height='14' viewBox='0 0 14 14'%3E%3Cpath d='m3 5 4 4 4-4' fill='none' stroke='%238a91a5' stroke-linecap='round' stroke-linejoin='round' stroke-width='1.5'/%3E%3C/svg%3E"); background-position: right 12px center; background-repeat: no-repeat; background-size: 14px 14px; }
  .access-card { justify-content: flex-start; gap: 32px; padding: 18px 24px; }
  .advanced-settings:not([open]) { height: auto; min-height: 0; }
  .advanced-settings:not([open]) > :not(summary) { display: none; }
  .form-footer { margin-top: 0; }
  .status-create-page { align-items: start; align-content: start; }
  .status-create-page > .create-shell { align-self: start; }
  .access-card > .toggle-row { width: fit-content; margin-left: 0; }
  .access-card { display: grid; grid-template-columns: minmax(160px, 280px) auto; justify-content: start; }
  .access-card { grid-template-columns: minmax(120px, 180px) auto; }
  .section-kicker-spacer { visibility: hidden; }
  @media (max-width: 760px) { .access-card { display: flex; } }
  @media (max-width: 760px) { .compact-controls select { width: 100%; }.access-card { gap: 18px; } }
  .advanced-settings > summary { list-style: none; cursor: pointer; }.advanced-settings > summary::-webkit-details-marker { display: none; }.advanced-settings > summary svg { transition: transform 140ms ease-out; }.advanced-settings[open] > summary svg { transform: rotate(180deg); }.advanced-settings > summary:focus-visible { outline: 2px solid var(--green); outline-offset: -2px; }
  .access-card { display: flex; align-items: center; justify-content: flex-start; gap: 24px; padding: 22px 24px; }.access-kicker { margin: 0 0 5px; color: var(--muted); font-size: 0.6875rem; letter-spacing: 0.02em; text-transform: uppercase; }.access-card h3 { font-size: 0.9375rem; line-height: 1.35; }.access-card > div > span { display: block; margin-top: 5px; color: var(--secondary); font-size: 0.8125rem; }
  @media (max-width: 760px) { .access-card { align-items: flex-start; flex-direction: column; } }
  .form-tabs { display: flex; gap: 4px; padding: 0 24px; border-bottom: 1px solid var(--border); background: rgb(15 18 26 / 28%); }
  .form-tabs button { position: relative; min-height: 48px; padding: 0 12px; border: 0; color: var(--muted); background: transparent; font: inherit; font-size: 0.8125rem; font-weight: 620; cursor: pointer; }
  .form-tabs button::after { position: absolute; right: 10px; bottom: -1px; left: 10px; height: 2px; border-radius: 2px; background: transparent; content: ''; }
  .form-tabs button:hover, .form-tabs button.active { color: var(--ink); }
  .form-tabs button.active::after { background: var(--green); }
  .status-create-page { width: 100%; max-width: 100vw; overflow-x: clip; }
  .status-create-page, .create-shell, .create-shell form, .form-section, .setting-copy, .setting-control { min-width: 0; max-width: 100%; }
  .form-tabs { flex-wrap: wrap; }
  .compact-controls select { max-width: 100%; }
  .monitor-picker { padding: 26px 24px 28px; }
  .monitor-picker-header { display: flex; align-items: flex-start; justify-content: space-between; gap: 20px; margin-bottom: 20px; }
  .monitor-picker-header h2 { margin: 0; font-size: 1rem; font-weight: 700; line-height: 1.35; }
  .monitor-picker-header p { margin: 7px 0 0; color: var(--secondary); font-size: 0.8125rem; }
  .selection-count { flex: 0 0 auto; padding: 5px 8px; border-radius: 999px; color: var(--green); background: rgb(16 185 129 / 12%); font-size: 0.6875rem; font-weight: 650; }
  .monitor-options { overflow: hidden; border: 1px solid var(--border); border-radius: 8px; }
  .monitor-option { display: grid; grid-template-columns: 18px 9px minmax(0, 1fr) auto; gap: 12px; min-height: 64px; align-items: center; padding: 0 16px; border-top: 1px solid var(--border); cursor: pointer; }
  .monitor-option:first-child { border-top: 0; }
  .monitor-option:hover { background: var(--surface-hover); }
  .monitor-option input { width: 16px; height: 16px; margin: 0; accent-color: var(--green); }
  .monitor-state { width: 9px; height: 9px; border-radius: 50%; }
  .monitor-state.operational { background: var(--green); box-shadow: 0 0 0 4px rgb(16 185 129 / 10%); }
  .monitor-copy { display: grid; gap: 3px; min-width: 0; }
  .monitor-copy strong { overflow: hidden; color: var(--ink); font-size: 0.8125rem; font-weight: 620; text-overflow: ellipsis; white-space: nowrap; }
  .monitor-copy small { overflow: hidden; color: var(--muted); font-size: 0.6875rem; text-overflow: ellipsis; white-space: nowrap; }
  .monitor-uptime { color: var(--secondary); font-size: 0.75rem; font-variant-numeric: tabular-nums; }
  .maintenance-form { padding: 26px 24px 28px; }
  .maintenance-list { padding: 26px 24px 28px; }
  .maintenance-list-header { display: flex; align-items: flex-start; justify-content: space-between; gap: 20px; margin-bottom: 22px; }
  .maintenance-list-header h2 { margin: 0; font-size: 1rem; font-weight: 700; line-height: 1.35; }
  .maintenance-list-header p { margin: 7px 0 0; color: var(--secondary); font-size: 0.8125rem; }
  .schedule-button { display: inline-flex; min-height: 36px; flex: 0 0 auto; align-items: center; gap: 7px; padding: 0 13px; border: 1px solid var(--green); border-radius: 5px; color: var(--page); background: var(--green); font: inherit; font-size: 0.75rem; font-weight: 650; cursor: pointer; }
  .schedule-button span { font-size: 1rem; font-weight: 500; line-height: 1; }
  .schedule-button:hover { filter: brightness(1.08); }
  .maintenance-items { overflow: hidden; border: 1px solid var(--border); border-radius: 8px; }
  .maintenance-item { display: grid; grid-template-columns: minmax(0, 1fr) auto auto; align-items: center; gap: 24px; min-height: 76px; margin-top: 0; padding: 15px 16px; border-top: 1px solid var(--border); }
  .maintenance-item:first-child { border-top: 0; }
  .maintenance-item:hover { background: var(--surface-hover); }
  .maintenance-item-main { display: flex; min-width: 0; align-items: center; gap: 12px; }
  .maintenance-dot { width: 9px; height: 9px; flex: 0 0 auto; padding: 0; border-radius: 50%; }
  .maintenance-dot.scheduled { background: var(--maintenance-blue, #60a5fa); box-shadow: 0 0 0 4px rgb(96 165 250 / 10%); }
  .maintenance-dot.completed { background: var(--green); box-shadow: 0 0 0 4px rgb(16 185 129 / 10%); }
  .maintenance-item h3 { overflow: hidden; margin: 0; color: var(--ink); font-size: 0.8125rem; font-weight: 620; text-overflow: ellipsis; white-space: nowrap; }
  .maintenance-item p { overflow: hidden; margin: 4px 0 0; color: var(--muted); font-size: 0.6875rem; text-overflow: ellipsis; white-space: nowrap; }
  .maintenance-item-meta { display: grid; flex: 0 0 auto; justify-items: end; gap: 5px; }
  .maintenance-status { padding: 4px 7px; border-radius: 999px; font-size: 0.625rem; font-weight: 650; }
  .scheduled-label { color: var(--maintenance-blue, #60a5fa); background: rgb(96 165 250 / 12%); }
  .completed-label { color: var(--green); background: rgb(16 185 129 / 12%); }
  .maintenance-item time { color: var(--muted); font-size: 0.6875rem; font-variant-numeric: tabular-nums; }
  .remove-maintenance { min-height: 30px; padding: 0 9px; border: 1px solid rgb(248 113 113 / 28%); border-radius: 5px; color: var(--red, #f87171); background: rgb(248 113 113 / 8%); font: inherit; font-size: 0.6875rem; cursor: pointer; }
  .remove-maintenance:hover { border-color: rgb(248 113 113 / 55%); background: rgb(248 113 113 / 15%); }
  .maintenance-empty { margin: 0; padding: 28px 16px; color: var(--muted); font-size: 0.75rem; text-align: center; }
  .back-to-list { margin: -8px 0 22px; padding: 0; border: 0; color: var(--secondary); background: transparent; font: inherit; font-size: 0.75rem; cursor: pointer; }
  .back-to-list:hover { color: var(--ink); }
  .maintenance-advanced { margin-top: 20px; border-top: 1px solid var(--border); }
  .maintenance-advanced summary { display: flex; min-height: 48px; align-items: center; gap: 9px; color: var(--muted); font-size: 0.8125rem; font-weight: 620; cursor: pointer; list-style: none; }
  .maintenance-advanced summary::-webkit-details-marker { display: none; }
  .maintenance-advanced summary svg { width: 15px; height: 15px; fill: none; stroke: currentColor; stroke-linecap: round; stroke-linejoin: round; stroke-width: 1.5; transition: transform 140ms ease-out; }
  .maintenance-advanced[open] summary svg { transform: rotate(180deg); }
  .maintenance-advanced summary:focus-visible { outline: 2px solid var(--green); outline-offset: -2px; }
  .maintenance-retention { display: flex; align-items: flex-start; justify-content: space-between; gap: 24px; padding: 18px 0 2px; border-top: 1px solid var(--border); }
  .maintenance-retention h3 { margin: 0; color: var(--ink); font-size: 0.8125rem; font-weight: 620; }
  .maintenance-retention p { max-width: 46ch; margin: 6px 0 0; color: var(--secondary); font-size: 0.75rem; line-height: 1.45; }
  .maintenance-retention label { display: grid; flex: 0 0 auto; gap: 7px; color: var(--secondary); font-size: 0.75rem; font-weight: 600; }
  .maintenance-retention select { width: 180px; height: 38px; padding: 0 34px 0 11px; border: 1px solid var(--border-light); border-radius: 5px; color: var(--ink); background-color: var(--page); background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='14' height='14' viewBox='0 0 14 14'%3E%3Cpath d='m3 5 4 4 4-4' fill='none' stroke='%238a91a5' stroke-linecap='round' stroke-linejoin='round' stroke-width='1.5'/%3E%3C/svg%3E"); background-position: right 11px center; background-repeat: no-repeat; background-size: 14px 14px; font: inherit; font-size: 0.8125rem; appearance: none; }
  .updates-list, .update-form { padding: 26px 24px 28px; }
  .updates-list-header, .update-form-header, .service-statuses-header { display: flex; align-items: flex-start; justify-content: space-between; gap: 20px; }
  .updates-list-header { margin-bottom: 22px; }
  .updates-list-header h2, .update-form-header h2 { margin: 0; font-size: 1rem; font-weight: 700; line-height: 1.35; }
  .updates-list-header p, .update-form-header p, .service-statuses-header p { margin: 7px 0 0; color: var(--secondary); font-size: 0.8125rem; }
  .update-badge { flex: 0 0 auto; padding: 5px 8px; border-radius: 999px; color: var(--maintenance-blue, #60a5fa); background: rgb(96 165 250 / 12%); font-size: 0.6875rem; font-weight: 650; }
  .update-items { overflow: hidden; border: 1px solid var(--border); border-radius: 8px; }
  .update-item { display: flex; align-items: flex-start; justify-content: space-between; gap: 20px; padding: 17px 16px; border-top: 1px solid var(--border); }
  .update-item:first-child { border-top: 0; }
  .update-item:hover { background: var(--surface-hover); }
  .update-item h3 { margin: 0; color: var(--ink); font-size: 0.8125rem; font-weight: 620; }
  .update-item p { margin: 5px 0 0; color: var(--secondary); font-size: 0.75rem; }
  .update-item time { flex: 0 0 auto; color: var(--muted); font-size: 0.6875rem; }
  .update-form-header { margin-bottom: 20px; }
  .update-fields { display: grid; gap: 15px; }
  .update-fields label:not(.check-row) { display: grid; gap: 7px; color: var(--secondary); font-size: 0.8125rem; font-weight: 600; }
  .update-fields input, .update-fields textarea { width: 100%; border: 1px solid var(--border-light); border-radius: 5px; color: var(--ink); background: var(--page); font: inherit; font-size: 0.8125rem; }
  .update-fields input { height: 42px; padding: 0 12px; }
  .update-fields textarea { min-height: 88px; padding: 12px; resize: vertical; line-height: 1.5; }
  .update-fields input:focus, .update-fields textarea:focus, .service-status-list select:focus { border-color: var(--green); outline: 2px solid rgb(16 185 129 / 18%); outline-offset: 1px; }
  .update-fields > small { margin-top: -8px; color: var(--muted); font-size: 0.6875rem; }
  .update-notify { display: flex !important; width: fit-content; align-items: center !important; gap: 9px !important; margin: 2px 0 0 !important; color: var(--secondary) !important; font-size: 0.8125rem !important; line-height: 1.4; }
  .update-notify input { width: 16px !important; height: 16px !important; flex: 0 0 auto; }
  .service-statuses { margin-top: 26px; }
  .service-statuses-header { margin-bottom: 14px; }
  .service-statuses-header h3 { margin: 0; color: var(--ink); font-size: 0.9375rem; font-weight: 650; }
  .service-status-list { overflow: hidden; border: 1px solid var(--border); border-radius: 8px; }
  .service-status-list label { display: flex; align-items: center; justify-content: space-between; gap: 16px; min-height: 60px; padding: 0 16px; border-top: 1px solid var(--border); color: var(--ink); font-size: 0.8125rem; font-weight: 620; }
  .service-status-list label:first-child { border-top: 0; }
  .service-status-list select { width: 210px; height: 38px; padding: 0 32px 0 11px; border: 1px solid var(--border-light); border-radius: 5px; color: var(--secondary); background-color: var(--page); background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='14' height='14' viewBox='0 0 14 14'%3E%3Cpath d='m3 5 4 4 4-4' fill='none' stroke='%238a91a5' stroke-linecap='round' stroke-linejoin='round' stroke-width='1.5'/%3E%3C/svg%3E"); background-position: right 10px center; background-repeat: no-repeat; background-size: 14px 14px; font: inherit; font-size: 0.75rem; appearance: none; }
  .maintenance-actions { display: flex; align-items: center; gap: 10px; }
  .cancel-button { min-height: 38px; padding: 0 14px; border: 1px solid var(--border-light); border-radius: 5px; color: var(--secondary); background: transparent; font: inherit; font-size: 0.8125rem; font-weight: 620; cursor: pointer; }
  .cancel-button:hover { color: var(--ink); border-color: var(--secondary); background: var(--surface-hover); }
  .maintenance-form-header, .affected-header { display: flex; align-items: flex-start; justify-content: space-between; gap: 20px; }
  .maintenance-form-header { margin-bottom: 24px; }
  .maintenance-form-header h2 { margin: 0; font-size: 1rem; font-weight: 700; line-height: 1.35; }
  .maintenance-form-header p, .affected-header p { margin: 7px 0 0; color: var(--secondary); font-size: 0.8125rem; }
  .maintenance-badge { flex: 0 0 auto; padding: 5px 8px; border-radius: 999px; color: var(--maintenance-blue, #60a5fa); background: rgb(96 165 250 / 12%); font-size: 0.6875rem; font-weight: 650; }
  .maintenance-fields { display: grid; gap: 18px; }
  .maintenance-fields label, .maintenance-dates label { display: grid; gap: 7px; color: var(--secondary); font-size: 0.8125rem; font-weight: 600; }
  .maintenance-fields input, .maintenance-fields textarea { width: 100%; border: 1px solid var(--border-light); border-radius: 5px; color: var(--ink); background: var(--page); font: inherit; font-size: 0.8125rem; }
  .maintenance-fields input { height: 42px; padding: 0 12px; }
  .maintenance-fields textarea { min-height: 88px; padding: 12px; resize: vertical; line-height: 1.5; }
  .maintenance-fields input:focus, .maintenance-fields textarea:focus { border-color: var(--green); outline: 2px solid rgb(16 185 129 / 18%); outline-offset: 1px; }
  .maintenance-dates { display: grid; grid-template-columns: 1fr 1fr; gap: 16px; }
  .affected-monitors { margin-top: 28px; }
  .affected-header { margin-bottom: 14px; }
  .affected-header h3 { margin: 0; color: var(--ink); font-size: 0.9375rem; font-weight: 650; }
  .affected-header > span { flex: 0 0 auto; color: var(--muted); font-size: 0.6875rem; }
  @media (max-width: 760px) { .form-tabs { padding: 0 16px; }.monitor-picker { padding: 22px 20px 24px; }.monitor-picker-header { flex-direction: column; gap: 10px; }.monitor-option { grid-template-columns: 18px 9px minmax(0, 1fr); padding: 0 12px; }.monitor-uptime { grid-column: 3; margin-top: -18px; justify-self: end; } }
  @media (max-width: 760px) { .maintenance-list, .maintenance-form, .updates-list, .update-form { padding: 22px 20px 24px; }.maintenance-list-header, .maintenance-form-header, .affected-header, .maintenance-retention, .updates-list-header, .update-form-header { flex-direction: column; gap: 10px; }.schedule-button { width: 100%; justify-content: center; }.maintenance-item { grid-template-columns: minmax(0, 1fr) auto; align-items: start; gap: 10px 14px; }.maintenance-item-main { grid-column: 1 / -1; }.maintenance-item-meta { align-items: start; justify-items: start; }.remove-maintenance { align-self: end; }.maintenance-dates { grid-template-columns: 1fr; gap: 18px; }.maintenance-actions { width: 100%; }.maintenance-actions button { flex: 1; }.maintenance-retention select { width: 100%; }.update-item { flex-direction: column; gap: 8px; }.update-item time { align-self: flex-start; }.service-status-list label { align-items: flex-start; flex-direction: column; gap: 9px; padding: 13px 12px; }.service-status-list select { width: 100%; } }
</style>
