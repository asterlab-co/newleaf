<script lang="ts">
  import type { Snippet } from 'svelte'

  let {
    background,
    children,
  }: {
    /** Optional full-bleed background image URL for the screen. */
    background?: string
    children: Snippet
  } = $props()

  // Fade the image in only once it has actually loaded, so a new tab shows a
  // calm reveal over the base color instead of a pop-in. If it never loads,
  // the layer simply stays invisible.
  let loaded = $state(false)
  $effect(() => {
    if (!background) return
    const img = new Image()
    img.onload = () => (loaded = true)
    img.src = background
  })
</script>

<div class="screen">
  {#if background}
    <div
      class="screen__bg"
      class:screen__bg--visible={loaded}
      style:background-image={`url("${background}")`}
      aria-hidden="true"
    ></div>
  {/if}
  {@render children()}
</div>

<style>
  .screen {
    min-height: 100vh;
  }

  .screen__bg {
    background-position: center;
    background-repeat: no-repeat;
    background-size: cover;
    inset: 0;
    opacity: 0;
    position: fixed;
    transition: opacity 400ms ease;
    z-index: -1;
  }

  /* Backdrop blur lives on its own layer, masked so it ramps from sharp at
     the top edge of the band to fully blurred at the bottom. It must stay
     separate from ::after — a mask also hides the element's own background,
     so it would erase the gradient where it's darkest. */
  .screen__bg::before {
    backdrop-filter: blur(1.5rem);
    -webkit-backdrop-filter: blur(1.5rem);
    bottom: 0;
    content: '';
    height: 50rem;
    mask-image: linear-gradient(to bottom, transparent, black);
    -webkit-mask-image: linear-gradient(to bottom, transparent, black);
    position: absolute;
    width: 100%;
  }

  .screen__bg::after {
    background: linear-gradient(to bottom, transparent, var(--color-background-dark) 90%);
    bottom: 0;
    content: '';
    height: 50rem;
    position: absolute;
    width: 100%;
  }

  .screen__bg--visible {
    opacity: 1;
  }

  @media (prefers-reduced-motion: reduce) {
    .screen__bg {
      transition-duration: 0s;
    }
  }
</style>
