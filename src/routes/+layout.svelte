<script lang="ts">
  import { onMount } from 'svelte';
  import '../app.css';

  let { children } = $props();

  onMount(() => {
    const labelSidebarLinks = () => {
      document.querySelectorAll<HTMLElement>('.sidebar-nav a:not([aria-label])').forEach((link) => {
        link.setAttribute('aria-label', link.textContent?.trim() || 'Navigation item');
      });
    };

    labelSidebarLinks();
    const observer = new MutationObserver(labelSidebarLinks);
    observer.observe(document.body, { childList: true, subtree: true });
    return () => observer.disconnect();
  });
</script>

{@render children()}
