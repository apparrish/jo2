<script lang="ts">
  import { onMount } from 'svelte';
  import gsap from 'gsap';
  import Header from '$lib/components/Header.svelte';
  import Footer from '$lib/components/Footer.svelte';

  onMount(() => {
    const tl = gsap.timeline({
      defaults: { duration: 1, ease: 'power3.out' }
    });

    tl.from('.header-image', {
      opacity: 0,
      duration: 1.2
    })
      .from(
        '.title',
        {
          y: 30,
          opacity: 0
        },
        '<+=0.3'
      )
      .from(
        '.content',
        {
          y: 20,
          opacity: 0
        },
        '<+=0.2'
      );
  });
</script>

<Header />

<main class="min-h-screen bg-white">
  <div class="relative">
    <img
      src="/about.png"
      alt="About header"
      class="header-image w-full h-[50vh] object-cover object-top"
    />
    <div class="absolute inset-0 bg-black/30 flex items-center justify-center">
      <h1 class="title text-6xl md:text-8xl font-bold text-white">About</h1>
    </div>
  </div>

  <div class="mx-auto px-4 pt-24 pb-32">
    <div
      class="content space-y-8 text-gray-800 text-lg w-[85%] md:w-2/3 lg:w-1/2 mx-auto"
    >
      <p>
        Johanna Obenda is a cultural practitioner and creative, exploring
        stories of the African diasporic experience through art, history, and
        culture. Her work ranges from museum object-interpretation to multimedia
        storytelling.
      </p>
      <p>Johanna is currently based in Washington, D.C.</p>
    </div>
  </div>
  <Footer />
</main>
