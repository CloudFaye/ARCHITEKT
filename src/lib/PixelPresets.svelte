<script>
  // Props
  let onApplyPreset = $props();
  onApplyPreset = onApplyPreset ?? (() => {});

  // Preset configurations
  const presets = $state([
    {
      name: "Standard Pixels",
      icon: "🔳",
      config: { pixelSize: 10, opacity: 0.8, dotSpacing: 2, dotColor: "#000000" }
    },
    {
      name: "Fine Detail",
      icon: "🔍",
      config: { pixelSize: 5, opacity: 0.7, dotSpacing: 1, dotColor: "#222222" }
    },
    {
      name: "Bold Blocks",
      icon: "🧱",
      config: { pixelSize: 20, opacity: 0.9, dotSpacing: 0, dotColor: "#000000" }
    },
    {
      name: "Retro Gaming",
      icon: "🎮",
      config: { pixelSize: 15, opacity: 1, dotSpacing: 1, dotColor: "#333333" }
    },
    {
      name: "Abstract Art",
      icon: "🎨",
      config: { pixelSize: 30, opacity: 0.6, dotSpacing: 5, dotColor: "#3b82f6" }
    },
    {
      name: "Neon Glow",
      icon: "✨",
      config: { pixelSize: 12, opacity: 0.7, dotSpacing: 3, dotColor: "#8b5cf6" }
    },
  ]);

  function handlePresetClick(preset) {
    onApplyPreset(preset.config);
  }
</script>

<div class="presets-container">
  <h3>Quick Effects</h3>
  <div class="presets-grid">
    {#each presets as preset}
      <button 
        class="preset-button" 
        onclick={() => handlePresetClick(preset)}
        title={preset.name}
      >
        <span class="preset-icon">{preset.icon}</span>
        <span class="preset-name">{preset.name}</span>
      </button>
    {/each}
  </div>
</div>

<style>
  .presets-container {
    background-color: #f3f4f6;
    padding: 1rem;
    border-radius: 0.5rem;
    margin-bottom: 1rem;
  }

  h3 {
    font-size: 1rem;
    margin-bottom: 0.75rem;
    color: #4b5563;
  }

  .presets-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
    gap: 0.75rem;
  }

  .preset-button {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    background-color: white;
    border: 1px solid #e5e7eb;
    padding: 0.75rem;
    border-radius: 0.375rem;
    transition: all 0.2s;
    cursor: pointer;
  }

  .preset-button:hover {
    transform: translateY(-2px);
    box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
    border-color: #d1d5db;
  }

  .preset-icon {
    font-size: 1.5rem;
    margin-bottom: 0.5rem;
  }

  .preset-name {
    font-size: 0.875rem;
    font-weight: 500;
    color: #4b5563;
  }
</style> 