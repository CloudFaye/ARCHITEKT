<script>
  /**
   * Image component with hover effects 
   * Shows grayscale by default and transitions to full color on hover
   */
  import { onMount, onDestroy } from 'svelte';
  
  // Props using standard Svelte syntax
  export let src = '';
  export let alt = '';
  export let width = 'auto';
  export let height = 'auto';
  export let className = '';
  export let parallax = false;
  export let parallaxRate = 0.1;
  
  // State for hover
  let hovered = false;
  let elementRef;
  
  // Handle mouse events
  function handleMouseEnter() {
    hovered = true;
  }
  
  function handleMouseLeave() {
    hovered = false;
  }
  
  // Set up parallax scroll effect if enabled
  let cleanup = null;
  
  onMount(() => {
    if (parallax && elementRef) {
      const handleScroll = () => {
        const scrollY = window.scrollY;
        const rect = elementRef.getBoundingClientRect();
        const centerY = rect.top + rect.height / 2 - window.innerHeight / 2;
        
        // Apply parallax transform
        elementRef.style.transform = `translateY(${-centerY * parallaxRate}px)`;
      };
      
      // Initial positioning
      handleScroll();
      
      // Add scroll listener
      window.addEventListener('scroll', handleScroll, { passive: true });
      
      // Cleanup function
      cleanup = () => {
        window.removeEventListener('scroll', handleScroll);
      };
    }
  });
  
  onDestroy(() => {
    if (cleanup) cleanup();
  });
</script>

<!-- svelte-ignore a11y_no_static_element_interactions -->
<div 
  class="image-container {className}"
  class:parallax-enabled={parallax}
  on:mouseenter={handleMouseEnter}
  on:mouseleave={handleMouseLeave}
  bind:this={elementRef}
>
  <img 
    {src}
    {alt}
    style="width: {width}; height: {height};"
    class="image"
    class:is-hovered={hovered}
  />
</div>

<style>
  .image-container {
    position: relative;
    overflow: hidden;
    display: inline-block;
    will-change: transform;
  }
  
  .parallax-enabled {
    will-change: transform;
  }
  
  .image {
    display: block;
    object-fit: cover;
    transition: filter var(--transition-speed, 0.3s) ease-out;
    filter: grayscale(100%);
    will-change: filter;
  }
  
  .image.is-hovered {
    filter: grayscale(0%);
  }
  
  /* Respect user preferences for reduced motion */
  @media (prefers-reduced-motion: reduce) {
    .image {
      transition: none !important;
      filter: none !important;
    }
    
    .parallax-enabled {
      transform: none !important;
    }
  }
</style> 