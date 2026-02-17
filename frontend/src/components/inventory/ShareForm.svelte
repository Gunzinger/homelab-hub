<script>
  import { createEventDispatcher } from 'svelte';
  
  export let share = null;
  export let storageId = null;
  export let defaultIp = '';
  export let defaultHostname = '';
  
  const dispatch = createEventDispatcher();
  
  let formData = {
    id: share?.id,
    storage_id: share?.storage_id || storageId,
    name: share?.name || '',
    hostname: share?.hostname || (share ? '' : defaultHostname),
    ip: share?.ip || (share ? '' : defaultIp),
    share_type: share?.share_type || 'NFS',
    notes: share?.notes || ''
  };
  
  const shareTypes = ['NFS', 'SMB', 'iSCSI', 'FTP', 'SFTP', 'WebDAV', 'Other'];
  
  function handleSubmit() {
    dispatch('submit', formData);
  }
  
  function handleCancel() {
    dispatch('cancel');
  }
</script>

<form on:submit|preventDefault={handleSubmit}>
  <div class="form-group">
    <label for="name">Name *</label>
    <input 
      type="text" 
      id="name" 
      bind:value={formData.name} 
      required 
      placeholder="e.g., Media Share"
    />
  </div>
  
  <div class="form-group">
    <label for="share_type">Share Type *</label>
    <select id="share_type" bind:value={formData.share_type} required>
      {#each shareTypes as type}
        <option value={type}>{type}</option>
      {/each}
    </select>
  </div>
  
  <div class="form-group">
    <label for="hostname">Hostname</label>
    <input 
      type="text" 
      id="hostname" 
      bind:value={formData.hostname} 
      placeholder="e.g., storage01"
    />
  </div>
  
  <div class="form-group">
    <label for="ip">IP Address</label>
    <input 
      type="text" 
      id="ip" 
      bind:value={formData.ip} 
      placeholder="e.g., 192.168.1.10"
    />
  </div>
  
  <div class="form-group">
    <label for="notes">Notes</label>
    <textarea 
      id="notes" 
      bind:value={formData.notes} 
      rows="3"
      placeholder="Additional information about this share..."
    ></textarea>
  </div>
  
  <div class="form-actions">
    <button type="submit" class="btn-primary">{share ? 'Update' : 'Add'} Share</button>
    <button type="button" class="btn-secondary" on:click={handleCancel}>Cancel</button>
  </div>
</form>

<style>
  form {
    display: flex;
    flex-direction: column;
  }
  
  .form-group {
    margin-bottom: 1rem;
  }
  
  label {
    display: block;
    margin-bottom: 0.25rem;
    color: #ccc;
    font-size: 0.9rem;
  }
  
  input, select, textarea {
    width: 100%;
    padding: 0.5rem;
    background: #1a1a1a;
    border: 1px solid #444;
    border-radius: 4px;
    color: #fff;
    font-family: inherit;
  }
  
  input:focus, select:focus, textarea:focus {
    outline: none;
    border-color: #4a9eff;
  }
  
  .form-actions {
    display: flex;
    gap: 0.5rem;
    margin-top: 1.5rem;
  }
  
  button {
    padding: 0.5rem 1rem;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    font-size: 0.9rem;
  }
  
  .btn-primary {
    background: #4a9eff;
    color: white;
  }
  
  .btn-primary:hover {
    background: #3a8eef;
  }
  
  .btn-secondary {
    background: #444;
    color: white;
  }
  
  .btn-secondary:hover {
    background: #555;
  }
</style>
