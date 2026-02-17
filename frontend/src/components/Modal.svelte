<script>
  import { createEventDispatcher } from 'svelte';
  
  export let isOpen = false;
  export let title = '';
  export let maxWidth = '600px';
  
  const dispatch = createEventDispatcher();
  
  function handleClose() {
    dispatch('close');
  }
  
  function handleBackdropClick(event) {
    if (event.target === event.currentTarget) {
      handleClose();
    }
  }
  
  function handleKeydown(event) {
    if (event.key === 'Escape' && isOpen) {
      handleClose();
    }
  }
</script>

<svelte:window on:keydown={handleKeydown} />

{#if isOpen}
  <!-- svelte-ignore a11y-click-events-have-key-events -->
  <!-- svelte-ignore a11y-no-static-element-interactions -->
  <div class="modal-backdrop" on:click={handleBackdropClick}>
    <div class="modal-container" style="max-width: {maxWidth}">
      <div class="modal-header">
        {#if title}
          <h2>{title}</h2>
        {/if}
        <button class="close-btn" on:click={handleClose} aria-label="Close">×</button>
      </div>
      <div class="modal-body">
        <slot />
      </div>
    </div>
  </div>
{/if}

<style>
  .modal-backdrop {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(0, 0, 0, 0.75);
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 9999;
    padding: 1rem;
    overflow-y: auto;
  }
  
  .modal-container {
    background: var(--pico-background-color, #1a1a1a);
    border-radius: 8px;
    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.5);
    width: 100%;
    max-height: 90vh;
    display: flex;
    flex-direction: column;
    margin: auto;
  }
  
  .modal-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 1.25rem 1.5rem;
    border-bottom: 1px solid #444;
  }
  
  .modal-header h2 {
    margin: 0;
    color: #fff;
    font-size: 1.25rem;
  }
  
  .close-btn {
    background: transparent;
    border: none;
    color: #999;
    font-size: 2rem;
    cursor: pointer;
    padding: 0;
    width: 32px;
    height: 32px;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 4px;
    line-height: 1;
    transition: all 0.2s;
  }
  
  .close-btn:hover {
    background: rgba(255, 255, 255, 0.1);
    color: #fff;
  }
  
  .modal-body {
    padding: 1.5rem;
    overflow-y: auto;
    flex: 1;
  }
  
  @media (max-width: 768px) {
    .modal-container {
      max-height: 100vh;
      border-radius: 0;
    }
    
    .modal-backdrop {
      padding: 0;
    }
  }
</style>
