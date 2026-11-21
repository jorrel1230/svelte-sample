<script>
  import { onMount } from 'svelte';

  let { duration, storageKey, label = 'Timer' } = $props();
  let end = $state(null);
  let paused = $state(null);
  let now = $state(Date.now());
  let ready = $state(false);
  let remaining = $derived(end === null ? (paused ?? duration) : Math.max(0, (end - now) / 1000));
  let running = $derived(end !== null && remaining > 0);

  onMount(() => {
    try {
      const saved = JSON.parse(localStorage.getItem(storageKey));
      if (typeof saved === 'number' && saved > 0) end = saved;
      else if (saved && typeof saved.end === 'number') end = saved.end;
      else if (saved && typeof saved.paused === 'number') paused = saved.paused;
    } catch { /* Storage is optional. */ }
    now = Date.now();
    ready = true;
    const interval = setInterval(() => { now = Date.now(); }, 250);
    return () => clearInterval(interval);
  });

  function toggle() {
    now = Date.now();
    if (end !== null && end > now) {
      paused = (end - now) / 1000;
      end = null;
    } else {
      end = now + Math.max(0, paused ?? duration) * 1000;
      paused = null;
    }
    try { localStorage.setItem(storageKey, JSON.stringify({ end, paused })); } catch { /* Keep state in memory. */ }
  }

  function reset() {
    end = null;
    paused = null;
    try { localStorage.removeItem(storageKey); } catch { /* Keep state in memory. */ }
  }

  function format(seconds) {
    const s = Math.max(0, Math.ceil(seconds));
    return `${Math.floor(s / 60)}:${String(s % 60).padStart(2, '0')}`;
  }
</script>

<div class="timer" aria-label={label}>
  <span>{label}: <strong>{format(remaining)}</strong></span>
  <button class="control" onclick={toggle} disabled={!ready} aria-label={running ? 'Pause timer' : (end !== null && remaining === 0 ? 'Restart timer' : 'Start timer')}>
    <svg class="progress" viewBox="0 0 44 44" aria-hidden="true">
      <circle class="track" cx="22" cy="22" r="19" />
      <circle class="arc" cx="22" cy="22" r="19" pathLength="1" stroke-dasharray="1" stroke-dashoffset={1 - (duration > 0 ? Math.min(1, Math.max(0, (duration - remaining) / duration)) : 0)} />
    </svg>
    <svg class="icon" viewBox="0 0 24 24" aria-hidden="true">
      {#if running}<path d="M7 5h4v14H7zm6 0h4v14h-4z" />{:else}<path d="M8 5v14l11-7z" />{/if}
    </svg>
  </button>
  {#if end !== null || paused !== null}
    <button class="reset" onclick={reset} aria-label="Reset timer" title="Reset timer">
      <svg viewBox="0 0 24 24" aria-hidden="true"><path d="M4 4v5h5M5 9a8 8 0 1 1-1 5" /></svg>
    </button>
  {/if}
</div>

<style>
  .timer { display: flex; align-items: center; flex-wrap: wrap; gap: .75rem; }
  .control { position: relative; display: grid; place-items: center; width: 3rem; height: 3rem; padding: 0; border: 0; background: transparent; color: #252521; cursor: pointer; }
  .control:not(:disabled):hover { background: #e5e5df; border-radius: 50%; }
  .control:focus-visible { outline: 2px solid #252521; outline-offset: 2px; border-radius: 50%; }
  .control:disabled { opacity: .5; cursor: default; }
  .progress { position: absolute; inset: 0; width: 100%; height: 100%; transform: rotate(-90deg); }
  circle { fill: none; stroke-width: 2.5; }
  .track { stroke: #deded8; }
  .arc { stroke: currentColor; stroke-linecap: round; }
  .icon { width: 1.2rem; height: 1.2rem; fill: currentColor; }
  .reset { display: grid; place-items: center; width: 2.5rem; height: 2.5rem; padding: .6rem; border: 0; border-radius: 50%; background: transparent; color: #252521; cursor: pointer; }
  .reset:hover { background: #e5e5df; }
  .reset:focus-visible { outline: 2px solid #252521; outline-offset: 2px; }
  .reset svg { width: 100%; height: 100%; fill: none; stroke: currentColor; stroke-width: 1.8; stroke-linecap: round; stroke-linejoin: round; }
</style>
