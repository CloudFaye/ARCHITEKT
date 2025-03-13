<script>
  import { onMount } from 'svelte';
  import { animate, inView, scroll } from 'motion';
  import Navigation from '../src/components/Navigation.svelte'
  import Hero from '../src/components/Hero.svelte';
  import Projects from '../src/components/Projects.svelte';
  import DesignLanguage from '../src/components/DesignLanguage.svelte';
  import Contact from '../src/components/Contact.svelte';
  import Footer from '../src/components/Footer.svelte';
  
  // Theme state
  let isDarkMode = $state(false);
  let isMenuOpen = $state(false);
  
  // Current section for navigation
  let currentSection = $state('home');
  
  // Toggle dark mode
  function toggleDarkMode() {
    isDarkMode = !isDarkMode;
    document.documentElement.classList.toggle('dark-mode');
  }
  
  // Toggle menu for mobile
  function toggleMenu() {
    isMenuOpen = !isMenuOpen;
  }
  
  // Scroll to section with Motion
  function scrollToSection(id) {
    const section = document.getElementById(id);
    if (section) {
      window.scrollTo({
        top: section.offsetTop,
        left: 0,
        behavior: 'smooth'
      });
    }
  }
  
  // Set up Motion scroll animations
  function setupScrollAnimations() {
    // Set up scroll-triggered animations
    document.querySelectorAll('.reveal-on-scroll').forEach(element => {
      inView(element, () => {
        animate(element, {
          opacity: [0, 1],
          y: [30, 0],
          scale: [0.97, 1]
        }, {
          duration: 0.8,
          easing: [0.22, 0.03, 0.26, 1],
          delay: 0.1
        });
        
        return () => {
          // Reset animation when element is out of view
          animate(element, { 
            opacity: [1, 0], 
            y: [0, 30], 
            scale: [1, 0.97] 
          }, { duration: 0 });
        };
      }, { margin: "-10% 0px -10% 0px" });
    });
    
    // Set up fade-in elements
    document.querySelectorAll('.fade-in').forEach(element => {
      inView(element, () => {
        animate(element, { opacity: [0, 1] }, { duration: 0.8 });
        
        return () => {
          animate(element, { opacity: [1, 0] }, { duration: 0 });
        };
      });
    });
    
    // Set up slide-up elements
    document.querySelectorAll('.slide-up').forEach(element => {
      inView(element, () => {
        animate(element, { 
          y: [30, 0], 
          opacity: [0, 1] 
        }, { duration: 0.6 });
        
        return () => {
          animate(element, { 
            y: [0, 30], 
            opacity: [1, 0] 
          }, { duration: 0 });
        };
      });
    });
    
    // Set up slide-in-left elements
    document.querySelectorAll('.slide-in-left').forEach(element => {
      inView(element, () => {
        animate(element, { 
          x: [-30, 0], 
          opacity: [0, 1] 
        }, { duration: 0.6 });
        
        return () => {
          animate(element, { 
            x: [0, -30], 
            opacity: [1, 0] 
          }, { duration: 0 });
        };
      });
    });
    
    // Set up image hover animations (grayscale to color)
    document.querySelectorAll('.image-hover-effect').forEach(element => {
      element.addEventListener('mouseenter', () => {
        animate(element, { 
          filter: ['grayscale(100%)', 'grayscale(0%)'] 
        }, { duration: 0.4, easing: 'ease-out' });
      });
      
      element.addEventListener('mouseleave', () => {
        animate(element, { 
          filter: ['grayscale(0%)', 'grayscale(100%)'] 
        }, { duration: 0.4, easing: 'ease-out' });
      });
    });
    
    // Set up button hover animations
    document.querySelectorAll('.button-hover-effect').forEach(element => {
      element.addEventListener('mouseenter', () => {
        animate(element, { 
          scale: [1, 1.05],
          backgroundColor: element.dataset.hoverColor || null
        }, { duration: 0.2 });
      });
      
      element.addEventListener('mouseleave', () => {
        animate(element, { 
          scale: [1.05, 1],
          backgroundColor: element.dataset.normalColor || null
        }, { duration: 0.2 });
      });
    });
    
    // Parallax effect on scroll (fixed to work with motion API)
    document.querySelectorAll('.parallax-element').forEach(element => {
      // Get the parallax rate from data attribute or use default
      const rate = parseFloat(element.getAttribute('data-parallax-rate') || '0.1');
      
      // Set up scroll watcher
      document.addEventListener('scroll', () => {
        const scrollY = window.scrollY;
        animate(element, { 
          translateY: `${-scrollY * rate}px` 
        }, { duration: 0 });
      });
    });
  }
  
  onMount(() => {
    // Check user preference for dark mode
    const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
    if (prefersDark) {
      isDarkMode = true;
      document.documentElement.classList.add('dark-mode');
    }
    
    // Set up scroll observer for section highlighting
    const sections = document.querySelectorAll('section[id]');
    const observer = new IntersectionObserver(entries => {
      entries.forEach(entry => {
        if (entry.isIntersecting) {
          currentSection = entry.target.id;
        }
      });
    }, { threshold: 0.3 });
    
    sections.forEach(section => observer.observe(section));
    
    // Initialize Motion animations
    setupScrollAnimations();
    
    // Clean up observer on component unmount
    return () => {
      observer.disconnect();
      
      // Clean up event listeners
      document.querySelectorAll('.image-hover-effect, .button-hover-effect').forEach(element => {
        element.removeEventListener('mouseenter', null);
        element.removeEventListener('mouseleave', null);
      });
      
      document.querySelectorAll('.parallax-element').forEach(() => {
        document.removeEventListener('scroll', null);
      });
    };
  });
</script>

<div class="app" class:dark-mode={isDarkMode}>
  <Navigation 
    {currentSection} 
    {isDarkMode} 
    {isMenuOpen}
    onToggleDarkMode={toggleDarkMode}
    onToggleMenu={toggleMenu}
    onNavigate={scrollToSection}
  />
  
  <main>
    <section id="home" class="reveal-on-scroll">
      <Hero />
    </section>
    
    <section id="projects" class="reveal-on-scroll">
      <Projects />
    </section>
    
    <section id="design" class="reveal-on-scroll">
      <DesignLanguage />
    </section>
    
    <section id="contact" class="reveal-on-scroll">
      <Contact />
    </section>
  </main>
  
  <Footer {isDarkMode} />
</div>

<style>
  :global(:root) {
    --color-bg: #ffffff;
    --color-text: #000000;
    --color-accent: #ff3e00;
    --color-secondary: #676778;
    --color-muted: #f6f6f6;
    --color-border: #e2e2e2;
    --font-sans: 'Helvetica Neue', Helvetica, Arial, sans-serif;
    --grid-unit: 8px;
    --transition-speed: 0.3s;
    --animation-speed: 0.8s;
  }
  
  :global(.dark-mode) {
    --color-bg: #121212;
    --color-text: #ffffff;
    --color-accent: #ff6b3d;
    --color-secondary: #a1a1a1;
    --color-muted: #1e1e1e;
    --color-border: #2c2c2c;
  }
  
  :global(body) {
    font-family: var(--font-sans);
    margin: 0;
    padding: 0;
    background-color: var(--color-bg);
    color: var(--color-text);
    transition: background-color var(--transition-speed), color var(--transition-speed);
    overflow-x: hidden;
    overflow-y: auto;
    overscroll-behavior: none;
  }
  
  :global(html) {
    scroll-behavior: smooth;
  }
  
  :global(h1, h2, h3, h4, h5, h6) {
    font-weight: 400;
    line-height: 1.2;
  }
  
  .app {
    display: flex;
    flex-direction: column;
    min-height: 100vh;
  }
  
  main {
    flex: 1;
    max-width: 1440px;
    margin: 0 auto;
    width: 100%;
    padding: 0 calc(var(--grid-unit) * 2);
  }
  
  section {
    padding: calc(var(--grid-unit) * 12) 0;
    scroll-margin-top: calc(var(--grid-unit) * 10);
  }
  
  /* Initial Motion animation states */
  :global(.reveal-on-scroll) {
    opacity: 0;
    transform: translateY(30px) scale(0.97);
    will-change: transform, opacity;
  }
  
  :global(.fade-in) {
    opacity: 0;
    will-change: opacity;
  }
  
  :global(.slide-up) {
    opacity: 0;
    transform: translateY(30px);
    will-change: transform, opacity;
  }
  
  :global(.slide-in-left) {
    opacity: 0;
    transform: translateX(-30px);
    will-change: transform, opacity;
  }
  
  :global(.parallax-element) {
    will-change: transform;
  }
  
  /* Image hover effect - default grayscale */
  :global(.image-hover-effect) {
    filter: grayscale(100%);
    transition: filter var(--transition-speed);
    will-change: filter;
  }
  
  /* Button hover effect */
  :global(.button-hover-effect) {
    transition: transform var(--transition-speed), background-color var(--transition-speed);
    will-change: transform, background-color;
  }
  
  @media (max-width: 768px) {
    section {
      padding: calc(var(--grid-unit) * 8) 0;
    }
  }
  
  /* Respect user preferences for reduced motion */
  @media (prefers-reduced-motion: reduce) {
    :global(.reveal-on-scroll),
    :global(.fade-in),
    :global(.slide-up),
    :global(.slide-in-left),
    :global(.image-hover-effect),
    :global(.button-hover-effect),
    :global(.parallax-element) {
      transition: none !important;
      transform: none !important;
      filter: none !important;
      opacity: 1 !important;
    }
  }
</style>
