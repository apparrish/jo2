<script lang="ts">
  import { onMount } from 'svelte';
  import gsap from 'gsap';

  type GalleryItem = {
    image: string;
    title: string;
    description: string;
    link?: string;
  };

  export let items: GalleryItem[] = [];
  let selectedItem: GalleryItem | null = null;
  let clickedImageElement: HTMLElement | null = null;
  let modalImage: HTMLElement | null = null;

  const createRows = (items: GalleryItem[]) => {
    const row1 = items.filter((_, i) => i % 3 === 0);
    const row2 = items.filter((_, i) => i % 3 === 1);
    const row3 = items.filter((_, i) => i % 3 === 2);

    return [
      Array(12).fill(row1).flat(),
      Array(12).fill(row2).flat(),
      Array(12).fill(row3).flat()
    ];
  };

  const rows = createRows(items);

  function openModal(item: GalleryItem, event: MouseEvent) {
    selectedItem = item;
    clickedImageElement = (event.currentTarget as HTMLElement).querySelector(
      'img'
    );
    document.body.style.overflow = 'hidden';

    setTimeout(() => {
      modalImage = document.querySelector('.modal-content');
      if (!clickedImageElement || !modalImage) return;

      const rect = clickedImageElement.getBoundingClientRect();
      const viewportHeight = window.innerHeight;
      const viewportWidth = window.innerWidth;

      const targetWidth = Math.min(viewportWidth * 0.9, 1200);
      const targetHeight = Math.min(
        viewportHeight * 0.8,
        targetWidth * (9 / 16)
      );

      gsap.set(modalImage, {
        position: 'fixed',
        width: rect.width,
        height: rect.height,
        top: rect.top,
        left: rect.left,
        opacity: 1
      });

      gsap.to(modalImage, {
        width: targetWidth,
        height: targetHeight,
        top: (viewportHeight - targetHeight) / 2,
        left: (viewportWidth - targetWidth) / 2,
        duration: 0.4,
        ease: 'power2.inOut'
      });

      gsap.to('.modal-overlay', { opacity: 1, duration: 0.3 });
      gsap.to('.modal-text', { opacity: 1, y: 0, duration: 0.3, delay: 0.2 });
    }, 0);
  }

  function closeModal() {
    if (!clickedImageElement || !modalImage) return;
    const rect = clickedImageElement.getBoundingClientRect();

    gsap.to(modalImage, {
      width: rect.width,
      height: rect.height,
      top: rect.top,
      left: rect.left,
      duration: 0.4,
      ease: 'power2.inOut'
    });

    gsap.to('.modal-overlay', { opacity: 0, duration: 0.3 });
    gsap.to('.modal-text', { opacity: 0, y: 20, duration: 0.2 });

    setTimeout(() => {
      selectedItem = null;
      clickedImageElement = null;
      modalImage = null;
      document.body.style.overflow = 'auto';
    }, 400);
  }

  onMount(() => {
    const tracks = document.querySelectorAll('.gallery-track');

    tracks.forEach((track, index) => {
      const direction = index === 1 ? 1 : -1;
      const duration = 40 + index * 5;

      gsap.set(track, {
        x: direction === 1 ? '0%' : '-50%'
      });

      gsap.to(track, {
        x: direction === 1 ? '-50%' : '0%',
        duration,
        ease: 'none',
        repeat: -1,
        yoyo: true
      });
    });
  });
</script>

<div class="gallery-wrapper relative w-full h-[85vh] overflow-hidden py-12">
  {#each rows as row, rowIndex}
    <div
      class="gallery-track absolute flex gap-8 items-center"
      style="top: {15 + rowIndex * 35}%;"
    >
      {#each row as item}
        <div
          class="gallery-item relative cursor-pointer transition-all duration-300 hover:scale-105 hover:z-10"
          class:pulsate={item.link}
          on:click={(e) => openModal(item, e)}
          on:keydown={(e) => e.key === 'Enter' && openModal(item, e)}
          role="button"
          tabindex="0"
        >
          <div class="relative">
            <img
              src={item.image}
              alt={item.title}
              class="rounded-lg object-cover shadow-lg"
              style="
                width: {item.link ? '480px' : '320px'}; 
                height: {item.link ? '270px' : '180px'};
              "
            />
            {#if item.title}
              <div
                class="info-overlay absolute inset-0 bg-black/60 rounded-lg opacity-0 transition-opacity duration-300 flex items-center justify-center"
              >
                <h3 class="text-white text-xl font-medium text-center px-4">
                  {item.title}
                </h3>
              </div>
            {/if}
          </div>
        </div>
      {/each}
    </div>
  {/each}
</div>

{#if selectedItem}
  <div
    class="fixed inset-0 z-50 flex items-center justify-center"
    on:click={closeModal}
    on:keydown={(e) => e.key === 'Escape' && closeModal()}
  >
    <div class="modal-overlay fixed inset-0 bg-black/90 opacity-0" />

    <div
      class="modal-content fixed bg-white rounded-lg overflow-hidden shadow-2xl"
      on:click|stopPropagation={() => {}}
    >
      <img
        src={selectedItem.image}
        alt={selectedItem.title}
        class="w-full h-full object-cover"
      />
      <div
        class="modal-text absolute bottom-0 left-0 right-0 bg-gradient-to-t from-black/90 to-transparent p-8 opacity-0 translate-y-4"
      >
        <h2 class="text-3xl font-bold text-white mb-3">{selectedItem.title}</h2>
        <p class="text-lg text-gray-200 mb-4 max-w-2xl">
          {selectedItem.description}
        </p>
        {#if selectedItem.link}
          <a
            href={selectedItem.link}
            target="_blank"
            rel="noopener noreferrer"
            class="inline-flex items-center text-primary-400 hover:text-primary-300 text-lg"
          >
            View Project <span class="ml-2">→</span>
          </a>
        {/if}
      </div>
      <button
        class="absolute top-6 right-6 text-white/80 hover:text-white bg-black/50 hover:bg-black/75 w-10 h-10 rounded-full flex items-center justify-center transition-colors duration-200"
        on:click={closeModal}
      >
        ×
      </button>
    </div>
  </div>
{/if}

<style>
  .gallery-wrapper {
    perspective: 1000px;
  }

  .gallery-track {
    will-change: transform;
    transform: translateY(-50%);
  }

  .gallery-item {
    flex-shrink: 0;
  }

  .gallery-item:hover .info-overlay {
    opacity: 1;
  }

  @keyframes pulsate {
    0% {
      transform: scale(1);
      box-shadow: 0 0 0 0 rgba(255, 255, 255, 0.4);
    }

    70% {
      transform: scale(1.02);
      box-shadow: 0 0 0 10px rgba(255, 255, 255, 0);
    }

    100% {
      transform: scale(1);
      box-shadow: 0 0 0 0 rgba(255, 255, 255, 0);
    }
  }

  @keyframes highlight {
    0% {
      box-shadow:
        0 0 20px 3px rgba(255, 255, 255, 0.2),
        0 0 30px 6px rgba(255, 255, 255, 0.1);
    }

    50% {
      box-shadow:
        0 0 25px 5px rgba(255, 255, 255, 0.3),
        0 0 35px 8px rgba(255, 255, 255, 0.2);
    }

    100% {
      box-shadow:
        0 0 20px 3px rgba(255, 255, 255, 0.2),
        0 0 30px 6px rgba(255, 255, 255, 0.1);
    }
  }

  .pulsate {
    animation:
      pulsate 3s ease-in-out infinite,
      highlight 4s ease-in-out infinite;
  }

  .pulsate:hover {
    animation-play-state: paused;
  }
</style>
