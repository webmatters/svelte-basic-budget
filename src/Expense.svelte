<script>
  import { getContext } from "svelte";
  import { slide } from "svelte/transition";
  import { quintOut } from "svelte/easing";

  export let id;
  export let name = "";
  export let amount = 0;
  let displayAmount = false;

  function toggleAmount() {
    displayAmount = !displayAmount;
  }

  const { removeExpense, setModifiedExpense } = getContext("state");
</script>

<style>

</style>

<article class="single-expense">
  <div class="expense-info">
    <h2>
      {name}
      <button on:click={toggleAmount} class="amount-btn">
        <i class="fas fa-caret-down" />
      </button>
    </h2>
    {#if displayAmount}
      <h4 transition:slide>amount : ${amount}</h4>
    {/if}
  </div>
  <div class="expense-buttons">
    <button class="expense-btn edit-btn" on:click={setModifiedExpense(id)}>
      <i class="fas fa-pen" />
    </button>
    <button class="expense-btn delete-btn" on:click={removeExpense(id)}>
      <i class="fas fa-trash" />
    </button>
  </div>
</article>
