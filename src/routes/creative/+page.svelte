<script lang="ts">
  import Header from '$lib/components/Header.svelte';
  import MasonryGallery from '$lib/components/MasonryGallery.svelte';
  import Footer from '$lib/components/Footer.svelte';

  const items = [
    {
      image: '/video/bgvideo.mp4',
      isVideo: true
    },

    {
      image: '/creative/workingoutloud.png',
      title: 'Working Out Loud',
      description:
        'Storytelling workshop for Brown University students interested in exploring audio production as a tool for examining themes of race, social justice, and freedom'
    },
    {
      image: '/work/watermelon.jpg',
      title: 'Untitled (Cracked Watermelon)',
      description:
        'Audio story on painting by African American still life painter Charles Ethan Porter',
      audio: '/creative/watermelon.mp3'
    },
    {
      image: '/creative/film1.jpg'
    },

    {
      image: '/creative/inournature.webp',
      title: 'In Our Nature',
      link: 'https://vimeo.com/109858182'
    },

    {
      image: '/creative/travel-1.jpeg'
    },

    {
      image: '/creative/reimagine.png',
      title: 'Reimagining DC',
      description:
        'Web experience & audio story featuring three culture workers reimagining art and audience in Washington, D.C.',
      link: 'https://reimaginedc.art'
    },
    {
      image: '/creative/film3.jpg'
    },
    {
      image: '/creative/hair-close.jpeg'
    },

    {
      image: '/creative/ocean-rocks.jpeg'
    },
    {
      image: '/creative/museum2.jpg'
    },
    {
      image: '/creative/film2.jpg'
    },

    {
      image: '/creative/form2.jpg'
    },

    {
      image: '/creative/hair-salon.jpeg'
    },
    {
      image: '/creative/blacknoise.webp',
      title: 'Black Noise',
      description: 'Video art exploring themes of sound and silence',
      link: 'https://vimeo.com/91730667'
    },
    {
      image: '/creative/image5.jpg'
    },
    {
      image: '/creative/form4.jpg'
    },
    {
      image: '/creative/form5.jpg'
    },
    {
      image: '/creative/travel-3.jpeg'
    },
    {
      image: '/creative/form1.jpg'
    }
  ];
</script>

<Header />

<main class="min-h-screen bg-white">
  <div class="relative h-[40vh] mb-16">
    <div
      class="absolute inset-0 bg-cover bg-center bg-no-repeat"
      style="background-image: url('/bg2.JPG')"
    >
      <div class="absolute inset-0 bg-black/40" />
    </div>
    <div
      class="relative h-full flex flex-col items-center justify-center text-white px-4"
    >
      <h1 class="text-5xl font-bold mb-4">Creative</h1>
      <p class="text-xl max-w-2xl text-center">
        A mix of old and new projects, experiments, and tinkerings
      </p>
    </div>
  </div>

  <div class="mx-auto px-4 pb-16">
    <MasonryGallery {items} />
  </div>
</main>
<Footer />
