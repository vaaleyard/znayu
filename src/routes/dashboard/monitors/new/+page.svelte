<svelte:head>
  <title>Create monitor — Znayu</title>
  <meta name="description" content="Create a monitor in the Znayu operator console." />
</svelte:head>

<main class="create-page">
  <header class="create-header">
    <a class="back-link" href="/dashboard/monitors">Back to monitors</a>
    <span class="prototype-chip">Prototype screen</span>
  </header>

  <section class="create-shell" aria-labelledby="create-title">
    <div class="create-heading"><p>Operator console</p><h1 id="create-title">Add new monitor</h1><span>Configure the check that will represent one service in your status pages.</span></div>
    <form onsubmit={(event) => event.preventDefault()}>
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
          <section class="form-section" aria-labelledby="notifications-title"><h2 id="notifications-title">Notifications</h2><p class="empty-setting">No notification channel configured.</p><button type="button" class="secondary-action">Configure notifications</button></section>
          <section class="form-section" aria-labelledby="proxy-title"><h2 id="proxy-title">Proxy</h2><p class="empty-setting">No proxy configured.</p><button type="button" class="secondary-action">Configure proxy</button></section>
          <section class="form-section" aria-labelledby="http-title"><h2 id="http-title">HTTP options</h2><label for="method">Method</label><select id="method"><option>GET</option><option>POST</option><option>HEAD</option></select><label for="encoding">Body encoding</label><select id="encoding"><option>JSON</option><option>Form data</option><option>Plain text</option></select><label for="body">Body <span>(optional)</span></label><textarea id="body" rows="6" placeholder={'Example:\n{\n  "key": "value"\n}'}></textarea></section>
        </div>
      </div>
      <footer class="form-footer"><p>Configuration is only previewed in this prototype.</p><button class="save-button" type="submit">Save monitor</button></footer>
    </form>
  </section>
</main>

<style>
  .create-page { min-height: 100svh; padding: 24px 20px 48px; background: var(--page); color: var(--ink); }.create-header, .create-shell { width: min(100%, 1080px); margin: 0 auto; }.create-header { display: flex; align-items: center; justify-content: space-between; }.back-link { color: var(--secondary); font-size: 0.8125rem; text-underline-offset: 3px; }.prototype-chip { padding: 5px 8px; border-radius: 999px; color: var(--blue); background: rgb(96 165 250 / 13%); font-size: 0.6875rem; }.create-shell { margin-top: 64px; overflow: hidden; border: 1px solid var(--border); border-radius: 12px; background: var(--surface); box-shadow: 0 4px 7px rgb(0 0 0 / 3%); }.create-heading { padding: 30px 30px 26px; border-bottom: 1px solid var(--border); }.create-heading p { margin-bottom: 7px; color: var(--muted); font-size: 0.6875rem; letter-spacing: 0.02em; text-transform: uppercase; }.create-heading h1 { font-size: 1.65rem; font-weight: 680; letter-spacing: -0.03em; line-height: 1.25; }.create-heading span { display: block; margin-top: 8px; color: var(--secondary); font-size: 0.8125rem; line-height: 1.5; }.form-columns { display: grid; grid-template-columns: minmax(0, 1fr) minmax(0, 0.9fr); }.form-section { padding: 28px 30px 34px; }.right-column { border-left: 1px solid var(--border); }.right-column .form-section + .form-section { border-top: 1px solid var(--border); }.form-section h2 { margin-bottom: 24px; font-size: 1rem; font-weight: 700; line-height: 1.35; }.form-section label { display: block; margin-top: 18px; color: var(--secondary); font-size: 0.8125rem; font-weight: 600; line-height: 1.4; }.form-section label:first-of-type { margin-top: 0; }.form-section label span { color: var(--muted); font-weight: 400; }.form-section input, .form-section select, .form-section textarea { display: block; width: 100%; margin-top: 7px; border: 1px solid var(--border-light); border-radius: 5px; color: var(--ink); background: var(--page); font: inherit; font-size: 0.8125rem; }.form-section input, .form-section select { height: 42px; padding: 0 12px; }.form-section textarea { min-height: 128px; padding: 12px; resize: vertical; line-height: 1.5; }.form-section input::placeholder, .form-section textarea::placeholder { color: var(--muted); }.form-section input:focus, .form-section select:focus, .form-section textarea:focus { border-color: var(--green); outline: 2px solid rgb(16 185 129 / 18%); outline-offset: 1px; }.form-section small { display: block; margin-top: 6px; color: var(--muted); font-size: 0.6875rem; line-height: 1.4; }.empty-setting { color: var(--secondary); font-size: 0.8125rem; line-height: 1.5; }.secondary-action { min-height: 36px; margin-top: 16px; padding: 0 14px; border: 1px solid var(--border-light); border-radius: 5px; color: var(--secondary); background: transparent; font-size: 0.8125rem; cursor: pointer; }.secondary-action:hover { color: var(--ink); border-color: var(--secondary); }.form-footer { display: flex; align-items: center; justify-content: space-between; gap: 20px; padding: 14px 18px; border-top: 1px solid var(--border); background: rgb(15 18 26 / 50%); }.form-footer p { color: var(--muted); font-size: 0.6875rem; line-height: 1.4; }.save-button { min-height: 38px; padding: 0 18px; border: 1px solid var(--green); border-radius: 5px; color: var(--page); background: var(--green); font-size: 0.8125rem; font-weight: 650; cursor: pointer; }.save-button:hover { filter: brightness(1.08); }:focus-visible { outline: 2px solid var(--green); outline-offset: 3px; }
  @media (max-width: 760px) { .create-page { padding: 18px 14px 36px; }.create-shell { margin-top: 36px; }.form-columns { display: block; }.right-column { border-top: 1px solid var(--border); border-left: 0; }.form-section { padding: 24px 20px 28px; } }
  @media (max-width: 440px) { .create-header { align-items: flex-start; flex-direction: column; gap: 12px; }.create-heading { padding: 24px 20px 22px; }.create-heading h1 { font-size: 1rem; }.form-footer { align-items: flex-start; flex-direction: column-reverse; }.save-button { width: 100%; } }
</style>
