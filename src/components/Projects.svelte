<script>
  import { onMount } from 'svelte';
  
  // Projects data
  const projects = [
    {
      id: 1,
      title: 'CONCRETE MINIMALISM',
      description: 'A residential project emphasizing clean lines and functional spaces, featuring exposed concrete and natural light.',
      image: 'src/assets/buildings1.png',
      year: '2023',
      location: 'Zurich, Switzerland',
      category: 'Residential'
    },
    {
      id: 2,
      title: 'URBAN GALLERY',
      description: 'Modern art exhibition space designed with flexible layouts and innovative lighting solutions.',
      image: 'src/assets/buildings2.png',
      year: '2022',
      location: 'Basel, Switzerland',
      category: 'Cultural'
    },
    {
      id: 3,
      title: 'ALPINE RETREAT',
      description: 'Mountain cabin that blends traditional materials with contemporary design approach.',
      image: 'src/assets/buildings3.png',
      year: '2021',
      location: 'Lucerne, Switzerland',
      category: 'Hospitality'
    },
    {
      id: 4,
      title: 'GEOMETRIC OFFICE',
      description: 'Corporate headquarters featuring modular workspaces and sustainable building practices.',
      image: 'src/assets/buildings4.png',
      year: '2022',
      location: 'Geneva, Switzerland',
      category: 'Commercial'
    }
  ];
  
  // States
  let activeProject = $state(1);
  let imagesLoaded = $state(new Set());
  
  function setActiveProject(id) {
    // Add transition class to trigger fade-out animation
    const projectDetails = document.querySelector('.project-details');
    if (projectDetails) {
      projectDetails.classList.add('transitioning-out');
      
      // Wait for animation to finish before changing project
      setTimeout(() => {
        activeProject = id;
        // Remove transitioning class after state update
        setTimeout(() => {
          const newProjectDetails = document.querySelector('.project-details');
          if (newProjectDetails) {
            newProjectDetails.classList.remove('transitioning-out');
            newProjectDetails.classList.add('transitioning-in');
            
            // Remove transitioning-in class after animation completes
            setTimeout(() => {
              newProjectDetails.classList.remove('transitioning-in');
            }, 500);
          }
        }, 50);
      }, 300);
    } else {
      activeProject = id;
    }
  }
  
  function handleImageLoad(id) {
    imagesLoaded.add(id);
  }
  
  function setupScrollObserver() {
    const animatedElements = [
      { selector: '.section-number', className: 'slide-in-left' },
      { selector: '.section-title h2', className: 'slide-up' },
      { selector: '.section-description', className: 'slide-up' },
      { selector: '.project-nav-item', className: 'fade-in' },
      { selector: '.project-image', className: 'fade-in' },
      { selector: '.project-info', className: 'slide-up' }
    ];
    
    const observer = new IntersectionObserver((entries) => {
      entries.forEach(entry => {
        if (entry.isIntersecting) {
          entry.target.classList.add('visible');
        }
      });
    }, { threshold: 0.2 });
    
    // Add animation classes and observe elements
    animatedElements.forEach(({ selector, className }) => {
      document.querySelectorAll(selector).forEach(el => {
        el.classList.add(className);
        observer.observe(el);
      });
    });
    
    // Cleanup function
    return () => observer.disconnect();
  }
  
  // Lifecycle
  onMount(() => {
    // Set up animations with a small delay to ensure DOM is ready
    setTimeout(() => {
      const cleanup = setupScrollObserver();
      
      // Preload the first project image
      const img = new Image();
      img.onload = () => handleImageLoad(activeProject);
      img.src = projects.find(p => p.id === activeProject).image;
      
      return cleanup;
    }, 100);
  });
</script>

<div class="projects">
  <div class="section-header">
    <div class="section-title">
      <span class="section-number">01</span>
      <h2>PROJECTS</h2>
    </div>
    <p class="section-description">
      A collection of our architectural work showcasing minimalist design principles, functional excellence, and spatial innovation.
    </p>
  </div>
  
  <div class="projects-gallery">
    <div class="projects-navigation">
      {#each projects as project}
        <button 
          class="project-nav-item" 
          class:active={activeProject === project.id}
          on:click={() => setActiveProject(project.id)}
        >
          <span class="project-number">{project.id.toString().padStart(2, '0')}</span>
          <span class="project-title">{project.title}</span>
          {#if activeProject === project.id}
            <span class="nav-indicator"></span>
          {/if}
        </button>
      {/each}
    </div>
    
    <div class="project-showcase">
      {#each projects as project}
        {#if activeProject === project.id}
          <div class="project-details">
            <div class="project-image parallax-element">
              <!-- Loading placeholder -->
              {#if !imagesLoaded.has(project.id)}
                <div class="image-placeholder-loading">
                  <div class="loading-indicator"></div>
                </div>
              {/if}
              
              <!-- Actual image with enhancements -->
              <img 
                src={project.image} 
                alt={project.title}
                class="image-actual project-image-{project.id}"
                class:loaded={imagesLoaded.has(project.id)}
                loading="lazy"
                on:load={() => handleImageLoad(project.id)}
              />
              
              <div class="image-overlay"></div>
            </div>
            
            <div class="project-info">
              <h3>{project.title}</h3>
              <p>{project.description}</p>
              
              <div class="project-meta">
                <div class="meta-item">
                  <span class="meta-label">YEAR</span>
                  <span class="meta-value">{project.year}</span>
                </div>
                <div class="meta-item">
                  <span class="meta-label">LOCATION</span>
                  <span class="meta-value">{project.location}</span>
                </div>
                <div class="meta-item">
                  <span class="meta-label">CATEGORY</span>
                  <span class="meta-value">{project.category}</span>
                </div>
              </div>
              
              <a href="/" class="view-project-btn">VIEW PROJECT</a>
            </div>
          </div>
        {/if}
      {/each}
    </div>
  </div>
</div>

<style>
  .projects {
    width: 100%;
  }
  
  .section-header {
    display: grid;
    grid-template-columns: 1fr;
    gap: calc(var(--grid-unit) * 4);
    margin-bottom: calc(var(--grid-unit) * 8);
  }
  
  .section-title {
    display: flex;
    align-items: baseline;
    gap: calc(var(--grid-unit) * 2);
  }
  
  .section-number {
    font-size: 1rem;
    color: var(--color-accent);
    font-weight: 500;
  }
  
  .section-title h2 {
    font-size: 2.5rem;
    font-weight: 500;
    letter-spacing: -0.02em;
    margin: 0;
  }
  
  .section-description {
    font-size: 1.125rem;
    line-height: 1.6;
    max-width: 550px;
  }
  
  .projects-gallery {
    display: grid;
    grid-template-columns: 1fr;
    gap: calc(var(--grid-unit) * 6);
  }
  
  .projects-navigation {
    display: flex;
    flex-direction: column;
    gap: calc(var(--grid-unit) * 2);
  }
  
  .project-nav-item {
    display: flex;
    align-items: center;
    gap: calc(var(--grid-unit) * 2);
    padding: calc(var(--grid-unit) * 2) 0;
    background: none;
    border: none;
    border-bottom: 1px solid var(--color-border);
    cursor: pointer;
    text-align: left;
    color: var(--color-text);
    transition: all var(--transition-speed);
    position: relative;
  }
  
  .project-nav-item:hover {
    color: var(--color-accent);
  }
  
  .project-nav-item.active {
    color: var(--color-accent);
  }
  
  .project-number {
    font-size: 0.875rem;
    opacity: 0.6;
    width: 32px;
  }
  
  .project-title {
    font-size: 1rem;
    letter-spacing: 1px;
    font-weight: 500;
  }
  
  .nav-indicator {
    position: absolute;
    right: 0;
    width: 8px;
    height: 8px;
    background-color: var(--color-accent);
  }
  
  .project-showcase {
    width: 100%;
  }
  
  .project-details {
    display: grid;
    grid-template-columns: 1fr;
    gap: calc(var(--grid-unit) * 4);
    opacity: 1;
    transform: translateY(0);
    transition: opacity 0.3s ease-out, transform 0.3s ease-out;
  }
  
  .project-details.transitioning-out {
    opacity: 0;
    transform: translateY(20px);
  }
  
  .project-details.transitioning-in {
    animation: fadeIn 0.5s ease-out forwards;
  }
  
  @keyframes fadeIn {
    from {
      opacity: 0;
      transform: translateY(20px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }
  
  .project-image {
    aspect-ratio: 16/9;
    width: 100%;
    overflow: hidden;
    position: relative;
    background-color: var(--color-muted);
    box-shadow: 0 8px 30px rgba(0,0,0,0.12);
    will-change: transform;
  }
  
  /* Parallax effect for project image */
  .parallax-element {
    transition: transform 0.3s ease-out;
  }
  
  @media (prefers-reduced-motion: no-preference) {
    .parallax-element {
      transform: translateY(0);
    }
    
    .parallax-element:hover {
      transform: translateY(-5px);
    }
  }
  
  /* Image loading placeholder */
  .image-placeholder-loading {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: var(--color-muted);
    z-index: 1;
  }
  
  .loading-indicator {
    width: 40px;
    height: 40px;
    border: 3px solid rgba(255, 255, 255, 0.3);
    border-radius: 50%;
    border-top-color: var(--color-accent);
    animation: spin 1s ease-in-out infinite;
  }
  
  @keyframes spin {
    to { transform: rotate(360deg); }
  }
  
  /* Actual image with filters */
  .image-actual {
    width: 100%;
    height: 100%;
    object-fit: cover;
    object-position: center;
    transition: transform 0.8s ease, filter 0.5s ease, opacity 0.8s ease;
    filter: contrast(1.1) brightness(0.95) saturate(1.05);
    will-change: transform, filter;
    transform-origin: center;
    position: relative;
    z-index: 2;
    opacity: 0;
  }
  
  .image-actual:hover {
    filter: contrast(1.15) brightness(1) saturate(1.1);
    transform: scale(1.02);
  }
  
  .image-actual.loaded {
    opacity: 1;
  }
  
  .image-overlay {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: linear-gradient(180deg, rgba(0,0,0,0) 0%, rgba(0,0,0,0.3) 100%);
    z-index: 3;
    pointer-events: none;
  }
  
  .project-info {
    padding: calc(var(--grid-unit) * 2);
  }
  
  .project-info h3 {
    font-size: 1.5rem;
    margin: 0 0 calc(var(--grid-unit) * 2) 0;
    font-weight: 500;
  }
  
  .project-info p {
    margin: 0 0 calc(var(--grid-unit) * 4) 0;
    line-height: 1.6;
  }
  
  .project-meta {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
    gap: calc(var(--grid-unit) * 3);
    margin-bottom: calc(var(--grid-unit) * 4);
    border-top: 1px solid var(--color-border);
    padding-top: calc(var(--grid-unit) * 3);
  }
  
  .meta-item {
    display: flex;
    flex-direction: column;
    gap: calc(var(--grid-unit));
  }
  
  .meta-label {
    font-size: 0.75rem;
    opacity: 0.6;
  }
  
  .meta-value {
    font-size: 0.875rem;
    font-weight: 500;
  }
  
  .view-project-btn {
    display: inline-block;
    background-color: transparent;
    border: 1px solid var(--color-text);
    color: var(--color-text);
    text-decoration: none;
    padding: calc(var(--grid-unit) * 1.5) calc(var(--grid-unit) * 3);
    font-size: 0.875rem;
    font-weight: 500;
    letter-spacing: 1px;
    transition: all var(--transition-speed);
    position: relative;
    overflow: hidden;
  }
  
  .view-project-btn:hover {
    background-color: var(--color-text);
    color: var(--color-bg);
  }
  
  .view-project-btn::after {
    content: "";
    position: absolute;
    width: 0;
    height: 2px;
    bottom: 0;
    left: 0;
    background-color: var(--color-accent);
    transition: width 0.3s ease;
  }
  
  .view-project-btn:hover::after {
    width: 100%;
  }
  
  @media (min-width: 768px) {
    .section-header {
      grid-template-columns: 1fr 1fr;
      align-items: end;
    }
    
    .projects-gallery {
      grid-template-columns: 300px 1fr;
    }
    
    .project-details {
      grid-template-columns: 1fr 400px;
    }
    
    .project-image {
      aspect-ratio: 1/1.5;
    }
  }
</style> 