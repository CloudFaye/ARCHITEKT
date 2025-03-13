<script>
  import { onMount } from 'svelte';
  import { animate, inView } from 'motion';
  import Navigation from '../src/components/Navigation.svelte'
  import Hero from '../src/components/Hero.svelte';
  import Projects from '../src/components/Projects.svelte';
  import DesignLanguage from '../src/components/DesignLanguage.svelte';
  import Contact from '../src/components/Contact.svelte';
  import Footer from '../src/components/Footer.svelte';
  import ImageWithEffect from '../src/components/ImageWithEffect.svelte';
  import ButtonWithEffect from '../src/components/ButtonWithEffect.svelte';
  
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
  
  // Scroll to section with native smooth scroll
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
    // Generic function to handle element animation with inView
    function setupElementAnimation(selector, animationProps, options = {}) {
      document.querySelectorAll(selector).forEach(element => {
        inView(element, () => {
          try {
            // @ts-ignore - Motion types are not fully compatible with TS
            animate(element, animationProps, {
              duration: 0.8,
              ...options
            });
          } catch (err) {
            console.error('Animation error:', err);
          }
          
          return () => {
            // Optional: Reset animations when scrolling out of view
            // We're not resetting to avoid jarring transitions
          };
        }, { margin: "-10% 0px -10% 0px" });
      });
    }
    
    // Set up different animation types
    setupElementAnimation('.reveal-on-scroll', { 
      opacity: [0, 1],
      y: [30, 0],
      scale: [0.97, 1]
    }, { 
      duration: 0.8,
      delay: 0.1
    });
    
    setupElementAnimation('.fade-in', { 
      opacity: [0, 1] 
    });
    
    setupElementAnimation('.slide-up', { 
      y: [30, 0], 
      opacity: [0, 1] 
    }, { 
      duration: 0.6 
    });
    
    setupElementAnimation('.slide-in-left', { 
      x: [-30, 0], 
      opacity: [0, 1] 
    }, { 
      duration: 0.6 
    });
    
    setupElementAnimation('.slide-in-right', { 
      x: [30, 0], 
      opacity: [0, 1] 
    }, { 
      duration: 0.6 
    });
    
    setupElementAnimation('.zoom-in', { 
      scale: [0.9, 1], 
      opacity: [0, 1] 
    }, { 
      duration: 0.7 
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
    
    <section id="projects" class="full-width">
      <Projects />
    </section>
    
    <section id="design">
      <h2 class="slide-up">Design Language</h2>
      <DesignLanguage />
    </section>
    
    <section id="contact">
      <h2 class="slide-up">Contact</h2>
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
  
  section.full-width {
    max-width: 100%;
    width: 100%;
    padding-left: 0;
    padding-right: 0;
  }
  
  h2 {
    font-size: 2.5rem;
    margin-bottom: 2rem;
    color: var(--color-accent);
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
  
  :global(.slide-in-right) {
    opacity: 0;
    transform: translateX(30px);
    will-change: transform, opacity;
  }
  
  :global(.zoom-in) {
    opacity: 0;
    transform: scale(0.9);
    will-change: transform, opacity;
  }
  
  @media (max-width: 768px) {
    section {
      padding: calc(var(--grid-unit) * 8) 0;
    }
    
    h2 {
      font-size: 2rem;
    }
  }
  
  /* Respect user preferences for reduced motion */
  @media (prefers-reduced-motion: reduce) {
    :global(.reveal-on-scroll),
    :global(.fade-in),
    :global(.slide-up),
    :global(.slide-in-left),
    :global(.slide-in-right),
    :global(.zoom-in) {
      transition: none !important;
      transform: none !important;
      opacity: 1 !important;
    }
  }
</style>
