<svelte:head>
  <title>Sign in — Znayu</title>
  <meta
    name="description"
    content="Sign in to the Znayu operator console."
  />
</svelte:head>

<main class="login-page">
  <div class="login-shell">
    <header class="login-header">
      <a class="wordmark" href="/status/acme-cloud" aria-label="Back to Znayu status page">znayu</a>
      <span class="access-label">Operator access</span>
    </header>

    <section class="login-register" aria-labelledby="login-title">
      <div class="login-intro">
        <span class="status-mark" aria-hidden="true"></span>
        <p>Private console</p>
      </div>

      <h1 id="login-title">Sign in to Znayu</h1>
      <p class="login-description">
        Manage monitors, maintenance windows, and the status your visitors rely on.
      </p>

      <form class="login-form" onsubmit={(event) => event.preventDefault()}>
        <label for="email">Email address</label>
        <input id="email" name="email" type="email" autocomplete="email" placeholder="you@example.com" />

        <div class="field-heading">
          <label for="password">Password</label>
          <a href="#forgot-password">Forgot password?</a>
        </div>
        <input id="password" name="password" type="password" autocomplete="current-password" />

        <button type="submit">Continue to console</button>
      </form>

      <p class="prototype-note">
        Authentication is not connected in this prototype.
      </p>
    </section>

    <footer class="login-footer">
      <a href="/status/acme-cloud">View public status</a>
      <span aria-hidden="true">·</span>
      <span>Znayu self-hosted status</span>
    </footer>
  </div>
</main>

<style>
  .login-page {
    min-height: 100svh;
    padding: 0 20px 32px;
    background: var(--page);
  }

  .login-shell {
    display: flex;
    min-height: calc(100svh - 32px);
    width: min(100%, 800px);
    margin: 0 auto;
    flex-direction: column;
  }

  .login-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    min-height: 64px;
    padding: 12px 0;
  }

  .wordmark {
    color: var(--ink);
    font-size: 1.65rem;
    font-weight: 720;
    letter-spacing: -0.045em;
    line-height: 1;
    text-decoration: none;
    transition: opacity 140ms ease-out;
  }

  .wordmark:hover {
    opacity: 0.78;
  }

  .access-label {
    color: var(--muted);
    font-size: 0.6875rem;
    font-weight: 600;
    letter-spacing: 0.02em;
    text-transform: uppercase;
  }

  .login-register {
    width: min(100%, 432px);
    margin: auto;
    padding: 36px 36px 32px;
    border: 1px solid var(--border);
    border-radius: 12px;
    background: var(--surface);
    box-shadow: 0 4px 7px rgb(0 0 0 / 3%);
  }

  .login-intro {
    display: flex;
    align-items: center;
    gap: 9px;
    color: var(--green);
    font-size: 0.6875rem;
    font-weight: 600;
    letter-spacing: 0.02em;
    text-transform: uppercase;
  }

  .login-intro p {
    margin: 0;
  }

  .status-mark {
    width: 7px;
    height: 7px;
    border-radius: 50%;
    background: var(--green);
    box-shadow: 0 0 0 4px rgb(16 185 129 / 12%);
  }

  h1 {
    margin: 20px 0 0;
    color: var(--ink);
    font-size: 1.65rem;
    font-weight: 680;
    letter-spacing: -0.03em;
    line-height: 1.25;
  }

  .login-description {
    max-width: 42ch;
    margin: 10px 0 0;
    color: var(--secondary);
    font-size: 0.8125rem;
    line-height: 1.55;
  }

  .login-form {
    display: grid;
    gap: 9px;
    margin-top: 28px;
  }

  label,
  .field-heading a {
    color: var(--secondary);
    font-size: 0.6875rem;
    font-weight: 600;
    line-height: 1.4;
  }

  .field-heading {
    display: flex;
    align-items: baseline;
    justify-content: space-between;
    gap: 12px;
    margin-top: 7px;
  }

  .field-heading a {
    color: var(--muted);
    font-size: 0.6875rem;
    font-weight: 500;
    text-underline-offset: 3px;
  }

  input {
    width: 100%;
    min-height: 42px;
    padding: 0 12px;
    border: 1px solid var(--border-light);
    border-radius: 5px;
    color: var(--ink);
    background: var(--page);
    font-size: 0.8125rem;
    line-height: 1;
    transition: border-color 140ms ease-out, background-color 140ms ease-out;
  }

  input::placeholder {
    color: var(--muted);
  }

  input:hover {
    border-color: var(--secondary);
  }

  input:focus {
    border-color: var(--green);
    background: var(--surface-hover);
    outline: none;
  }

  button {
    min-height: 42px;
    margin-top: 10px;
    padding: 0 16px;
    border: 1px solid var(--green);
    border-radius: 5px;
    color: var(--page);
    background: var(--green);
    font-size: 0.8125rem;
    font-weight: 650;
    cursor: pointer;
    transition: filter 140ms ease-out, transform 140ms ease-out;
  }

  button:hover {
    filter: brightness(1.08);
  }

  button:active {
    transform: translateY(1px);
  }

  .prototype-note {
    margin: 20px 0 0;
    padding-top: 16px;
    border-top: 1px solid var(--border);
    color: var(--muted);
    font-size: 0.6875rem;
    line-height: 1.5;
  }

  .login-footer {
    display: flex;
    justify-content: center;
    gap: 9px;
    padding-top: 24px;
    color: var(--muted);
    font-size: 0.6875rem;
    line-height: 1.5;
  }

  .login-footer a {
    color: var(--secondary);
    text-underline-offset: 3px;
  }

  @media (max-width: 520px) {
    .login-page {
      padding-right: 12px;
      padding-left: 12px;
    }

    .login-header {
      min-height: 58px;
      padding-top: 10px;
    }

    .login-register {
      margin-top: 64px;
      padding: 28px 20px 24px;
    }

    .login-footer {
      flex-wrap: wrap;
      text-align: center;
    }
  }

  @media (prefers-reduced-motion: reduce) {
    *,
    *::before,
    *::after {
      transition-duration: 0.01ms !important;
    }
  }
</style>
