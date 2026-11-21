<script>
  import Checklist from '../../lib/components/Checklist.svelte';

  let checklist;
  const items = [
    { id: 'water', label: 'Fill a water bottle' },
    { id: 'shoes', label: 'Pack walking shoes' },
    { id: 'map', label: 'Download the trail map' }
  ];
</script>

<svelte:head><title>Checklist example</title></svelte:head>
<main>
  <p class="kicker">COMPONENT EXAMPLE</p>
  <h1>Ready for the trail.</h1>
  <p class="lead">A simple packing list. Your selections stay on this device.</p>
  <hr class="rule">
  <Checklist bind:this={checklist} {items} storageKey="example-trail-checklist" label="Before you leave" />
  <p><button onclick={() => checklist.reset()}>Clear checklist</button></p>
</main>
