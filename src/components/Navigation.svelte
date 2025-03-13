<script>
  import { animate } from 'motion';
  import ButtonWithEffect from '../components/ButtonWithEffect.svelte';
  
  // Props
  export let currentSection = '';
  export let isDarkMode = false;
  export let isMenuOpen = false;
  export let onToggleDarkMode = () => {};
  export let onToggleMenu = () => {};
  export let onNavigate = (id) => {};
  
  // Navigation items
  const navLinks = [
    { id: 'home', label: 'HOME' },
    { id: 'projects', label: 'PROJECTS' },
    { id: 'design', label: 'DESIGN LANGUAGE' },
    { id: 'contact', label: 'CONTACT' }
  ];
  
  // Handle navigation click
  function handleNavClick(id) {
    onNavigate(id);
    
    // Close mobile menu if open
    if (isMenuOpen) {
      onToggleMenu();
    }
  }
  
  // Animate menu toggle
  function animateMenuToggle(isOpen) {
    const menuElement = document.querySelector('.nav-links-wrapper');
    if (menuElement) {
      try {
        // Using setTimeout to avoid animation errors
        setTimeout(() => {
          if (isOpen) {
            // Use setAttribute to avoid TypeScript errors
            menuElement.setAttribute('style', 'opacity: 1; transform: translateY(0px)');
          } else {
            menuElement.setAttribute('style', 'opacity: 0; transform: translateY(-10px)');
          }
        }, 10);
        
        // Animate each navigation link with staggered delay
        const navLinks = document.querySelectorAll('.nav-link');
        navLinks.forEach((link, index) => {
          setTimeout(() => {
            if (isOpen) {
              link.setAttribute('style', 'opacity: 1; transform: translateY(0px)');
            } else {
              link.setAttribute('style', 'opacity: 0; transform: translateY(10px)');
            }
          }, isOpen ? 100 + (index * 50) : 10);
        });
      } catch (error) {
        console.error('Animation error:', error);
      }
    }
  }
  
  // Watch for isMenuOpen changes
  $: {
    if (typeof document !== 'undefined') {
      animateMenuToggle(isMenuOpen);
    }
  }
</script>

<nav class:menu-open={isMenuOpen}>
  <div class="nav-container">
    <div class="logo-container">
      <a href="/" class="logo">
        <div class="logo-grid">
          <div class="logo-dot"></div>
          <div class="logo-dot"></div>
          <div class="logo-dot"></div>
          <div class="logo-dot"></div>
        </div>
        <span>ARCHITEKT</span>
      </a>
    </div>
    
    <div class="nav-controls">
      <button class="hamburger-button" on:click={onToggleMenu} aria-label="Toggle menu">
        <div class="hamburger">
          <span></span>
          <span></span>
          <span></span>
        </div>
      </button>
    </div>
  </div>
  
  <div class="nav-links-wrapper">
    <div class="nav-header">
      <button class="theme-toggle" on:click={onToggleDarkMode} aria-label="Toggle dark mode">
        {#if isDarkMode}
          <div class="theme-icon">
            <svg viewBox="0 0 24 24" width="24" height="24"><path fill="currentColor" d="M12 10.999c1.437.438 2.562 1.564 2.999 3.001.44-1.437 1.565-2.562 3.001-3-1.436-.439-2.561-1.563-3.001-3-.437 1.436-1.562 2.561-2.999 2.999zm8.001.001c.958.293 1.707 1.042 2 2.001.291-.959 1.042-1.709 1.999-2.001-.957-.292-1.707-1.042-2-2-.293.958-1.042 1.708-1.999 2zm-1-9c-.437 1.437-1.563 2.562-2.998 3.001 1.438.44 2.561 1.564 3.001 3.002.437-1.438 1.563-2.563 2.996-3.002-1.433-.437-2.559-1.564-2.999-3.001zm-7.001 22c-6.617 0-12-5.383-12-12s5.383-12 12-12c1.894 0 3.63.497 5.37 1.179-2.948.504-9.37 3.266-9.37 10.821 0 7.454 5.917 10.208 9.37 10.821-1.5.846-3.476 1.179-5.37 1.179z"/></svg>
            <span>Dark Mode</span>
          </div>
        {:else}
          <div class="theme-icon">
            <svg viewBox="0 0 24 24" width="24" height="24"><path fill="currentColor" d="M4.069 13h-4.069v-2h4.069c-.041.328-.069.661-.069 1s.028.672.069 1zm3.034-7.312l-2.881-2.881-1.414 1.414 2.881 2.881c.411-.529.885-1.003 1.414-1.414zm11.209 1.414l2.881-2.881-1.414-1.414-2.881 2.881c.528.411 1.002.886 1.414 1.414zm-6.312-3.102c.339 0 .672.028 1 .069v-4.069h-2v4.069c.328-.041.661-.069 1-.069zm0 16c-.339 0-.672-.028-1-.069v4.069h2v-4.069c-.328.041-.661.069-1 .069zm7.931-9c.041.328.069.661.069 1s-.028.672-.069 1h4.069v-2h-4.069zm-3.033 7.312l2.88 2.88 1.415-1.414-2.88-2.88c-.412.528-.886 1.002-1.415 1.414zm-11.21-1.415l-2.88 2.88 1.414 1.414 2.88-2.88c-.528-.411-1.003-.885-1.414-1.414zm6.312-10.897c-3.314 0-6 2.686-6 6s2.686 6 6 6 6-2.686 6-6-2.686-6-6-6z"/></svg>
            <span>Light Mode</span>
          </div>
        {/if}
      </button>
      
      
    </div>
    
    <ul class="nav-links">
      {#each navLinks as link}
        <li class:active={currentSection === link.id}>
          <button 
            on:click={() => handleNavClick(link.id)}
            class="nav-link"
          >
            <span class="nav-link-text">{link.label}</span>
            {#if currentSection === link.id}
              <span class="nav-link-indicator"></span>
            {/if}
          </button>
        </li>
      {/each}
    </ul>
    
    <div class="nav-footer">
      <ButtonWithEffect 
        text="CONTACT US" 
        primary={true} 
        hoverColor="var(--color-accent)" 
        className="contact-button" 
        fullWidth={true}
        on:click={() => handleNavClick('contact')}
      />
      
      <div class="social-links">
        <a href="https://instagram.com" target="_blank" rel="noopener noreferrer" class="social-link">Instagram</a>
        <a href="https://linkedin.com" target="_blank" rel="noopener noreferrer" class="social-link">LinkedIn</a>
        <a href="https://dribbble.com" target="_blank" rel="noopener noreferrer" class="social-link">Dribbble</a>
      </div>
    </div>
  </div>
</nav>

<style>
  nav {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    z-index: 1000;
    background-color: var(--color-bg);
    border-bottom: 1px solid var(--color-border);
    transition: all var(--transition-speed);
  }
  
  .nav-container {
    display: flex;
    justify-content: space-between;
    align-items: center;
    max-width: 1440px;
    margin: 0 auto;
    padding: calc(var(--grid-unit) * 2) calc(var(--grid-unit) * 3);
    position: relative;
  }
  
  .logo-container {
    display: flex;
    align-items: center;
  }
  
  .logo {
    display: flex;
    align-items: center;
    gap: calc(var(--grid-unit) * 1.5);
    text-decoration: none;
    color: var(--color-text);
    font-weight: 500;
    letter-spacing: 2px;
    font-size: 1.125rem;
  }
  
  .logo-grid {
    display: grid;
    grid-template-columns: repeat(2, 6px);
    grid-template-rows: repeat(2, 6px);
    gap: 2px;
  }
  
  .logo-dot {
    width: 6px;
    height: 6px;
    background-color: var(--color-accent);
  }
  
  .logo-dot:nth-child(2) {
    background-color: var(--color-text);
  }
  
  .logo-dot:nth-child(3) {
    background-color: var(--color-secondary);
  }
  
  .nav-controls {
    display: flex;
    align-items: center;
    gap: calc(var(--grid-unit) * 2);
  }
  
  /* Hamburger button */
  .hamburger-button {
    background: none;
    border: none;
    cursor: pointer;
    color: var(--color-text);
    padding: var(--grid-unit);
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 1001;
  }
  
  .hamburger {
    display: flex;
    flex-direction: column;
    gap: 6px;
    width: 26px;
    height: 20px;
    justify-content: center;
    transition: all var(--transition-speed);
  }
  
  .hamburger span {
    display: block;
    height: 2px;
    width: 26px;
    background-color: var(--color-text);
    transition: all var(--transition-speed);
    transform-origin: center;
  }
  
  .hamburger span:nth-child(1) {
    width: 16px;
    align-self: flex-end;
  }
  
  .hamburger:hover span:nth-child(1) {
    width: 26px;
  }
  
  /* Menu wrapper */
  .nav-links-wrapper {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100vh;
    background-color: var(--color-bg);
    display: flex;
    flex-direction: column;
    opacity: 0;
    pointer-events: none;
    transition: opacity 0.4s ease, transform 0.4s ease;
    overflow-y: auto;
    padding: calc(var(--grid-unit) * 6) calc(var(--grid-unit) * 3);
    justify-content: space-between;
    transform: translateY(-10px);
  }
  
  .menu-open .nav-links-wrapper {
    opacity: 1;
    pointer-events: all;
    transform: translateY(0);
  }
  
  /* Navigation header */
  .nav-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: calc(var(--grid-unit) * 6);
  }
  
  .theme-toggle {
    background: none;
    border: none;
    cursor: pointer;
    color: var(--color-text);
    padding: 0;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  
  .theme-icon {
    display: flex;
    align-items: center;
    gap: calc(var(--grid-unit) * 1.5);
  }
  
  .close-menu {
    background: none;
    border: none;
    cursor: pointer;
    width: 36px;
    height: 36px;
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
  }
  
  .close-icon {
    width: 24px;
    height: 24px;
    position: relative;
  }
  
  .close-icon span {
    position: absolute;
    top: 50%;
    left: 0;
    width: 24px;
    height: 2px;
    background-color: var(--color-text);
    transform-origin: center;
  }
  
  .close-icon span:first-child {
    transform: translateY(-50%) rotate(45deg);
  }
  
  .close-icon span:last-child {
    transform: translateY(-50%) rotate(-45deg);
  }
  
  /* Nav links */
  .nav-links {
    list-style: none;
    margin: 0;
    padding: 0;
    display: flex;
    flex-direction: column;
    gap: calc(var(--grid-unit) * 4);
  }
  
  .nav-link {
    display: flex;
    align-items: center;
    gap: calc(var(--grid-unit) * 2);
    background: none;
    border: none;
    padding: 0;
    cursor: pointer;
    color: var(--color-text);
    font-weight: 400;
    font-size: 2.5rem;
    text-transform: uppercase;
    letter-spacing: 2px;
    position: relative;
    transition: all var(--transition-speed), opacity 0.4s ease, transform 0.4s ease;
    opacity: 0;
    transform: translateY(10px);
  }
  
  .menu-open .nav-link {
    opacity: 1;
    transform: translateY(0);
  }
  
  .nav-link:hover {
    color: var(--color-accent);
  }
  
  .nav-link-text {
    position: relative;
    display: inline-block;
  }
  
  .active .nav-link {
    color: var(--color-accent);
  }
  
  .nav-link-indicator {
    margin-left: calc(var(--grid-unit) * 1);
    display: inline-block;
    width: 8px;
    height: 8px;
    background-color: var(--color-accent);
  }
  
  /* Nav footer */
  .nav-footer {
    margin-top: auto;
    padding-top: calc(var(--grid-unit) * 6);
  }
  
  .social-links {
    display: flex;
    gap: calc(var(--grid-unit) * 3);
    margin-top: calc(var(--grid-unit) * 4);
    justify-content: center;
  }
  
  .social-link {
    color: var(--color-text);
    text-decoration: none;
    font-size: 0.875rem;
    position: relative;
    transition: color var(--transition-speed);
  }
  
  .social-link:hover {
    color: var(--color-accent);
  }
  
  .social-link::after {
    content: '';
    position: absolute;
    bottom: -4px;
    left: 0;
    width: 0;
    height: 1px;
    background-color: var(--color-accent);
    transition: width var(--transition-speed);
  }
  
  .social-link:hover::after {
    width: 100%;
  }
  
  /* Larger screens */
  @media (min-width: 1024px) {
    .nav-links {
      gap: calc(var(--grid-unit) * 6);
    }
    
    .nav-links-wrapper {
      padding: calc(var(--grid-unit) * 8) calc(var(--grid-unit) * 8);
    }
  }
</style> 