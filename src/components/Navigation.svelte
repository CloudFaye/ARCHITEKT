<script>
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
      <button class="theme-toggle" on:click={onToggleDarkMode} aria-label="Toggle dark mode">
        {#if isDarkMode}
          <svg viewBox="0 0 24 24" width="24" height="24"><path fill="currentColor" d="M12 10.999c1.437.438 2.562 1.564 2.999 3.001.44-1.437 1.565-2.562 3.001-3-1.436-.439-2.561-1.563-3.001-3-.437 1.436-1.562 2.561-2.999 2.999zm8.001.001c.958.293 1.707 1.042 2 2.001.291-.959 1.042-1.709 1.999-2.001-.957-.292-1.707-1.042-2-2-.293.958-1.042 1.708-1.999 2zm-1-9c-.437 1.437-1.563 2.562-2.998 3.001 1.438.44 2.561 1.564 3.001 3.002.437-1.438 1.563-2.563 2.996-3.002-1.433-.437-2.559-1.564-2.999-3.001zm-7.001 22c-6.617 0-12-5.383-12-12s5.383-12 12-12c1.894 0 3.63.497 5.37 1.179-2.948.504-9.37 3.266-9.37 10.821 0 7.454 5.917 10.208 9.37 10.821-1.5.846-3.476 1.179-5.37 1.179z"/></svg>
        {:else}
          <svg viewBox="0 0 24 24" width="24" height="24"><path fill="currentColor" d="M4.069 13h-4.069v-2h4.069c-.041.328-.069.661-.069 1s.028.672.069 1zm3.034-7.312l-2.881-2.881-1.414 1.414 2.881 2.881c.411-.529.885-1.003 1.414-1.414zm11.209 1.414l2.881-2.881-1.414-1.414-2.881 2.881c.528.411 1.002.886 1.414 1.414zm-6.312-3.102c.339 0 .672.028 1 .069v-4.069h-2v4.069c.328-.041.661-.069 1-.069zm0 16c-.339 0-.672-.028-1-.069v4.069h2v-4.069c-.328.041-.661.069-1 .069zm7.931-9c.041.328.069.661.069 1s-.028.672-.069 1h4.069v-2h-4.069zm-3.033 7.312l2.88 2.88 1.415-1.414-2.88-2.88c-.412.528-.886 1.002-1.415 1.414zm-11.21-1.415l-2.88 2.88 1.414 1.414 2.88-2.88c-.528-.411-1.003-.885-1.414-1.414zm6.312-10.897c-3.314 0-6 2.686-6 6s2.686 6 6 6 6-2.686 6-6-2.686-6-6-6z"/></svg>
        {/if}
      </button>
      
      <button class="menu-toggle" on:click={onToggleMenu} aria-label="Toggle menu">
        <div class="hamburger">
          <span></span>
          <span></span>
        </div>
      </button>
    </div>
  </div>
  
  <div class="nav-links-wrapper">
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
  
  .theme-toggle, .menu-toggle {
    background: none;
    border: none;
    cursor: pointer;
    color: var(--color-text);
    padding: var(--grid-unit);
    display: flex;
    align-items: center;
    justify-content: center;
  }
  
  .hamburger {
    display: flex;
    flex-direction: column;
    gap: 6px;
    width: 24px;
    height: 18px;
    justify-content: center;
    transition: all var(--transition-speed);
  }
  
  .hamburger span {
    display: block;
    height: 2px;
    width: 24px;
    background-color: var(--color-text);
    transition: all var(--transition-speed);
  }
  
  .menu-open .hamburger span:first-child {
    transform: translateY(4px) rotate(45deg);
  }
  
  .menu-open .hamburger span:last-child {
    transform: translateY(-4px) rotate(-45deg);
  }
  
  .nav-links-wrapper {
    height: 0;
    overflow: hidden;
    transition: height var(--transition-speed);
  }
  
  .menu-open .nav-links-wrapper {
    height: calc(100vh - 70px);
    border-top: 1px solid var(--color-border);
  }
  
  .nav-links {
    list-style: none;
    margin: 0;
    padding: calc(var(--grid-unit) * 4) calc(var(--grid-unit) * 3);
    display: flex;
    flex-direction: column;
    gap: calc(var(--grid-unit) * 6);
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
    transition: all var(--transition-speed);
  }
  
  .nav-link-text {
    position: relative;
    display: inline-block;
  }
  
  .active .nav-link-text::after {
    content: '';
    position: absolute;
    bottom: -4px;
    left: 0;
    width: 100%;
    height: 2px;
    background-color: var(--color-accent);
  }
  
  .nav-link-indicator {
    position: absolute;
    left: -24px;
    width: 16px;
    height: 16px;
    border-radius: 50%;
    background-color: var(--color-accent);
  }
  
  @media (min-width: 769px) {
    .menu-toggle {
      display: none;
    }
    
    .nav-container {
      padding: calc(var(--grid-unit) * 3) calc(var(--grid-unit) * 3);
    }
    
    .nav-links-wrapper {
      height: auto !important;
      position: absolute;
      right: calc(var(--grid-unit) * 3);
      top: 50%;
      transform: translateY(-50%);
      border: none !important;
    }
    
    .nav-links {
      flex-direction: row;
      gap: calc(var(--grid-unit) * 4);
      padding: 0;
    }
    
    .nav-link {
      font-size: 0.875rem;
      font-weight: 500;
    }
    
    .nav-link-indicator {
      position: absolute;
      left: 50%;
      transform: translateX(-50%);
      top: -24px;
      width: 8px;
      height: 8px;
    }
    
    .active .nav-link-text::after {
      display: none;
    }
  }
</style> 