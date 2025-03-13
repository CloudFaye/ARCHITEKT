<script>
  import ImageWithEffect from '../components/ImageWithEffect.svelte';
  
  // Design principles data
  const designPrinciples = [
    {
      id: 1,
      title: 'MINIMALISM',
      description: 'Reducing design elements to only what is necessary, focusing on simplicity, clarity and elimination of the superfluous.'
    },
    {
      id: 2,
      title: 'PRECISION',
      description: 'Executing each design with meticulous attention to detail, proportions and construction quality.'
    },
    {
      id: 3,
      title: 'FUNCTIONALITY',
      description: 'Designing with purpose where form follows function and every element serves a practical purpose.'
    },
    {
      id: 4,
      title: 'WHITE SPACE',
      description: 'Utilizing negative space as an active design element to create breathing room and visual clarity.'
    }
  ];
  
  // Stats data
  const stats = [
    { value: '95', unit: '%', label: 'SUSTAINABLE MATERIALS' },
    { value: '120', unit: '+', label: 'COMPLETED PROJECTS' },
    { value: '15', unit: '', label: 'DESIGN AWARDS' }
  ];
  
  // Quote
  const quote = {
    text: "Design is not just what it looks like and feels like. Design is how it works.",
    author: "Steve Jobs"
  };
  
  // Background image
  const backgroundImage = "src/assets/texture2.png";
  
  let isBackgroundLoaded = $state(false);
  
  function handleBackgroundLoad() {
    isBackgroundLoaded = true;
  }
</script>

<section class="design-language-section">
  <div class="background-image-container">
    <div class="background-overlay"></div>
    <img 
      src={backgroundImage} 
      alt="Textured background" 
      class="background-image" 
      class:loaded={isBackgroundLoaded}
      on:load={handleBackgroundLoad}
    />
  </div>
  
  <div class="design-language-content">
    <div class="section-header">
      <h2 class="section-title slide-up">Design Philosophy</h2>
      <p class="section-subtitle slide-up">Our approach to creating timeless, functional aesthetics</p>
    </div>
    
    <div class="design-principles">
      {#each designPrinciples as principle}
        <div class="principle-item slide-up">
          <h3 class="principle-title">{principle.title}</h3>
          <p class="principle-description">{principle.description}</p>
        </div>
      {/each}
    </div>
    
    <div class="design-quote slide-in-left">
      <blockquote>"{quote.text}"</blockquote>
      <cite>— {quote.author}</cite>
    </div>
    
    <div class="design-stats">
      {#each stats as stat}
        <div class="stat-item zoom-in">
          <div class="stat-value">{stat.value}<span>{stat.unit}</span></div>
          <div class="stat-label">{stat.label}</div>
        </div>
      {/each}
    </div>
  </div>
</section>

<style>
  .design-language-section {
    position: relative;
    width: 100%;
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    overflow: hidden;
  }
  
  .background-image-container {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: -1;
    overflow: hidden;
  }
  
  .background-overlay {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: var(--color-bg);
    opacity: 0.92;
    z-index: 1;
  }
  
  .background-image {
    width: 100%;
    height: 100%;
    object-fit: cover;
    opacity: 0;
    transition: opacity 1s ease;
    filter: grayscale(100%) contrast(0.8) brightness(0.6);
  }
  
  .background-image.loaded {
    opacity: 1;
  }
  
  .design-language-content {
    padding: 6rem 2rem;
    max-width: 1200px;
    width: 100%;
    display: flex;
    flex-direction: column;
    gap: 5rem;
  }
  
  .section-header {
    text-align: center;
    margin-bottom: 2rem;
  }
  
  .section-title {
    font-size: 3.5rem;
    font-weight: 600;
    margin: 0;
    letter-spacing: -0.03em;
  }
  
  .section-subtitle {
    font-size: 1.2rem;
    color: var(--color-secondary);
    margin: 1rem 0 0 0;
  }
  
  .design-principles {
    display: grid;
    grid-template-columns: repeat(1, 1fr);
    gap: 2rem;
  }
  
  .principle-item {
    padding: 2rem;
    background-color: var(--color-muted);
    position: relative;
    overflow: hidden;
  }
  
  .principle-title {
    font-size: 1.5rem;
    font-weight: 600;
    margin: 0 0 1rem 0;
    letter-spacing: 0.05em;
  }
  
  .principle-description {
    font-size: 1rem;
    line-height: 1.6;
    margin: 0;
    max-width: 90%;
  }
  
  .design-quote {
    font-size: 1.8rem;
    line-height: 1.4;
    max-width: 800px;
    margin: 0 auto;
    text-align: center;
    padding: 0 2rem;
  }
  
  .design-quote blockquote {
    margin: 0 0 1rem 0;
    font-style: italic;
    font-weight: 300;
  }
  
  .design-quote cite {
    font-style: normal;
    font-size: 1rem;
    color: var(--color-secondary);
    display: block;
  }
  
  .design-stats {
    display: grid;
    grid-template-columns: repeat(1, 1fr);
    gap: 2rem;
  }
  
  .stat-item {
    text-align: center;
    padding: 2rem;
    background-color: var(--color-muted);
  }
  
  .stat-value {
    font-size: 3.5rem;
    font-weight: 700;
    line-height: 1;
    margin-bottom: 0.5rem;
    display: flex;
    justify-content: center;
    align-items: baseline;
  }
  
  .stat-value span {
    font-size: 1.5rem;
    margin-left: 2px;
  }
  
  .stat-label {
    font-size: 0.8rem;
    letter-spacing: 0.1em;
    font-weight: 500;
    color: var(--color-secondary);
  }
  
  @media (min-width: 768px) {
    .design-principles {
      grid-template-columns: repeat(2, 1fr);
    }
    
    .design-stats {
      grid-template-columns: repeat(3, 1fr);
    }
  }
  
  @media (min-width: 992px) {
    .design-language-content {
      padding: 8rem 2rem;
    }
    
    .design-principles {
      grid-template-columns: repeat(4, 1fr);
    }
    
    .section-title {
      font-size: 4rem;
    }
  }
</style> 