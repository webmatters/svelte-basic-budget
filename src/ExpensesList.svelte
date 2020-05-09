<script>
  import { fly } from "svelte/transition";
  import { flip } from "svelte/animate";

  import StyledTitle from "./StyledTitle.svelte";
  import Expense from "./Expense.svelte";
  export let expenses = [];
</script>

<style>
  h2 {
    text-transform: capitalize;
  }
</style>

<section>
  <StyledTitle title="expenses list" />
  <ul>
    {#each expenses as expense, index (expense.id)}
      <div transition:fly={{ x: 200, delay: index * 500 }} animate:flip>
        <Expense {...expense} />
      </div>
    {:else}
      <h2>No expenses</h2>
    {/each}
  </ul>
</section>
