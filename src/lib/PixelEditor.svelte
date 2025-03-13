<script>
  import { onMount, createEventDispatcher } from 'svelte';
  import PixelPresets from './PixelPresets.svelte';

  // Props
  let {imageSrc, pixelSize, opacity, dotSpacing, dotColor} = $props();

  // Default values
  imageSrc = imageSrc ?? 'https://images.unsplash.com/photo-1606041008023-472dfb5e530f?ixlib=rb-4.0.3&auto=format&fit=crop&w=800&q=80';
  pixelSize = pixelSize ?? 10;
  opacity = opacity ?? 0.8;
  dotSpacing = dotSpacing ?? 2;
  dotColor = dotColor ?? '#000000';

  // Internal state
  let canvas = $state(null);
  let ctx = $state(null);
  let image = $state(new Image());
  let originalImageData = $state(null);
  let isImageLoaded = $state(false);
  let canvasWidth = $state(0);
  let canvasHeight = $state(0);
  let isLoading = $state(false);
  let imageUrl = $state('');
  let uploadProgress = $state(0);
  let isUploading = $state(false);
  let uploadError = $state('');
  
  const dispatch = createEventDispatcher();
  const MAX_FILE_SIZE = 10 * 1024 * 1024; // 10MB max

  // Load image on mount
  onMount(() => {
    if (imageSrc) {
      loadImage(imageSrc);
    }
  });
  
  // Load image when source changes
  $effect(() => {
    if (imageSrc) {
      loadImage(imageSrc);
    }
  });

  function loadImage(src) {
    isLoading = true;
    uploadError = '';
    image = new Image();
    image.crossOrigin = "Anonymous"; // To avoid CORS issues with external images
    
    image.onload = () => {
      isImageLoaded = true;
      isLoading = false;
      isUploading = false;
      uploadProgress = 0;
      setupCanvas();
      applyPixelEffect();
    };
    
    image.onerror = () => {
      isLoading = false;
      isUploading = false;
      uploadProgress = 0;
      uploadError = "Error loading image. Please try another one.";
      console.error("Error loading image");
    };
    
    image.src = src;
  }

  function setupCanvas() {
    if (!canvas) return;
    
    // Make canvas match image dimensions
    canvasWidth = image.width;
    canvasHeight = image.height;
    
    // Get 2D context
    ctx = canvas.getContext('2d');
    
    // Draw original image to canvas
    ctx.drawImage(image, 0, 0, canvasWidth, canvasHeight);
    
    // Store original image data
    originalImageData = ctx.getImageData(0, 0, canvasWidth, canvasHeight);
  }

  function applyPixelEffect() {
    if (!ctx || !isImageLoaded) return;
    
    // Clear canvas
    ctx.clearRect(0, 0, canvasWidth, canvasHeight);
    
    // Draw original image as background
    ctx.drawImage(image, 0, 0, canvasWidth, canvasHeight);
    
    // Create dotted pixel effect
    for (let y = 0; y < canvasHeight; y += pixelSize) {
      for (let x = 0; x < canvasWidth; x += pixelSize) {
        // Get the color at this pixel block's center
        const centerX = Math.min(x + Math.floor(pixelSize / 2), canvasWidth - 1);
        const centerY = Math.min(y + Math.floor(pixelSize / 2), canvasHeight - 1);
        
        const pixel = ctx.getImageData(centerX, centerY, 1, 1).data;
        
        // Draw a dot using the sampled color
        const dotSize = pixelSize - dotSpacing;
        
        ctx.fillStyle = `rgba(${pixel[0]}, ${pixel[1]}, ${pixel[2]}, ${opacity})`;
        ctx.beginPath();
        ctx.rect(x + dotSpacing/2, y + dotSpacing/2, dotSize, dotSize);
        ctx.fill();
      }
    }
    
    // Overlay the dots with the selected dot color
    ctx.fillStyle = `${dotColor}20`; // 20 is hex for 12.5% opacity
    ctx.fillRect(0, 0, canvasWidth, canvasHeight);
    
    // Notify parent component that the image has been processed
    dispatch('processed', { canvas });
  }

  // Apply effects when parameters change
  $effect(() => {
    if (isImageLoaded) {
      applyPixelEffect();
    }
  });

  // Handle file upload
  function handleFileUpload(event) {
    const file = event.target.files[0];
    if (!file) return;
    
    // Reset
    uploadError = '';
    
    // Check file size
    if (file.size > MAX_FILE_SIZE) {
      uploadError = `File is too large. Maximum size is ${MAX_FILE_SIZE / (1024 * 1024)}MB.`;
      return;
    }
    
    // Check file type
    if (!file.type.startsWith('image/')) {
      uploadError = 'Only image files are accepted.';
      return;
    }
    
    isUploading = true;
    uploadProgress = 0;
    
    const reader = new FileReader();
    
    // Progress tracking
    reader.onprogress = (event) => {
      if (event.lengthComputable) {
        uploadProgress = Math.round((event.loaded / event.total) * 100);
      }
    };
    
    reader.onload = (e) => {
      uploadProgress = 100;
      // Small delay to show 100% progress
      setTimeout(() => {
        loadImage(e.target.result);
      }, 300);
    };
    
    reader.onerror = () => {
      isUploading = false;
      uploadProgress = 0;
      uploadError = 'Error reading file. Please try again.';
    };
    
    reader.readAsDataURL(file);
  }

  // Handle image URL input
  function handleUrlSubmit() {
    if (imageUrl.trim()) {
      uploadError = '';
      loadImage(imageUrl);
    }
  }

  // Export edited image
  function exportImage() {
    if (!canvas) return;
    
    const link = document.createElement('a');
    link.download = 'pixelated-image.png';
    link.href = canvas.toDataURL('image/png');
    link.click();
  }

  // Reset to original
  function resetToOriginal() {
    pixelSize = 10;
    opacity = 0.8;
    dotSpacing = 2;
    dotColor = '#000000';
  }
  
  // Handle preset application
  function handleApplyPreset(config) {
    pixelSize = config.pixelSize;
    opacity = config.opacity;
    dotSpacing = config.dotSpacing;
    dotColor = config.dotColor;
  }
</script>

<div class="pixel-editor">
  <!-- Presets section -->
  <PixelPresets onApplyPreset={handleApplyPreset} />
  
  <!-- Controls section -->
  <div class="controls">
    <div class="control-group">
      <label for="pixelSize">Pixel Size: {pixelSize}px</label>
      <input 
        id="pixelSize" 
        type="range" 
        min="5" 
        max="50" 
        bind:value={pixelSize}
      />
    </div>
    
    <div class="control-group">
      <label for="opacity">Dot Opacity: {opacity.toFixed(2)}</label>
      <input 
        id="opacity" 
        type="range" 
        min="0" 
        max="1" 
        step="0.05" 
        bind:value={opacity}
      />
    </div>
    
    <div class="control-group">
      <label for="dotSpacing">Dot Spacing: {dotSpacing}px</label>
      <input 
        id="dotSpacing" 
        type="range" 
        min="0" 
        max="10" 
        bind:value={dotSpacing}
      />
    </div>
    
    <div class="control-group">
      <label for="dotColor">Dot Color:</label>
      <input 
        id="dotColor" 
        type="color" 
        bind:value={dotColor}
      />
    </div>
  </div>
  
  <!-- Image source controls -->
  <div class="image-source-controls">
    <div class="url-input">
      <input 
        type="text" 
        placeholder="Enter image URL" 
        bind:value={imageUrl}
      />
      <button onclick={handleUrlSubmit}>Load URL</button>
    </div>
    
    <div class="button-group">
      <label class="upload-btn">
        Upload Image
        <input type="file" accept="image/*" onchange={handleFileUpload} />
      </label>
      
      <button onclick={resetToOriginal}>
        Reset Settings
      </button>
      
      <button onclick={exportImage} disabled={!isImageLoaded} class="export-btn">
        Export Image
      </button>
    </div>
  </div>
  
  <!-- Error message -->
  {#if uploadError}
    <div class="error-message">
      <p>{uploadError}</p>
    </div>
  {/if}
  
  <!-- Upload progress -->
  {#if isUploading}
    <div class="progress-container">
      <div class="progress-bar" style="width: {uploadProgress}%"></div>
      <span>{uploadProgress}%</span>
    </div>
  {/if}
  
  <!-- Canvas preview -->
  <div class="canvas-container">
    {#if isLoading}
      <div class="loading">
        <p>Loading image...</p>
      </div>
    {:else if !isImageLoaded}
      <div class="placeholder">
        <p>Upload an image or provide an image URL to begin editing</p>
      </div>
    {:else}
      <canvas 
        bind:this={canvas} 
        width={canvasWidth} 
        height={canvasHeight}
      ></canvas>
    {/if}
  </div>
</div>

<style>
  .pixel-editor {
    display: flex;
    flex-direction: column;
    gap: 1rem;
    width: 100%;
    max-width: 1200px;
    margin: 0 auto;
  }
  
  .controls {
    display: flex;
    flex-wrap: wrap;
    gap: 1rem;
    background-color: #f3f4f6;
    padding: 1rem;
    border-radius: 0.5rem;
  }
  
  .control-group {
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
    min-width: 200px;
    flex: 1;
  }
  
  label {
    font-weight: 500;
    font-size: 0.875rem;
  }
  
  input[type="range"] {
    width: 100%;
  }
  
  input[type="file"] {
    display: none;
  }
  
  .image-source-controls {
    display: flex;
    flex-wrap: wrap;
    gap: 1rem;
    align-items: center;
    background-color: #f3f4f6;
    padding: 1rem;
    border-radius: 0.5rem;
  }
  
  .url-input {
    display: flex;
    gap: 0.5rem;
    flex: 1;
  }
  
  .url-input input {
    flex: 1;
    padding: 0.5rem;
    border: 1px solid #d1d5db;
    border-radius: 0.25rem;
  }
  
  .button-group {
    display: flex;
    gap: 0.5rem;
    flex-wrap: wrap;
  }
  
  button, .upload-btn {
    background-color: #3b82f6;
    color: white;
    padding: 0.5rem 1rem;
    border: none;
    border-radius: 0.25rem;
    font-weight: 500;
    cursor: pointer;
    transition: background-color 0.2s;
    text-align: center;
    white-space: nowrap;
  }
  
  button:hover, .upload-btn:hover {
    background-color: #2563eb;
  }
  
  button:disabled {
    background-color: #9ca3af;
    cursor: not-allowed;
  }
  
  .export-btn {
    background-color: #10b981;
  }
  
  .export-btn:hover {
    background-color: #059669;
  }
  
  .error-message {
    background-color: #fee2e2;
    color: #ef4444;
    padding: 0.75rem;
    border-radius: 0.5rem;
    font-size: 0.875rem;
    border-left: 4px solid #ef4444;
  }
  
  .progress-container {
    height: 1.5rem;
    background-color: #e5e7eb;
    border-radius: 0.25rem;
    overflow: hidden;
    position: relative;
  }
  
  .progress-bar {
    height: 100%;
    background-color: #3b82f6;
    transition: width 0.3s ease;
  }
  
  .progress-container span {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    display: flex;
    align-items: center;
    justify-content: center;
    color: white;
    font-size: 0.75rem;
    font-weight: 600;
    text-shadow: 0 1px 2px rgba(0, 0, 0, 0.2);
  }
  
  .canvas-container {
    background-color: #e5e7eb;
    border-radius: 0.5rem;
    padding: 1rem;
    display: flex;
    justify-content: center;
    min-height: 400px;
  }
  
  canvas {
    max-width: 100%;
    max-height: 70vh;
    object-fit: contain;
    box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
  }
  
  .placeholder, .loading {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 300px;
    width: 100%;
    background-color: #f9fafb;
    border: 2px dashed #d1d5db;
    border-radius: 0.25rem;
    color: #6b7280;
  }
  
  .loading {
    background-color: #f3f4f6;
    border: none;
  }
</style> 