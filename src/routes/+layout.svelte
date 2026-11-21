<script>
  import { page } from '$app/state'
  import '../app.css'
  let { children } = $props()
</script>

<div class="page">
  <header>
    <span class="brand">A SMALL, PERSONAL SITE</span>
    <nav aria-label="Main navigation">
      <a class="nav" href="/" aria-current={page.url.pathname === '/' ? 'page' : undefined}>Home</a>
      <a class="nav" href="/about/" aria-current={page.url.pathname.replace(/\/$/, '') === '/about' ? 'page' : undefined}>About</a>
    </nav>
  </header>

  {@render children()}

  <footer>© 2026 Your name</footer>
</div>
