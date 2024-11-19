<script>
  export let placeholder = "Search courses...";
  export let onSearch;

  let searchTerm = "";

  function handleInput(event) {
    searchTerm = event.target.value;
    onSearch(searchTerm);
  }

  function clearSearch() {
    searchTerm = "";
    onSearch("");
  }
</script>

<div class="search-container">
  <span class="material-icons-round search-icon">search</span>
  <input
    type="text"
    class="search-input"
    {placeholder}
    bind:value={searchTerm}
    on:input={handleInput}
  />
  {#if searchTerm}
    <button class="clear-button" on:click={clearSearch}>
      <span class="material-icons-round">close</span>
    </button>
  {/if}
</div>

<style>
  .search-container {
    position: relative;
    margin-bottom: 16px;
  }

  .search-input {
    width: 100%;
    padding: 10px 36px;
    background: #252525;
    border: 1px solid #333;
    border-radius: 8px;
    color: #fff;
    font-size: 14px;
    transition: all 0.2s ease;
  }

  .search-input:focus {
    outline: none;
    border-color: #4CAF50;
    background: #2a2a2a;
  }

  .search-icon {
    position: absolute;
    left: 12px;
    top: 50%;
    transform: translateY(-50%);
    color: #888;
    font-size: 20px;
  }

  .clear-button {
    position: absolute;
    right: 8px;
    top: 50%;
    transform: translateY(-50%);
    background: none;
    border: none;
    padding: 4px;
    cursor: pointer;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    color: #888;
  }

  .clear-button:hover {
    background: #333;
  }

  .clear-button .material-icons-round {
    font-size: 20px;
  }
</style> 