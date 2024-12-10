<script lang="ts">
  import { onMount } from 'svelte';
  import gsap from 'gsap';
  import Header from '$lib/components/Header.svelte';

  onMount(() => {
    const tl = gsap.timeline({
      defaults: { duration: 1, ease: 'power3.out' }
    });

    gsap.set('.name-text', {
      opacity: 0,
      y: -50
    });

    tl.to('.name-text', {
      opacity: 1,
      y: 0,
      duration: 1.2
    });
  });
</script>

<Header />

<main
  class="min-h-screen relative overflow-hidden bg-cover bg-center bg-no-repeat"
  style="background-image: url('/bg3.jpg')"
>
  <div class="absolute inset-0 bg-black/30"></div>

  <span
    class="name-text text-4xl sm:text-6xl md:text-8xl font-bold absolute top-1/4 left-1/2 -translate-x-1/2 -translate-y-1/2 z-20 text-white text-center px-4"
  >
    Johanna Obenda
  </span>
</main>

<style>
  .name-text {
    transform-origin: center center;
    will-change: transform;
  }
</style>
