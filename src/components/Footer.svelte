<script>
  // Props for the footer component
  let { isDarkMode } = $props();
  
  // Get current year
  const currentYear = new Date().getFullYear();
</script>

<footer class="footer">
  <div class="footer-grid">
    <div class="footer-brand">
      <a href="/" class="footer-logo">
        <div class="logo-grid">
          <div class="logo-dot"></div>
          <div class="logo-dot"></div>
          <div class="logo-dot"></div>
          <div class="logo-dot"></div>
        </div>
        <span>ARCHITEKT</span>
      </a>
      <p class="tagline">Swiss design principles applied to modern architectural solutions.</p>
    </div>
    
    <div class="footer-nav">
      <h4>NAVIGATION</h4>
      <ul>
        <li><a href="#home">Home</a></li>
        <li><a href="#projects">Projects</a></li>
        <li><a href="#design">Design Language</a></li>
        <li><a href="#contact">Contact</a></li>
      </ul>
    </div>
    
    <div class="footer-contact">
      <h4>CONTACT</h4>
      <address>
        123 Design Avenue<br>
        Zurich, Switzerland<br>
        CH-8001<br>
        <a href="mailto:hello@architekt.com">hello@architekt.com</a><br>
        <a href="tel:+41881234567">+41 88 123 45 67</a>
      </address>
    </div>
    
    <div class="footer-newsletter">
      <h4>SUBSCRIBE</h4>
      <p>Sign up for our newsletter to receive updates on our latest projects and design insights.</p>
      <form class="newsletter-form">
        <input type="email" placeholder="Your email address" aria-label="Email address for newsletter">
        <button type="submit" aria-label="Subscribe to newsletter">→</button>
      </form>
    </div>
  </div>
  
  <div class="footer-bottom">
    <div class="copyright">
      © {currentYear} ARCHITEKT. All rights reserved.
    </div>
    <div class="footer-links">
      <a href="/">Privacy Policy</a>
      <span class="divider">|</span>
      <a href="/">Terms of Service</a>
    </div>
  </div>
</footer>

<style>
  .footer {
    margin-top: calc(var(--grid-unit) * 12);
    border-top: 1px solid var(--color-border);
    padding-top: calc(var(--grid-unit) * 8);
    padding-bottom: calc(var(--grid-unit) * 4);
    width: 100%;
  }
  
  .footer-grid {
    display: grid;
    grid-template-columns: 1fr;
    gap: calc(var(--grid-unit) * 6);
    max-width: 1440px;
    margin: 0 auto;
    padding: 0 calc(var(--grid-unit) * 3);
  }
  
  .footer-brand {
    display: flex;
    flex-direction: column;
    gap: calc(var(--grid-unit) * 2);
  }
  
  .footer-logo {
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
  
  .tagline {
    font-size: 0.9375rem;
    color: var(--color-secondary);
    line-height: 1.6;
    margin: 0;
  }
  
  .footer-nav h4,
  .footer-contact h4,
  .footer-newsletter h4 {
    font-size: 0.875rem;
    font-weight: 500;
    margin: 0 0 calc(var(--grid-unit) * 3) 0;
    letter-spacing: 1px;
  }
  
  .footer-nav ul {
    list-style: none;
    padding: 0;
    margin: 0;
    display: flex;
    flex-direction: column;
    gap: calc(var(--grid-unit) * 2);
  }
  
  .footer-nav a,
  .footer-contact a {
    text-decoration: none;
    color: var(--color-text);
    font-size: 0.9375rem;
    transition: color var(--transition-speed);
  }
  
  .footer-nav a:hover,
  .footer-contact a:hover {
    color: var(--color-accent);
  }
  
  .footer-contact address {
    font-style: normal;
    font-size: 0.9375rem;
    line-height: 1.6;
    color: var(--color-secondary);
  }
  
  .footer-newsletter p {
    font-size: 0.9375rem;
    line-height: 1.6;
    color: var(--color-secondary);
    margin: 0 0 calc(var(--grid-unit) * 3) 0;
  }
  
  .newsletter-form {
    display: flex;
    border-bottom: 1px solid var(--color-border);
  }
  
  .newsletter-form input {
    flex: 1;
    border: none;
    background: transparent;
    padding: calc(var(--grid-unit)) 0;
    font-size: 0.9375rem;
    color: var(--color-text);
    font-family: var(--font-sans);
  }
  
  .newsletter-form input:focus {
    outline: none;
  }
  
  .newsletter-form button {
    background: transparent;
    border: none;
    color: var(--color-text);
    font-size: 1.25rem;
    cursor: pointer;
    padding: calc(var(--grid-unit));
    transition: color var(--transition-speed);
  }
  
  .newsletter-form button:hover {
    color: var(--color-accent);
  }
  
  .footer-bottom {
    display: flex;
    flex-direction: column;
    gap: calc(var(--grid-unit) * 2);
    margin-top: calc(var(--grid-unit) * 8);
    padding: 0 calc(var(--grid-unit) * 3);
    max-width: 1440px;
    margin-left: auto;
    margin-right: auto;
    border-top: 1px solid var(--color-border);
    padding-top: calc(var(--grid-unit) * 4);
    font-size: 0.75rem;
    color: var(--color-secondary);
  }
  
  .footer-links {
    display: flex;
    gap: calc(var(--grid-unit) * 2);
  }
  
  .footer-links a {
    color: var(--color-secondary);
    text-decoration: none;
    transition: color var(--transition-speed);
  }
  
  .footer-links a:hover {
    color: var(--color-accent);
  }
  
  .divider {
    color: var(--color-border);
  }
  
  @media (min-width: 768px) {
    .footer-grid {
      grid-template-columns: 2fr 1fr 1fr 2fr;
    }
    
    .footer-bottom {
      flex-direction: row;
      justify-content: space-between;
    }
  }
</style> 