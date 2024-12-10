<script lang="ts">
  import { onMount, onDestroy } from 'svelte';
  import gsap from 'gsap';

  interface GalleryItem {
    image: string;
    title?: string;
    description?: string;
    link?: string;
    isVideo?: boolean;
    audio?: string;
  }

  export let items: GalleryItem[] = [];
  let selectedItem: GalleryItem | null = null;
  let selectedIndex = -1;
  let clickedImageElement: HTMLElement | null = null;
  let modalImage: HTMLElement | null = null;
  let audio: HTMLAudioElement;
  let progress = 0;
  let isPlaying = false;
  let duration = 0;
  let mounted = false;
  let centerItemIndex = -1;
  let isMobile = false;

  onMount(() => {
    mounted = true;
    // Check if mobile on mount
    isMobile = window.innerWidth < 640;

    // Update centerItemIndex on scroll
    const handleScroll = () => {
      if (!isMobile) return;

      const galleryItems = document.querySelectorAll('.gallery-item');
      const viewportCenter = window.innerHeight / 2 + window.scrollY;

      let closestItem = null;
      let closestDistance = Infinity;
      let closestIndex = -1;

      galleryItems.forEach((item, index) => {
        const rect = item.getBoundingClientRect();
        const itemCenter = rect.top + window.scrollY + rect.height / 2;
        const distance = Math.abs(viewportCenter - itemCenter);

        if (distance < closestDistance) {
          closestDistance = distance;
          closestItem = item;
          closestIndex = index;
        }
      });

      centerItemIndex = closestIndex;
    };

    // Update isMobile on resize
    const handleResize = () => {
      isMobile = window.innerWidth < 640;
      if (!isMobile) centerItemIndex = -1;
      else handleScroll();
    };

    window.addEventListener('scroll', handleScroll);
    window.addEventListener('resize', handleResize);

    // Initial check
    handleScroll();

    return () => {
      window.removeEventListener('scroll', handleScroll);
      window.removeEventListener('resize', handleResize);
      if (mounted) {
        document.body.style.overflow = 'auto';
      }
    };
  });

  const openModal = (item: GalleryItem, index: number) => {
    selectedItem = item;
    selectedIndex = index;
    if (mounted) {
      document.body.style.overflow = 'hidden';
    }
  };

  const closeModal = () => {
    if (audio) {
      audio.pause();
      audio.currentTime = 0;
    }
    selectedItem = null;
    if (mounted) {
      document.body.style.overflow = 'auto';
    }
  };

  const handleBackdropClick = (event: MouseEvent) => {
    // Only close if clicking the backdrop itself, not its children
    if (event.target === event.currentTarget) {
      closeModal();
    }
  };

  const formatTime = (seconds: number) => {
    const minutes = Math.floor(seconds / 60);
    const remainingSeconds = Math.floor(seconds % 60);
    return `${minutes}:${remainingSeconds.toString().padStart(2, '0')}`;
  };

  const handlePlayPause = () => {
    if (!audio) return;
    if (isPlaying) {
      audio.pause();
    } else {
      audio.play();
    }
    isPlaying = !isPlaying;
  };

  const updateProgress = () => {
    if (!audio) return;
    progress = (audio.currentTime / audio.duration) * 100;
  };

  const handleTimeUpdate = () => {
    if (!audio) return;
    updateProgress();
  };

  const handleClick = (event: MouseEvent) => {
    if (!audio) return;
    const rect = (
      event.currentTarget as HTMLDivElement
    ).getBoundingClientRect();
    const x = event.clientX - rect.left;
    const percentage = x / rect.width;
    audio.currentTime = percentage * audio.duration;
    updateProgress();
  };
</script>

<div class="columns-1 sm:columns-2 md:columns-3 gap-4">
  {#each items as item, i}
    {#if item.link}
      <a
        href={item.link}
        target="_blank"
        rel="noopener noreferrer"
        class="gallery-item block break-inside-avoid mb-4 relative cursor-pointer overflow-hidden rounded-lg"
      >
        {#if item.isVideo}
          <video
            src={item.image}
            class="w-full aspect-video object-cover"
            autoplay
            loop
            muted
            playsinline
          />
        {:else}
          <img src={item.image} alt={item.title || ''} class="w-full" />
        {/if}

        {#if (item.title || item.description || item.link) && (!isMobile || i === centerItemIndex)}
          <div
            class="absolute inset-0 bg-black/70 flex flex-col justify-center items-center text-center p-4 text-white
                   sm:opacity-0 sm:hover:opacity-100 transition-opacity duration-300"
          >
            {#if item.title}
              <h3 class="text-lg font-bold mb-2 whitespace-pre-line">
                {@html item.title}
              </h3>
            {/if}
            {#if item.description}
              <p class="text-sm mb-4">{@html item.description}</p>
            {/if}
            <span
              class="mt-4 inline-flex items-center justify-center px-6 py-2 border-2 border-white text-sm font-medium text-white
                     hover:bg-white hover:text-black transition-colors duration-200 rounded-full"
            >
              View →
            </span>
          </div>
        {/if}
      </a>
    {:else}
      <div
        class="gallery-item break-inside-avoid mb-4 relative cursor-pointer overflow-hidden rounded-lg"
        on:click={() => openModal(item, i)}
        on:keydown={(e) => e.key === 'Enter' && openModal(item, i)}
        role="button"
        tabindex="0"
      >
        {#if item.isVideo}
          <video
            src={item.image}
            class="w-full aspect-video object-cover"
            autoplay
            loop
            muted
            playsinline
          />
        {:else}
          <img src={item.image} alt={item.title || ''} class="w-full" />
        {/if}

        {#if (item.title || item.description || item.link) && (!isMobile || i === centerItemIndex)}
          <div
            class="absolute inset-0 bg-black/70 flex flex-col justify-center items-center text-center p-4 text-white
                   sm:opacity-0 sm:hover:opacity-100 transition-opacity duration-300"
          >
            {#if item.title}
              <h3 class="text-lg font-bold mb-2 whitespace-pre-line">
                {@html item.title}
              </h3>
            {/if}
            {#if item.description}
              <p class="text-sm mb-4">{@html item.description}</p>
            {/if}
            {#if item.link}
              <a
                href={item.link}
                class="mt-4 inline-flex items-center justify-center px-6 py-2 border-2 border-white text-sm font-medium text-white
                       hover:bg-white hover:text-black transition-colors duration-200 rounded-full"
                on:click|stopPropagation
                target="_blank"
                rel="noopener noreferrer"
              >
                View →
              </a>
            {/if}
          </div>
        {/if}
      </div>
    {/if}
  {/each}
</div>

{#if selectedItem}
  <div
    class="fixed inset-0 bg-black/80 flex items-center justify-center p-4 z-50"
    on:click={handleBackdropClick}
  >
    <div class="relative max-w-4xl w-full bg-white rounded-lg overflow-hidden">
      <button
        class="absolute top-4 right-4 text-white z-10 bg-black/50 p-2 rounded-full hover:bg-black/70 transition-colors"
        on:click={closeModal}
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 24 24"
          stroke="currentColor"
          class="w-6 h-6"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M6 18L18 6M6 6l12 12"
          />
        </svg>
      </button>

      <div class="relative aspect-video">
        {#if selectedItem.isVideo}
          <video
            src={selectedItem.image}
            class="w-full h-full object-cover"
            autoplay
            loop
            muted
            playsinline
          />
        {:else}
          <img
            src={selectedItem.image}
            alt={selectedItem.title || ''}
            class="w-full h-full object-cover"
          />
        {/if}

        {#if selectedItem.title || selectedItem.description}
          <div
            class="absolute inset-x-0 bottom-0 bg-gradient-to-t from-black/80 to-transparent p-6 pb-24 text-white"
          >
            {#if selectedItem.title}
              <h3 class="text-2xl font-bold mb-2 whitespace-pre-line">
                {@html selectedItem.title}
              </h3>
            {/if}
            {#if selectedItem.description}
              <p class="text-lg">{selectedItem.description}</p>
            {/if}
          </div>
        {/if}

        {#if selectedItem.audio}
          <div class="absolute inset-x-0 bottom-0 p-6">
            <audio
              bind:this={audio}
              src={selectedItem.audio}
              on:timeupdate={handleTimeUpdate}
              on:loadedmetadata={() => (duration = audio.duration)}
              class="hidden"
            />
            <div
              class="relative h-16 bg-white rounded-lg overflow-hidden cursor-pointer group"
              on:click={handleClick}
            >
              <!-- Base progress bar that grows -->
              <div
                class="absolute inset-y-0 left-0 bg-green-600 transition-all duration-100"
                style="width: {progress}%"
              />

              <!-- Fixed-width play button container -->
              <div
                class="absolute inset-y-0 left-0 w-16 sm:w-20 bg-green-600 flex items-center justify-center"
              >
                <button
                  class="text-white hover:text-green-100 transition-colors z-10"
                  on:click|stopPropagation={handlePlayPause}
                >
                  {#if isPlaying}
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      class="h-6 w-6 sm:h-8 sm:w-8"
                      fill="none"
                      viewBox="0 0 24 24"
                      stroke="currentColor"
                    >
                      <path
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        stroke-width="2"
                        d="M10 9v6m4-6v6"
                      />
                    </svg>
                  {:else}
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      class="h-6 w-6 sm:h-8 sm:w-8"
                      fill="none"
                      viewBox="0 0 24 24"
                      stroke="currentColor"
                    >
                      <path
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        stroke-width="2"
                        d="M14.752 11.168l-3.197-2.132A1 1 0 0010 9.87v4.263a1 1 0 001.555.832l3.197-2.132a1 1 0 000-1.664z"
                      />
                    </svg>
                  {/if}
                </button>
              </div>

              <!-- Title with dynamic color based on progress -->
              <div
                class="absolute inset-0 flex items-center justify-center pl-16 sm:pl-20"
              >
                <span
                  class="font-medium relative z-10 text-xs sm:text-base truncate pr-16"
                  style="color: {progress > 50
                    ? 'white'
                    : '#16a34a'}; transition: color 0.3s ease;"
                >
                  Untitled (Cracked Watermelon)
                </span>
              </div>

              <!-- Time with dynamic color -->
              <div
                class="absolute right-3 sm:right-4 top-1/2 -translate-y-1/2 text-[10px] sm:text-sm whitespace-nowrap transition-colors duration-100"
                style="color: {progress > 85 ? 'white' : '#16a34a'};"
              >
                {formatTime(audio?.currentTime || 0)} / {formatTime(duration)}
              </div>
            </div>
          </div>
        {/if}
      </div>
    </div>
  </div>
{/if}
