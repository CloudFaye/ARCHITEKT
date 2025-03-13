<script>
  // Form state
  let name = $state('');
  let email = $state('');
  let message = $state('');
  let formSubmitted = $state(false);
  
  // Simple form validation
  let nameError = $state('');
  let emailError = $state('');
  let messageError = $state('');
  
  function validateEmail(email) {
    const re = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    return re.test(email);
  }
  
  function handleSubmit() {
    // Reset errors
    nameError = '';
    emailError = '';
    messageError = '';
    
    let isValid = true;
    
    // Validate inputs
    if (!name.trim()) {
      nameError = 'Please enter your name';
      isValid = false;
    }
    
    if (!email.trim()) {
      emailError = 'Please enter your email';
      isValid = false;
    } else if (!validateEmail(email)) {
      emailError = 'Please enter a valid email';
      isValid = false;
    }
    
    if (!message.trim()) {
      messageError = 'Please enter your message';
      isValid = false;
    }
    
    if (isValid) {
      // In a real app, you would send the form data to a server
      console.log('Form submitted:', { name, email, message });
      formSubmitted = true;
      
      // Reset form
      name = '';
      email = '';
      message = '';
    }
  }
</script>

<div class="contact">
  <div class="section-header">
    <div class="section-title">
      <span class="section-number">03</span>
      <h2>CONTACT</h2>
    </div>
    <p class="section-description">
      Interested in working with us? We'd love to hear from you. Get in touch to discuss your project needs.
    </p>
  </div>
  
  <div class="contact-container">
    <div class="contact-info">
      <div class="info-block">
        <h3>VISIT US</h3>
        <p>123 Design Avenue<br>Zurich, Switzerland<br>CH-8001</p>
      </div>
      
      <div class="info-block">
        <h3>REACH US</h3>
        <p>hello@architekt.com<br>+41 88 123 45 67</p>
      </div>
      
      <div class="info-block">
        <h3>FOLLOW US</h3>
        <div class="social-links">
          <a href="/" class="social-link" aria-label="Instagram">
            <svg viewBox="0 0 24 24" width="24" height="24"><path fill="currentColor" d="M12 2.163c3.204 0 3.584.012 4.85.07 3.252.148 4.771 1.691 4.919 4.919.058 1.265.069 1.645.069 4.849 0 3.205-.012 3.584-.069 4.849-.149 3.225-1.664 4.771-4.919 4.919-1.266.058-1.644.07-4.85.07-3.204 0-3.584-.012-4.849-.07-3.26-.149-4.771-1.699-4.919-4.92-.058-1.265-.07-1.644-.07-4.849 0-3.204.013-3.583.07-4.849.149-3.227 1.664-4.771 4.919-4.919 1.266-.057 1.645-.069 4.849-.069zm0-2.163c-3.259 0-3.667.014-4.947.072-4.358.2-6.78 2.618-6.98 6.98-.059 1.281-.073 1.689-.073 4.948 0 3.259.014 3.668.072 4.948.2 4.358 2.618 6.78 6.98 6.98 1.281.058 1.689.072 4.948.072 3.259 0 3.668-.014 4.948-.072 4.354-.2 6.782-2.618 6.979-6.98.059-1.28.073-1.689.073-4.948 0-3.259-.014-3.667-.072-4.947-.196-4.354-2.617-6.78-6.979-6.98-1.281-.059-1.69-.073-4.949-.073zm0 5.838c-3.403 0-6.162 2.759-6.162 6.162s2.759 6.163 6.162 6.163 6.162-2.759 6.162-6.163c0-3.403-2.759-6.162-6.162-6.162zm0 10.162c-2.209 0-4-1.79-4-4 0-2.209 1.791-4 4-4s4 1.791 4 4c0 2.21-1.791 4-4 4zm6.406-11.845c-.796 0-1.441.645-1.441 1.44s.645 1.44 1.441 1.44c.795 0 1.439-.645 1.439-1.44s-.644-1.44-1.439-1.44z"/></svg>
          </a>
          <a href="/" class="social-link" aria-label="Twitter">
            <svg viewBox="0 0 24 24" width="24" height="24"><path fill="currentColor" d="M24 4.557c-.883.392-1.832.656-2.828.775 1.017-.609 1.798-1.574 2.165-2.724-.951.564-2.005.974-3.127 1.195-.897-.957-2.178-1.555-3.594-1.555-3.179 0-5.515 2.966-4.797 6.045-4.091-.205-7.719-2.165-10.148-5.144-1.29 2.213-.669 5.108 1.523 6.574-.806-.026-1.566-.247-2.229-.616-.054 2.281 1.581 4.415 3.949 4.89-.693.188-1.452.232-2.224.084.626 1.956 2.444 3.379 4.6 3.419-2.07 1.623-4.678 2.348-7.29 2.04 2.179 1.397 4.768 2.212 7.548 2.212 9.142 0 14.307-7.721 13.995-14.646.962-.695 1.797-1.562 2.457-2.549z"/></svg>
          </a>
          <a href="/" class="social-link" aria-label="LinkedIn">
            <svg viewBox="0 0 24 24" width="24" height="24"><path fill="currentColor" d="M4.98 3.5c0 1.381-1.11 2.5-2.48 2.5s-2.48-1.119-2.48-2.5c0-1.38 1.11-2.5 2.48-2.5s2.48 1.12 2.48 2.5zm.02 4.5h-5v16h5v-16zm7.982 0h-4.968v16h4.969v-8.399c0-4.67 6.029-5.052 6.029 0v8.399h4.988v-10.131c0-7.88-8.922-7.593-11.018-3.714v-2.155z"/></svg>
          </a>
        </div>
      </div>
    </div>
    
    <div class="contact-form">
      {#if formSubmitted}
        <div class="form-success">
          <h3>Thank You!</h3>
          <p>Your message has been sent successfully. We'll get back to you soon.</p>
          <button 
            class="reset-form-btn" 
            on:click={() => formSubmitted = false}
          >
            Send Another Message
          </button>
        </div>
      {:else}
        <form on:submit|preventDefault={handleSubmit}>
          <div class="form-group">
            <label for="name">NAME</label>
            <input 
              type="text" 
              id="name" 
              bind:value={name} 
              class:error={nameError}
            />
            {#if nameError}
              <div class="error-message">{nameError}</div>
            {/if}
          </div>
          
          <div class="form-group">
            <label for="email">EMAIL</label>
            <input 
              type="email" 
              id="email" 
              bind:value={email} 
              class:error={emailError}
            />
            {#if emailError}
              <div class="error-message">{emailError}</div>
            {/if}
          </div>
          
          <div class="form-group">
            <label for="message">MESSAGE</label>
            <textarea 
              id="message" 
              rows="5" 
              bind:value={message} 
              class:error={messageError}
            ></textarea>
            {#if messageError}
              <div class="error-message">{messageError}</div>
            {/if}
          </div>
          
          <button type="submit" class="submit-btn">SEND MESSAGE</button>
        </form>
      {/if}
    </div>
  </div>
</div>

<style>
  .contact {
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
  
  .contact-container {
    display: grid;
    grid-template-columns: 1fr;
    gap: calc(var(--grid-unit) * 8);
  }
  
  .contact-info {
    display: grid;
    grid-template-columns: 1fr;
    gap: calc(var(--grid-unit) * 6);
  }
  
  .info-block {
    display: flex;
    flex-direction: column;
    gap: calc(var(--grid-unit) * 2);
  }
  
  .info-block h3 {
    font-size: 1rem;
    font-weight: 500;
    letter-spacing: 1px;
    margin: 0;
    position: relative;
    padding-bottom: calc(var(--grid-unit) * 1.5);
  }
  
  .info-block h3::after {
    content: '';
    position: absolute;
    bottom: 0;
    left: 0;
    width: 20px;
    height: 2px;
    background-color: var(--color-accent);
  }
  
  .info-block p {
    font-size: 1rem;
    line-height: 1.6;
    margin: 0;
  }
  
  .social-links {
    display: flex;
    gap: calc(var(--grid-unit) * 2);
  }
  
  .social-link {
    width: 40px;
    height: 40px;
    display: flex;
    align-items: center;
    justify-content: center;
    border: 1px solid var(--color-border);
    color: var(--color-text);
    transition: all var(--transition-speed);
  }
  
  .social-link:hover {
    background-color: var(--color-text);
    color: var(--color-bg);
  }
  
  .contact-form {
    width: 100%;
  }
  
  .form-group {
    margin-bottom: calc(var(--grid-unit) * 4);
  }
  
  label {
    display: block;
    font-size: 0.75rem;
    letter-spacing: 1px;
    margin-bottom: calc(var(--grid-unit));
    font-weight: 500;
  }
  
  input, textarea {
    width: 100%;
    background-color: transparent;
    border: none;
    border-bottom: 1px solid var(--color-border);
    padding: calc(var(--grid-unit) * 1.5) 0;
    color: var(--color-text);
    font-family: var(--font-sans);
    font-size: 1rem;
    transition: all var(--transition-speed);
  }
  
  input:focus, textarea:focus {
    outline: none;
    border-color: var(--color-accent);
  }
  
  input.error, textarea.error {
    border-color: #e53e3e;
  }
  
  .error-message {
    color: #e53e3e;
    font-size: 0.75rem;
    margin-top: calc(var(--grid-unit));
  }
  
  .submit-btn {
    display: inline-block;
    background-color: var(--color-text);
    color: var(--color-bg);
    border: none;
    padding: calc(var(--grid-unit) * 2) calc(var(--grid-unit) * 4);
    font-size: 0.875rem;
    letter-spacing: 2px;
    cursor: pointer;
    transition: all var(--transition-speed);
  }
  
  .submit-btn:hover {
    background-color: var(--color-accent);
  }
  
  .form-success {
    text-align: center;
    padding: calc(var(--grid-unit) * 6) 0;
  }
  
  .form-success h3 {
    font-size: 1.5rem;
    margin: 0 0 calc(var(--grid-unit) * 2) 0;
    font-weight: 500;
  }
  
  .form-success p {
    margin-bottom: calc(var(--grid-unit) * 4);
    color: var(--color-secondary);
  }
  
  .reset-form-btn {
    display: inline-block;
    background-color: transparent;
    border: 1px solid var(--color-text);
    color: var(--color-text);
    padding: calc(var(--grid-unit) * 1.5) calc(var(--grid-unit) * 3);
    font-size: 0.875rem;
    letter-spacing: 1px;
    cursor: pointer;
    transition: all var(--transition-speed);
  }
  
  .reset-form-btn:hover {
    background-color: var(--color-text);
    color: var(--color-bg);
  }
  
  @media (min-width: 768px) {
    .section-header {
      grid-template-columns: 1fr 1fr;
      align-items: end;
    }
    
    .contact-container {
      grid-template-columns: 1fr 1fr;
    }
    
    .contact-info {
      grid-template-columns: repeat(3, 1fr);
    }
    
    @media (min-width: 1024px) {
      .contact-info {
        grid-column: 1 / -1;
        margin-bottom: calc(var(--grid-unit) * 6);
      }
      
      .contact-form {
        grid-column: 1 / -1;
      }
    }
  }
</style> 