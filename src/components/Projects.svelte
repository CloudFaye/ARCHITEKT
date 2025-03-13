<script>
  import { onMount } from 'svelte';
  import { slide, fade } from 'svelte/transition';
  import { cubicOut } from 'svelte/easing';
  
  // Combine all projects into one array
  const allProjects = [
    {
      id: 1,
      title: 'SALAD',
      subtitle: 'Healthy apparel',
      description: 'Contemporary clothing with environmentally-friendly materials and minimalist aesthetic.',
      image: 'src/assets/project1.png',
      year: '2023',
      tags: ['Fashion', 'Sustainability', 'Branding']
    },
    {
      id: 2,
      title: 'AEGLE\'S',
      subtitle: 'Glowing skin naturally',
      description: 'Premium skincare formulated with organic ingredients and scientific innovation.',
      image: 'src/assets/project2.png',
      year: '2022',
      tags: ['Beauty', 'Packaging', 'Identity']
    },
    {
      id: 3,
      title: 'CHASER',
      subtitle: 'Ergonomic cybersecurity',
      description: 'Digital security solutions with a focus on user-friendly interfaces and robust protection.',
      image: 'src/assets/project3.png',
      year: '2022',
      tags: ['Tech', 'Security', 'Digital']
    },
    {
      id: 4,
      title: 'VERTEX',
      subtitle: 'Architectural innovation',
      description: 'Cutting-edge structural designs that blend form and function in urban environments.',
      image: 'src/assets/project4.png',
      year: '2021',
      tags: ['Architecture', 'Urban', 'Design']
    },
    {
      id: 5,
      title: 'CONCRETE MINIMALISM',
      subtitle: 'Residential spaces',
      description: 'Clean lines and functional spaces with exposed concrete and natural light.',
      image: 'src/assets/buildings1.png',
      year: '2023',
      tags: ['Residential', 'Minimalist', 'Zurich']
    },
    {
      id: 6,
      title: 'URBAN GALLERY',
      subtitle: 'Cultural spaces',
      description: 'Exhibition space designed with flexible layouts and innovative lighting solutions.',
      image: 'src/assets/buildings2.png',
      year: '2022',
      tags: ['Cultural', 'Exhibition', 'Basel']
    },
    {
      id: 7,
      title: 'ALPINE RETREAT',
      subtitle: 'Mountain getaway',
      description: 'Cabin blending traditional materials with contemporary design approach.',
      image: 'src/assets/buildings3.png',
      year: '2021',
      tags: ['Hospitality', 'Alpine', 'Lucerne']
    },
    {
      id: 8,
      title: 'GEOMETRIC OFFICE',
      subtitle: 'Workspace solutions',
      description: 'Corporate headquarters featuring modular spaces and sustainable building practices.',
      image: 'src/assets/buildings4.png',
      year: '2022',
      tags: ['Commercial', 'Office', 'Geneva']
    }
  ];
  
  // States
  let hoveredProject = $state(null);
  let inViewProjects = $state(new Set());
  let loadedImages = $state(new Set());
  
  function setHoveredProject(id) {
    hoveredProject = id;
  }
  
  function clearHoveredProject() {
    hoveredProject = null;
  }
  
  function handleImageLoad(id) {
    loadedImages.add(id);
  }
  
  function setupScrollObserver() {
    const observer = new IntersectionObserver(
      (entries) => {
        entries.forEach(entry => {
          const projectId = parseInt(entry.target.getAttribute('data-project-id'), 10);
          
          if (entry.isIntersecting) {
            // Add project to in-view set
            inViewProjects.add(projectId);
          } else {
            // Remove project from in-view set
            inViewProjects.delete(projectId);
          }
        });
      },
      { 
        threshold: 0.15,
        rootMargin: '0px 0px -10% 0px'
      }
    );
    
    // Observe all project items
    document.querySelectorAll('.project-item').forEach(item => {
      observer.observe(item);
    });
    
    return () => observer.disconnect();
  }
  
  // Lifecycle
  onMount(() => {
    // Allow DOM to settle before setting up observer
    setTimeout(setupScrollObserver, 100);
  });
</script>

<section class="projects-section">
  <div class="projects-header">
    <h2>projects</h2>
  </div>
  
  <div class="projects-grid">
    {#each allProjects as project (project.id)}
      <!-- svelte-ignore a11y_no_static_element_interactions -->
      <div 
        class="project-item" 
        data-project-id={project.id}
        on:mouseenter={() => setHoveredProject(project.id)}
        on:mouseleave={clearHoveredProject}
        class:in-view={inViewProjects.has(project.id)}
        class:hovered={hoveredProject === project.id}
      >
        <div class="project-image-container">
          <div class="image-placeholder" class:hidden={loadedImages.has(project.id)}></div>
          <img 
            src={project.image} 
            alt={project.title} 
            class="project-image" 
            loading="lazy" 
            on:load={() => handleImageLoad(project.id)}
            class:loaded={loadedImages.has(project.id)}
          />
        </div>
        
        <div class="project-content">
          <h3 class="project-title">{project.title}</h3>
          <p class="project-subtitle">{project.subtitle}</p>
        </div>
      </div>
    {/each}
  </div>
</section>

<style>
  .projects-section {
    padding: 4rem 0;
    width: 100%;
  }
  
  .projects-header {
    margin-bottom: 4rem;
    padding: 0 1rem;
    text-align: center;
  }
  
  h2 {
    font-size: 3rem;
    font-weight: 600;
    letter-spacing: -0.02em;
    color: var(--color-text);
    margin: 0;
  }
  
  .projects-grid {
    display: grid;
    grid-template-columns: repeat(1, 1fr);
    gap: 2rem;
    margin-top: 2rem;
  }
  
  .project-item {
    display: grid;
    gap: 1rem;
    opacity: 0;
    transform: translateY(20px);
    transition: opacity 0.8s ease, transform 0.8s cubic-bezier(0.16, 1, 0.3, 1);
    overflow: hidden;
    cursor: pointer;
  }
  
  .project-item.in-view {
    opacity: 1;
    transform: translateY(0);
  }
  
  .project-image-container {
    position: relative;
    width: 100%;
    aspect-ratio: 3/2;
    overflow: hidden;
    background-color: var(--color-muted);
  }
  
  .image-placeholder {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: var(--color-muted);
    z-index: 1;
    opacity: 1;
    transition: opacity 0.5s ease;
  }
  
  .image-placeholder.hidden {
    opacity: 0;
    z-index: -1;
  }
  
  .project-image {
    width: 100%;
    height: 100%;
    object-fit: cover;
    transition: transform 1.2s cubic-bezier(0.16, 1, 0.3, 1), opacity 0.5s ease, filter 0.5s ease;
    opacity: 0;
    filter: grayscale(100%);
  }
  
  .project-image.loaded {
    opacity: 1;
  }
  
  .project-item.hovered .project-image {
    transform: scale(1.05);
    filter: grayscale(0%);
  }
  
  .project-content {
    padding: 0.5rem 0 1.5rem;
  }
  
  .project-title {
    font-size: 1.5rem;
    font-weight: 600;
    letter-spacing: 0.02em;
    margin: 0;
    margin-bottom: 0.5rem;
  }
  
  .project-subtitle {
    font-size: 1rem;
    margin: 0;
    color: var(--color-secondary);
  }
  
  @media (min-width: 768px) {
    .projects-grid {
      grid-template-columns: repeat(2, 1fr);
      gap: 2rem;
    }
  }
  
  @media (min-width: 992px) {
    .projects-header {
      padding: 0;
    }
    
    .projects-grid {
      grid-template-columns: repeat(3, 1fr);
      gap: 2.5rem;
    }
  }
  
  @media (min-width: 1200px) {
    .projects-grid {
      grid-template-columns: repeat(4, 1fr);
      gap: 3rem;
    }
  }
</style> 