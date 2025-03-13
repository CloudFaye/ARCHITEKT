<script>
  // Hero component props
</script>

<div class="hero">
  <div class="hero-content">
    <div class="hero-title-container">
      <h1 class="hero-title">
        <span class="title-line">SWISS</span>
        <span class="title-line">DESIGN</span>
        <span class="title-line">ARCHITECTURE</span>
      </h1>
    </div>
    
    <div class="hero-description">
      <p>Combining minimalism, functionality, and precision in architectural projects that inspire and transform spaces.</p>
      
      <div class="hero-cta">
        <a href="#projects" class="cta-button">VIEW PROJECTS</a>
        <div class="scroll-indicator">
          <span>SCROLL</span>
          <svg viewBox="0 0 24 24" width="24" height="24"><path fill="currentColor" d="M12 19.59l-6.3-6.3 1.41-1.41 4.89 4.89 4.89-4.89 1.41 1.41-6.3 6.3z"></path></svg>
        </div>
      </div>
    </div>
  </div>
  
  <div class="hero-grid">
    <div class="grid-lines horizontal">
      {#each Array(10) as _, i}
        <div class="grid-line"></div>
      {/each}
    </div>
    <div class="grid-lines vertical">
      {#each Array(10) as _, i}
        <div class="grid-line"></div>
      {/each}
    </div>
  </div>
</div>

<style>
  .hero {
    position: relative;
    min-height: 100vh;
    display: flex;
    align-items: center;
    padding-top: calc(var(--grid-unit) * 10);
    overflow: hidden;
  }
  
  .hero-grid {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    pointer-events: none;
    z-index: 0;
  }
  
  .grid-lines {
    position: absolute;
    width: 100%;
    height: 100%;
  }
  
  .horizontal {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
  }
  
  .vertical {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
  }
  
  .grid-line {
    background-color: var(--color-text);
    opacity: 0.05;
  }
  
  .horizontal .grid-line {
    width: 100%;
    height: 1px;
  }
  
  .vertical .grid-line {
    height: 100%;
    width: 1px;
  }
  
  .hero-content {
    position: relative;
    z-index: 1;
    width: 100%;
    display: grid;
    grid-template-columns: 1fr;
    gap: calc(var(--grid-unit) * 6);
  }
  
  .hero-title-container {
    overflow: hidden;
  }
  
  .hero-title {
    font-size: clamp(2.5rem, 8vw, 6rem);
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: -0.02em;
    line-height: 1;
    margin: 0;
    display: flex;
    flex-direction: column;
  }
  
  .title-line {
    display: block;
    transform: translateY(0);
    transition: transform 0.5s ease;
  }
  
  .title-line:hover {
    transform: translateY(-5px);
  }
  
  .hero-description {
    max-width: 500px;
  }
  
  .hero-description p {
    font-size: 1.25rem;
    line-height: 1.6;
    margin-bottom: calc(var(--grid-unit) * 6);
  }
  
  .hero-cta {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  
  .cta-button {
    background-color: var(--color-text);
    color: var(--color-bg);
    text-decoration: none;
    padding: calc(var(--grid-unit) * 2) calc(var(--grid-unit) * 3);
    font-size: 0.875rem;
    font-weight: 500;
    letter-spacing: 2px;
    transition: all var(--transition-speed);
  }
  
  .cta-button:hover {
    background-color: var(--color-accent);
  }
  
  .scroll-indicator {
    display: flex;
    align-items: center;
    gap: calc(var(--grid-unit));
    animation: bounce 2s infinite;
  }
  
  .scroll-indicator span {
    font-size: 0.75rem;
    font-weight: 500;
    letter-spacing: 2px;
  }
  
  @keyframes bounce {
    0%, 100% { transform: translateY(0); }
    50% { transform: translateY(5px); }
  }
  
  @media (min-width: 768px) {
    .hero-content {
      grid-template-columns: 1fr 1fr;
    }
    
    .hero-description {
      max-width: none;
    }
  }
</style> 