<script lang="ts">
  import { onMount } from 'svelte';
  import gsap from 'gsap';
  import Gallery from '$lib/components/Gallery.svelte';
  import MasonryGallery from '$lib/components/MasonryGallery.svelte';
  import Header from '$lib/components/Header.svelte';
  import Footer from '$lib/components/Footer.svelte';

  interface GalleryItem {
    image: string;
    title: string;
    description: string;
    link: string;
  }

  interface $$Props {
    items?: GalleryItem[];
    sectionId?: string;
  }

  export let items: GalleryItem[] = [];
  export let sectionId: string = '';

  const curationItems = [
    {
      image: '/gallery1.png',
      title: "In Slavery's Wake: Making Black Freedom in the World",
      description: 'Co-Curator, Lead Exhibition Developer',
      link: 'https://nmaahc.si.edu/explore/exhibitions/in-slaverys-wake',
      backgroundColor: '#2b3081'
    },
    {
      image: '/gallery2.jpg',
      title: 'Memory Dishes: Women and African Diasporic Cooking',
      description: 'Lead Curator',
      link: 'https://sites.brown.edu/memorydishes/'
    },
    {
      image: '/gallery3.png',
      title: 'A Seat @ The Table',
      description: 'Co-Curator',
      link: ''
    }
  ];

  const educationItems = [
    {
      image: '/gallery4.jpg',
      title: 'Studio Museum in Harlem',
      description: 'Graduate Practicum, Public Programs',
      link: 'https://www.studiomuseum.org/about'
    },
    {
      image: '/work/bath.jpg',
      title: 'Yale University Art Gallery,',
      description: 'Cullman-Payson Fellow in Academic Affairs and Outreach',
      link: 'https://artgallery.yale.edu/research-and-learning/education'
    },
    {
      image: '/work/watermelon.jpg',
      title: 'Metropolitan Museum of Art',
      description: '12-Month Post-Graduate Education Intern',
      link: 'https://www.metmuseum.org/learn/learning-resources'
    }
  ];

  const writingItems = [
    {
      image: '/work/isw1.png',
      title: `Co-Editor\n<i>In Slavery's Wake: Making Black Freedom in the World</i>`,
      description: '2024',
      link: 'https://www.smithsonianbooks.com/store/national-museum-african-american-history-and-cultu/in-slaverys-wake-making-black-freedom-in-the-world/'
    },
    {
      image: '/work/mortevivum.png',
      title: `Provocations: Kimberly Juanita Brown's <i>Mortevivium</i>`,
      description: '2024',
      link: 'https://asapjournal.com/review/provocations-live-forever-rendering-a-history-and-present-of-photography-in-kimberly-juanita-browns-mortevivium/'
    },
    {
      image: '/creative/reimagine.png',
      title: `<i>Reimagining Arts Engagement</i>\nFaith Flanagan Fellowsip, ArtTable D.C.`,
      description: '2022',
      link: 'http://reimaginedc.art'
    },
    {
      image: '/work/poh.avif',
      title: `'The Power of Hair'\nThe Metropolitan Museum of Art`,
      description: '2016',
      link: 'https://www.metmuseum.org/perspectives/power-hair'
    },
    {
      image: '/work/memorydishes.png',
      title: '<i>Memory Dishes</i> Catalog\nBrown University',
      description: '2019',
      link: 'https://sites.brown.edu/memorydishes/meet-the-families/'
    },
    {
      image: '/work/bellgallery1.png',
      title:
        '<i>The Kitchen Table: Through the Lens of Carrie Mae Weems and Others</i>',
      description: '2017',
      link: 'https://bellgallery.wordpress.com/author/davidwintonbellgallery/'
    }
  ];

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
  <div class="relative h-[40vh] mb-16">
    <img
      src="/work.png"
      alt="Work header"
      class="header-image absolute inset-0 w-full h-full object-cover"
    />
    <div class="absolute inset-0 bg-black/30" />
    <div
      class="relative h-full flex flex-col items-center justify-center text-white px-4"
    >
      <h1 class="title text-5xl font-bold mb-4">Work</h1>
      <p class="text-xl max-w-2xl text-center">
        Curation, writing, education, and public programming
      </p>
    </div>
  </div>

  <div class="mx-auto px-4 pb-24">
    <div class="mt-16 space-y-24">
      <!-- Curation Section -->
      <section class="curation-section">
        <h2 class="text-4xl font-bold text-gray-900 mb-8 text-center">
          Curation
        </h2>
        <Gallery items={curationItems} sectionId="curation" />
      </section>

      <!-- Writing & Research Section -->
      <div class="mt-24 mb-16">
        <h2 class="text-4xl font-bold text-center mb-12">Writing & Research</h2>
        <MasonryGallery items={writingItems} />
      </div>

      <!-- Education & Programs Section -->
      <section class="education-section">
        <h2 class="text-4xl font-bold text-gray-900 mb-8 text-center">
          Education & Programs
        </h2>
        <Gallery items={educationItems} sectionId="education" />
      </section>
    </div>
  </div>
</main>
<Footer />
