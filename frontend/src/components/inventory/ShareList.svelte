<script>
  import { createEventDispatcher } from 'svelte';
  import Modal from '../Modal.svelte';
  import ShareForm from './ShareForm.svelte';
  
  export let shares = [];
  export let storageId;
  export let parentIp = '';
  export let parentHostname = '';
  
  const dispatch = createEventDispatcher();
  
  let isModalOpen = false;
  let editingShare = null;
  
  function handleAddShare() {
    editingShare = null;
    isModalOpen = true;
  }
  
  function handleEditShare(share) {
    editingShare = share;
    isModalOpen = true;
  }
  
  function handleFormSubmit(event) {
    dispatch('save', event.detail);
    isModalOpen = false;
    editingShare = null;
  }
  
  function handleModalClose() {
    isModalOpen = false;
    editingShare = null;
  }
  
  function handleDeleteShare(share) {
    if (confirm(`Are you sure you want to delete the share "${share.name}"?`)) {
      dispatch('delete', share);
    }
  }
</script>

<div class="shares-section">
  <div class="shares-header">
    <h3>Shares</h3>
    <button class="btn-add" on:click={handleAddShare}>+ Add Share</button>
  </div>
  
  {#if shares.length > 0}
    <div class="shares-list">
      {#each shares as share}
        <div class="share-item">
          <div class="share-info">
            <div class="share-name">{share.name}</div>
            <div class="share-details">
              <span class="share-type">{share.share_type}</span>
              {#if share.hostname}
                <span class="share-detail">Host: {share.hostname}</span>
              {/if}
              {#if share.ip}
                <span class="share-detail">IP: {share.ip}</span>
              {/if}
            </div>
            {#if share.notes}
              <div class="share-notes">{share.notes}</div>
            {/if}
          </div>
          <div class="share-actions">
            <button class="btn-icon" on:click={() => handleEditShare(share)} title="Edit">
              ✏️
            </button>
            <button class="btn-icon btn-delete" on:click={() => handleDeleteShare(share)} title="Delete">
              🗑️
            </button>
          </div>
        </div>
      {/each}
    </div>
  {:else}
    <div class="no-shares">
      No shares configured. Click "Add Share" to create one.
    </div>
  {/if}
</div>

<Modal 
  isOpen={isModalOpen} 
  title={editingShare ? 'Edit Share' : 'Add Share'}
  on:close={handleModalClose}
>
  <ShareForm 
    share={editingShare} 
    storageId={storageId}
    defaultIp={parentIp}
    defaultHostname={parentHostname}
    on:submit={handleFormSubmit}
    on:cancel={handleModalClose}
  />
</Modal>

<style>
  .shares-section {
    margin-top: 2rem;
    padding-top: 1.5rem;
    border-top: 1px solid #444;
  }
  
  .shares-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 1rem;
  }
  
  .shares-header h3 {
    margin: 0;
    color: #fff;
  }
  
  .btn-add {
    padding: 0.5rem 1rem;
    background: #4a9eff;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    font-size: 0.9rem;
  }
  
  .btn-add:hover {
    background: #3a8eef;
  }
  
  .shares-list {
    display: flex;
    flex-direction: column;
    gap: 0.75rem;
  }
  
  .share-item {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    padding: 0.75rem;
    background: #2a2a2a;
    border-radius: 4px;
    border: 1px solid #444;
  }
  
  .share-info {
    flex: 1;
  }
  
  .share-name {
    font-weight: 500;
    color: #fff;
    margin-bottom: 0.25rem;
  }
  
  .share-details {
    display: flex;
    gap: 0.75rem;
    flex-wrap: wrap;
    font-size: 0.85rem;
    color: #999;
    margin-bottom: 0.25rem;
  }
  
  .share-type {
    background: #4a9eff;
    color: white;
    padding: 0.15rem 0.5rem;
    border-radius: 3px;
    font-size: 0.75rem;
    font-weight: 500;
  }
  
  .share-detail {
    color: #ccc;
  }
  
  .share-notes {
    font-size: 0.85rem;
    color: #999;
    margin-top: 0.5rem;
    font-style: italic;
  }
  
  .share-actions {
    display: flex;
    gap: 0.25rem;
  }
  
  .btn-icon {
    padding: 0.25rem 0.5rem;
    background: transparent;
    border: none;
    cursor: pointer;
    font-size: 1rem;
    opacity: 0.7;
  }
  
  .btn-icon:hover {
    opacity: 1;
  }
  
  .btn-delete {
    background: rgba(220, 53, 69, 0.15);
    border-radius: 3px;
  }
  
  .btn-delete:hover {
    background: rgba(220, 53, 69, 0.25);
  }
  
  .no-shares {
    padding: 2rem;
    text-align: center;
    color: #999;
    font-style: italic;
  }
</style>
