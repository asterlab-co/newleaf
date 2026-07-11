<script lang="ts">
  import type { Snippet } from 'svelte'

  let { children }: { children: Snippet } = $props()
</script>

<div class="screen-content">
  {@render children()}
</div>

<style>
  .screen-content {
    align-items: start;
    display: flex;
    flex-direction: column-reverse;
    gap: clamp(2rem, 4.17vw, 2.5rem);
    justify-content: end;
    min-height: 100vh;
    padding: clamp(1.25rem, 4.17vw, 2.5rem) clamp(1.25rem, 4.17vw, 2.5rem) clamp(2rem, 5.83vw, 3.5rem);
  }

  @media (min-width: 40rem) {
    .screen-content {
      flex-direction: column;
    }
  }
</style>
