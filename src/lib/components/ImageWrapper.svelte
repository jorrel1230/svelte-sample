<script>
  let { src, alt, position = 'center', height = '320px', children } = $props();
</script>

<div class="image-wrapper" style:height>
  <img {src} {alt} style:object-position={position}>
  {#if children}<div class="content">{@render children()}</div>{/if}
</div>

<style>
  .image-wrapper { position: relative; overflow: hidden; }
  img { display: block; width: 100%; height: 100%; object-fit: cover; }
  .content { position: absolute; inset: 0; }
</style>
