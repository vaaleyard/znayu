<script lang="ts">
  type DayState = 'operational' | 'degraded' | 'unavailable';

  type Service = {
    name: string;
    uptime: string;
    days: DayState[];
  };

  const uptimeDays = (degraded: number[] = [], unavailable: number[] = []): DayState[] =>
    Array.from({ length: 90 }, (_, index) => {
      if (unavailable.includes(index)) return 'unavailable';
      if (degraded.includes(index)) return 'degraded';
      return 'operational';
    });

  const services: Service[] = [
    { name: 'Main website', uptime: '99.98%', days: uptimeDays() },
    { name: 'Public API', uptime: '99.97%', days: uptimeDays([], [88]) },
    { name: 'Database', uptime: '100%', days: uptimeDays() },
    { name: 'Transactional email', uptime: '99.95%', days: uptimeDays() }
  ];
</script>

<svelte:head>
  <title>Znayu — Service status</title>
  <meta
    name="description"
    content="Real-time service availability and historical uptime information."
  />
</svelte:head>

<main class="status-page">
  <div class="status-shell">
    <header class="site-header">
      <a class="wordmark" href="#status" aria-label="Znayu status page">znayu</a>
      <div class="header-navigation">
        <nav class="primary-nav" aria-label="Status page navigation">
          <a class="active" href="#status" aria-current="page">Overview</a>
          <a href="#maintenance">Maintenance</a>
          <a href="#incidents">History</a>
        </nav>
        <div class="header-action">
          <button type="button" disabled aria-describedby="prototype-note">Subscribe</button>
        </div>
      </div>
    </header>

    <section class="overall-health" id="status" aria-labelledby="overall-health-title">
      <div class="health-copy">
        <span class="health-dot" aria-hidden="true"></span>
        <div>
          <h1 id="overall-health-title">All systems operational</h1>
        </div>
      </div>
      <div class="health-meta">
        <p><strong>4 of 4</strong> services operational</p>
        <time datetime="2026-08-16T00:01:00-03:00">Updated 1 minute ago</time>
      </div>
    </section>

    <section class="services-section" aria-label="Monitored services">
      <div class="services-panel">
        <div class="service-list">
          {#each services as service}
            <article class="service-row">
              <div class="service-heading">
                <div class="service-name">
                  <span aria-hidden="true"></span>
                  <h3>{service.name}</h3>
                </div>
                <div class="service-result">
                  <span>Operational</span>
                  <strong>{service.uptime}</strong>
                </div>
              </div>

              <div
                class="uptime-rail"
                role="img"
                aria-label={`${service.name}: ${service.uptime} uptime over the last 90 days. Each segment represents one day.`}
              >
                {#each service.days as day, index}
                  <span
                    class:degraded={day === 'degraded'}
                    class:unavailable={day === 'unavailable'}
                    aria-hidden="true"
                    title={`Day ${index + 1}: ${day === 'operational' ? 'operational' : day === 'degraded' ? 'degraded' : 'unavailable'}`}
                  ></span>
                {/each}
              </div>

              <div class="rail-range" aria-hidden="true">
                <span class="desktop-range">90 days ago</span>
                <span class="mobile-range">30 days ago</span>
                <span>Today</span>
              </div>
            </article>
          {/each}
        </div>
      </div>
    </section>

    <section class="maintenance-section" id="maintenance" aria-labelledby="maintenance-title">
      <h2 id="maintenance-title">Scheduled maintenance</h2>
      <article class="maintenance-item">
        <time datetime="2026-08-18">August 18, 2026</time>
        <div>
          <h3>Database upgrade</h3>
          <p>Scheduled between 02:00 and 03:00 BRT. Services may experience brief interruptions.</p>
        </div>
        <span>Scheduled</span>
      </article>
    </section>

    <section class="incident-section" id="incidents" aria-labelledby="incidents-title">
      <h2 id="incidents-title">Previous incidents</h2>
      <p class="incident-date">August 15, 2026</p>
      <article class="incident-item">
        <header>
          <h3>Elevated Public API latency</h3>
          <span>Resolved</span>
        </header>
        <div class="incident-update">
          <i aria-hidden="true"></i>
          <div>
            <strong>Resolved</strong>
            <time datetime="2026-08-15T14:50:00-03:00">14:50 BRT</time>
            <p>API response times returned to normal levels. The incident lasted 18 minutes.</p>
          </div>
        </div>
      </article>
    </section>

    <footer class="page-footer" id="prototype-note">
      Sample data · Znayu status page
    </footer>
  </div>
</main>
