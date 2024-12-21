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
      src="/creative/film2.jpg"
      alt="Contact header"
      class="header-image w-full h-[40vh] object-cover"
    />
    <div class="absolute inset-0 bg-black/30 flex items-center justify-center">
      <h1 class="title text-6xl md:text-8xl font-bold text-white">Contact</h1>
    </div>
  </div>

  <div class="mx-auto px-4 pt-16 pb-24">
    <div class="content max-w-xl mx-auto">
      <form
        action="https://api.web3forms.com/submit"
        method="POST"
        class="space-y-6"
      >
        <input
          type="hidden"
          name="access_key"
          value="165c37df-5e3a-47ec-9e4c-468edbbdce7d"
        />

        <!-- Name Field -->
        <div>
          <label for="name" class="block text-gray-700 font-medium mb-2"
            >Name</label
          >
          <input
            type="text"
            id="name"
            name="name"
            required
            class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-primary-500 focus:border-transparent"
          />
        </div>

        <!-- Email Field -->
        <div>
          <label for="email" class="block text-gray-700 font-medium mb-2"
            >Email</label
          >
          <input
            type="email"
            id="email"
            name="email"
            required
            class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-primary-500 focus:border-transparent"
          />
        </div>

        <!-- Message Field -->
        <div>
          <label for="message" class="block text-gray-700 font-medium mb-2"
            >Message</label
          >
          <textarea
            id="message"
            name="message"
            required
            rows="6"
            class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-primary-500 focus:border-transparent"
          ></textarea>
        </div>

        <!-- Honeypot -->
        <input
          type="checkbox"
          name="botcheck"
          class="hidden"
          style="display: none;"
        />

        <!-- Submit Button -->
        <button
          type="submit"
          class="w-full bg-gray-900 text-white py-3 px-6 rounded-lg hover:bg-gray-800 transition-colors duration-300"
        >
          Send Message
        </button>
      </form>
    </div>
  </div>
</main>
<Footer />
