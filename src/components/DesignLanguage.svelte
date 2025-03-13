<script>
  // Design principles data
  const designPrinciples = [
    {
      id: 1,
      title: 'MINIMALISM',
      description: 'Reducing design elements to only what is necessary, focusing on simplicity, clarity and elimination of the superfluous.',
      icon: '□'
    },
    {
      id: 2,
      title: 'GRID SYSTEMS',
      description: 'Employing mathematical grid structures to create visual harmony, alignment and spatial organization in our designs.',
      icon: '▦'
    },
    {
      id: 3,
      title: 'TYPOGRAPHY',
      description: 'Using clean, sans-serif typography with careful attention to hierarchy, spacing, and readability.',
      icon: 'A'
    },
    {
      id: 4,
      title: 'PRECISION',
      description: 'Executing each design with meticulous attention to detail, proportions and construction quality.',
      icon: '+'
    },
    {
      id: 5,
      title: 'FUNCTIONALITY',
      description: 'Designing with purpose where form follows function and every element serves a practical purpose.',
      icon: '▢'
    },
    {
      id: 6,
      title: 'WHITE SPACE',
      description: 'Utilizing negative space as an active design element to create breathing room and visual clarity.',
      icon: '◯'
    }
  ];
</script>

<div class="design-language">
  <div class="section-header">
    <div class="section-title">
      <span class="section-number">02</span>
      <h2>DESIGN LANGUAGE</h2>
    </div>
    <p class="section-description">
      Our architectural approach is rooted in Swiss design principles, emphasizing clarity, functionality, and timeless aesthetics.
    </p>
  </div>
  
  <div class="principles-grid">
    {#each designPrinciples as principle}
      <div class="principle-card">
        <div class="principle-icon">{principle.icon}</div>
        <h3 class="principle-title">{principle.title}</h3>
        <p class="principle-description">{principle.description}</p>
      </div>
    {/each}
  </div>
  
  <div class="design-showcase">
    <div class="showcase-header">
      <h3>OUR APPROACH</h3>
      <div class="showcase-line"></div>
    </div>
    
    <div class="showcase-content">
      <div class="design-quote">
        <blockquote>
          "Architecture should speak of its time and place, but yearn for timelessness."
        </blockquote>
        <cite>— Frank Gehry</cite>
      </div>
      
      <div class="design-stats">
        <div class="stat-item">
          <div class="stat-value">95%</div>
          <div class="stat-label">SUSTAINABLE MATERIALS</div>
        </div>
        <div class="stat-item">
          <div class="stat-value">100+</div>
          <div class="stat-label">COMPLETED PROJECTS</div>
        </div>
        <div class="stat-item">
          <div class="stat-value">15</div>
          <div class="stat-label">DESIGN AWARDS</div>
        </div>
      </div>
    </div>
  </div>
</div>

<style>
  .design-language {
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
  
  .principles-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
    gap: calc(var(--grid-unit) * 4);
    margin-bottom: calc(var(--grid-unit) * 10);
  }
  
  .principle-card {
    padding: calc(var(--grid-unit) * 3);
    border: 1px solid var(--color-border);
    transition: all var(--transition-speed);
  }
  
  .principle-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 5px 15px rgba(0, 0, 0, 0.05);
  }
  
  .principle-icon {
    font-size: 2.5rem;
    margin-bottom: calc(var(--grid-unit) * 3);
    color: var(--color-accent);
  }
  
  .principle-title {
    font-size: 1.25rem;
    font-weight: 500;
    margin: 0 0 calc(var(--grid-unit) * 2) 0;
    letter-spacing: 1px;
  }
  
  .principle-description {
    font-size: 0.9375rem;
    line-height: 1.6;
    margin: 0;
    color: var(--color-secondary);
  }
  
  .design-showcase {
    margin-top: calc(var(--grid-unit) * 8);
  }
  
  .showcase-header {
    display: flex;
    align-items: center;
    gap: calc(var(--grid-unit) * 3);
    margin-bottom: calc(var(--grid-unit) * 6);
    justify-content: center;
  }
  
  .showcase-header h3 {
    font-size: 1.5rem;
    font-weight: 500;
    margin: 0;
    white-space: nowrap;
  }
  
  .showcase-line {
    flex-grow: 1;
    height: 1px;
    background-color: var(--color-border);
  }
  
  .showcase-content {
    display: grid;
    grid-template-columns: 1fr;
    gap: calc(var(--grid-unit) * 6);
  }
  
  .design-quote {
    font-size: 1.5rem;
    line-height: 1.4;
    padding: calc(var(--grid-unit) * 2);
    position: relative;
  }
  
  .design-quote blockquote {
    font-style: italic;
    margin: 0 0 calc(var(--grid-unit) * 2) 0;
    position: relative;
    padding-left: calc(var(--grid-unit) * 4);
  }
  
  .design-quote blockquote::before {
    content: '';
    position: absolute;
    left: 0;
    top: 0;
    height: 100%;
    width: 2px;
    background-color: var(--color-accent);
  }
  
  .design-quote cite {
    font-style: normal;
    font-size: 1rem;
    opacity: 0.7;
    display: block;
    text-align: right;
  }
  
  .design-stats {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
    gap: calc(var(--grid-unit) * 3);
  }
  
  .stat-item {
    text-align: center;
    padding: calc(var(--grid-unit) * 3);
    background-color: var(--color-muted);
  }
  
  .stat-value {
    font-size: 2.5rem;
    font-weight: 700;
    margin-bottom: calc(var(--grid-unit));
    color: var(--color-accent);
  }
  
  .stat-label {
    font-size: 0.75rem;
    letter-spacing: 1px;
    font-weight: 500;
  }
  
  @media (min-width: 768px) {
    .section-header {
      grid-template-columns: 1fr 1fr;
      align-items: end;
    }
    
    .showcase-content {
      grid-template-columns: 1fr 1fr;
    }
  }
</style> 