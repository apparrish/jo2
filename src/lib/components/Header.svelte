<script lang="ts">
  import {
    Navbar,
    NavBrand,
    NavLi,
    NavUl,
    NavHamburger
  } from 'flowbite-svelte';

  let isScrolled = false;
  let isMenuOpen = false;

  function handleScroll() {
    isScrolled = window.scrollY > 50;
  }

  function toggleMenu() {
    isMenuOpen = !isMenuOpen;
  }
</script>

<svelte:window on:scroll={handleScroll} />

<Navbar
  class={`fixed w-full z-50 transition-all duration-300 ${
    isScrolled ? 'bg-black/30 backdrop-blur-sm' : 'bg-transparent'
  }`}
>
  <div class="w-full flex justify-end">
    <button
      class="md:hidden p-2"
      on:click={toggleMenu}
      aria-label="Toggle menu"
    >
      <svg
        class="w-6 h-6 text-white"
        fill="none"
        stroke="currentColor"
        viewBox="0 0 24 24"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          stroke-width="2"
          d="M4 6h16M4 12h16M4 18h16"
        />
      </svg>
    </button>

    <div
      class={`${isMenuOpen ? 'block' : 'hidden'} md:block absolute md:relative top-full right-0 w-full md:w-auto bg-black/90 md:bg-transparent`}
    >
      <NavUl class="flex flex-col md:flex-row items-center md:items-end">
        <NavLi href="/about">
          <span
            class="text-white text-xs uppercase tracking-wider font-extrabold
                   hover:text-gray-300 transition-colors duration-200
                   px-5 py-4 md:py-5 inline-block w-full text-center md:text-left"
          >
            About
          </span>
        </NavLi>
        <NavLi href="/work">
          <span
            class="text-white text-xs uppercase tracking-wider font-extrabold
                   hover:text-gray-300 transition-colors duration-200
                   px-5 py-4 md:py-5 inline-block w-full text-center md:text-left"
          >
            Work
          </span>
        </NavLi>
        <NavLi href="/creative">
          <span
            class="text-white text-xs uppercase tracking-wider font-extrabold
                   hover:text-gray-300 transition-colors duration-200
                   px-5 py-4 md:py-5 inline-block w-full text-center md:text-left"
          >
            Creative
          </span>
        </NavLi>
        <NavLi href="/contact">
          <span
            class="text-white text-xs uppercase tracking-wider font-extrabold
                   hover:text-gray-300 transition-colors duration-200
                   px-5 py-4 md:py-5 inline-block w-full text-center md:text-left"
          >
            Contact
          </span>
        </NavLi>
      </NavUl>
    </div>
  </div>
</Navbar>
