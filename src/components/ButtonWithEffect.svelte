<script lang="ts">
  /**
   * Button component with hover effects
   */
  import { onMount, onDestroy } from 'svelte';
  import { animate } from 'motion';
  
  // Props
  export let text = '';
  export let type: 'button' | 'submit' | 'reset' = 'button';
  export let className = '';
  export let disabled = false;
  export let primary = false;
  export let secondary = false;
  export let outlined = false;
  export let small = false;
  export let large = false;
  export let fullWidth = false;
  export let icon = '';
  export let iconPosition = 'left';
  export let hoverColor = '';
  export let normalColor = '';
  
  // Element reference for animations
  let buttonRef;
  let isHovered = false;
  
  // Handle mouse events
  function handleMouseEnter() {
    isHovered = true;
    
    if (buttonRef) {
      try {
        // @ts-ignore - Motion types are not fully compatible with TS
        animate(buttonRef, { 
          scale: [1, 1.05]
        }, { 
          duration: 0.2,
          ease: [0.22, 0.03, 0.26, 1]
        });
      } catch (error) {
        console.error('Animation error:', error);
      }
    }
  }
  
  function handleMouseLeave() {
    isHovered = false;
    
    if (buttonRef) {
      try {
        // @ts-ignore - Motion types are not fully compatible with TS
        animate(buttonRef, { 
          scale: [1.05, 1]
        }, { 
          duration: 0.2,
          ease: [0.22, 0.03, 0.26, 1]
        });
      } catch (error) {
        console.error('Animation error:', error);
      }
    }
  }
  
  // Determine button variant class
  let buttonVariant = '';
  $: {
    if (primary) buttonVariant = 'primary';
    else if (secondary) buttonVariant = 'secondary';
    else if (outlined) buttonVariant = 'outlined';
    else buttonVariant = '';
  }
  
  // Determine button size class
  let buttonSize = '';
  $: {
    if (small) buttonSize = 'small';
    else if (large) buttonSize = 'large';
    else buttonSize = '';
  }
</script>

<button 
  {type}
  class="button {buttonVariant} {buttonSize} {className}"
  class:full-width={fullWidth}
  class:is-hovered={isHovered}
  {disabled}
  on:click
  on:mouseenter={handleMouseEnter}
  on:mouseleave={handleMouseLeave}
  bind:this={buttonRef}
  style={hoverColor && isHovered ? `background-color: ${hoverColor};` : 
         normalColor && !isHovered ? `background-color: ${normalColor};` : ''}
>
  {#if icon && iconPosition === 'left'}
    <span class="icon left-icon">{icon}</span>
  {/if}
  
  <span class="button-text">{text}</span>
  
  {#if icon && iconPosition === 'right'}
    <span class="icon right-icon">{icon}</span>
  {/if}
  
  <slot></slot>
</button>

<style>
  .button {
    position: relative;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    padding: 0.75rem 1.5rem;
    border-radius: 4px;
    font-weight: 500;
    text-align: center;
    cursor: pointer;
    transition: transform 0.2s, background-color 0.2s, color 0.2s, border-color 0.2s;
    will-change: transform, background-color;
    border: none;
    outline: none;
    background-color: var(--color-accent, #ff3e00);
    color: white;
    font-family: var(--font-sans, sans-serif);
    font-size: 1rem;
    line-height: 1.5;
    gap: 0.5rem;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    width: fit-content;
  }
  
  .button:focus {
    box-shadow: 0 0 0 2px rgba(255, 62, 0, 0.4);
  }
  
  .button:disabled {
    opacity: 0.6;
    cursor: not-allowed;
  }
  
  /* Button variants */
  .button.primary {
    background-color: var(--color-accent, #ff3e00);
    color: white;
  }
  
  .button.secondary {
    background-color: var(--color-secondary, #676778);
    color: white;
  }
  
  .button.outlined {
    background-color: transparent;
    border: 2px solid var(--color-accent, #ff3e00);
    color: var(--color-accent, #ff3e00);
  }
  
  /* Button sizes */
  .button.small {
    padding: 0.5rem 1rem;
    font-size: 0.875rem;
  }
  
  .button.large {
    padding: 1rem 2rem;
    font-size: 1.125rem;
  }
  
  .button.full-width {
    width: 100%;
  }
  
  /* Icon styling */
  .icon {
    display: inline-flex;
    align-items: center;
    justify-content: center;
  }
  
  .left-icon {
    margin-right: 0.5rem;
  }
  
  .right-icon {
    margin-left: 0.5rem;
  }
  
  /* Hover states are handled via the motion.dev animations in script */
  
  /* Respect user preferences for reduced motion */
  @media (prefers-reduced-motion: reduce) {
    .button {
      transition: none !important;
      transform: none !important;
    }
  }
</style> 