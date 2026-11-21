<script>
  import { onMount } from 'svelte';

  let { items = [], storageKey, label = 'Checklist' } = $props();
  let checked = $state([]);
  let ready = $state(false);

  onMount(() => {
    try {
      const saved = JSON.parse(localStorage.getItem(storageKey) || '[]');
      if (Array.isArray(saved)) checked = saved;
    } catch { /* Ignore unavailable or invalid storage. */ }
    ready = true;
  });

  function save() {
    try { localStorage.setItem(storageKey, JSON.stringify(checked)); } catch { /* Keep state in memory. */ }
  }

  function toggle(id) {
    checked = checked.includes(id) ? checked.filter((value) => value !== id) : [...checked, id];
    save();
  }

  export function reset() {
    checked = [];
    if (ready) save();
  }
</script>

<fieldset>
  <legend>{label}</legend>
  {#each items as item (item.id)}
    <label><input type="checkbox" checked={checked.includes(item.id)} onchange={() => toggle(item.id)} disabled={!ready}> {item.label}</label>
  {/each}
</fieldset>

<style>
  fieldset { display: grid; gap: .5rem; border: 1px solid #ddd; }
  label { display: flex; align-items: center; gap: .5rem; }
  input { appearance: none; width: 1.1rem; height: 1.1rem; margin: 0; border: 1.5px solid #252521; border-radius: 50%; display: grid; place-content: center; flex: none; cursor: pointer; }
  input:not(:disabled):hover { box-shadow: 0 0 0 4px #e5e5df; }
  input:checked { background: #252521; }
  input:checked::after { content: ''; width: .45rem; height: .25rem; border: solid white; border-width: 0 0 2px 2px; transform: translateY(-1px) rotate(-45deg); }
  input:focus-visible { outline: 2px solid #252521; outline-offset: 3px; }
</style>
