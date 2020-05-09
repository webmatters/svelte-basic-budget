<script>
  export let showForm;
</script>

<nav class="nav">
  <div class="nav-center">
    <h1 class="nav-title">budget calculator</h1>
    <button type="button" on:click={showForm} class="nav-btn">
      <i class="far fa-plus-square" />
      add item
    </button>
  </div>
</nav>
