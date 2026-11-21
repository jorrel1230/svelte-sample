<script>
  import Timer from '../../lib/components/Timer.svelte';
</script>

<svelte:head><title>Timer example</title></svelte:head>
<main>
  <p class="kicker">COMPONENT EXAMPLE</p>
  <h1>Take a tea break.</h1>
  <p class="lead">Let your tea steep for three minutes. The timer keeps its end time even if you leave this page.</p>
  <hr class="rule">
  <Timer duration={180} storageKey="example-tea-timer" label="Steeping time" />
</main>
