<script lang="ts">
  import { Splide, SplideSlide } from '@splidejs/svelte-splide';
  import '@splidejs/svelte-splide/css';

  type GalleryItem = {
    image: string;
    title: string;
    description: string;
    link?: string;
    backgroundColor?: string;
  };

  export let items: GalleryItem[] = [];

  const splideOptions = {
    type: 'loop',
    padding: '20%',
    gap: '1rem',
    height: '400px',
    // focus: 'center',
    arrows: true,
    pagination: false,
    speed: 600,
    easing: 'cubic-bezier(0.4, 0, 0.2, 1)',
    autoplay: true,
    interval: 5000,
    resetProgress: false,
    clickable: 'free',
    preventClickOnDrag: true,
    drag: true,
    snap: true
  };
</script>

<div class="gallery-container">
  <Splide options={splideOptions} aria-label="Gallery">
    {#each items as item}
      <SplideSlide>
        <div
          class="slide-content"
          style="background-color: {item.backgroundColor || 'transparent'}"
        >
          {#if item.link}
            <a href={item.link} target="_blank" rel="noopener noreferrer">
              <img src={item.image} alt={item.title} />
              <div class="info-overlay">
                <h3>{@html item.title}</h3>
                <p>{item.description}</p>
                <span class="text-primary-400 text-sm mt-4">View →</span>
              </div>
              <div class="preview-overlay" />
            </a>
          {:else}
            <div class="slide-wrapper">
              <img src={item.image} alt={item.title} />
              <div class="info-overlay">
                <h3>{@html item.title}</h3>
                <p>{item.description}</p>
                {#if item.link}
                  <span class="text-primary-400 text-sm mt-4">View →</span>
                {/if}
              </div>
              <div class="preview-overlay" />
            </div>
          {/if}
        </div>
      </SplideSlide>
    {/each}
  </Splide>
</div>

<style>
  .gallery-container {
    width: 100%;
    padding: 1rem;
  }

  .slide-content,
  .slide-wrapper {
    position: relative;
    width: 100%;
    height: 100%;
    aspect-ratio: 16/9;
  }

  img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }

  .info-overlay {
    position: absolute;
    inset: 0;
    background: rgba(0, 0, 0, 0);
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    text-align: center;
    color: white;
    padding: 2rem;
    opacity: 0;
    transition: all 0.3s ease-in-out;
    z-index: 2;
  }

  .preview-overlay {
    position: absolute;
    inset: 0;
    background: rgba(71, 85, 105, 0.7);
    opacity: 1;
    transition: opacity 0.3s ease-in-out;
    z-index: 1;
  }

  :global(.splide__slide.is-active) .preview-overlay {
    opacity: 0;
  }

  :global(.splide__slide.is-active) .slide-wrapper:hover .info-overlay,
  :global(.splide__slide.is-active) a:hover .info-overlay {
    background: rgba(0, 0, 0, 0.7);
    opacity: 1;
  }

  h3 {
    font-size: 1.5rem;
    font-weight: bold;
    margin-bottom: 1rem;
    transform: translateY(20px);
    transition: transform 0.3s ease-out;
  }

  p {
    font-size: 1.1rem;
    line-height: 1.5;
    max-width: 80%;
    transform: translateY(20px);
    transition: transform 0.3s ease-out;
  }

  span {
    transform: translateY(20px);
    transition: transform 0.3s ease-out;
  }

  :global(.splide__slide.is-active) .slide-wrapper:hover h3,
  :global(.splide__slide.is-active) .slide-wrapper:hover p,
  :global(.splide__slide.is-active) .slide-wrapper:hover span,
  :global(.splide__slide.is-active) a:hover h3,
  :global(.splide__slide.is-active) a:hover p,
  :global(.splide__slide.is-active) a:hover span {
    transform: translateY(0);
  }
</style>
