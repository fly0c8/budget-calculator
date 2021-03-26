<script>
  import SectionTitle from "./Title.svelte";
  import Expense from "./Expense.svelte";
  import { fly } from 'svelte/transition';
  import { flip } from 'svelte/animate';
  export let expenses = [];
</script>

<section>
  <SectionTitle title="expenses list" />
  <ul>
    {#each expenses as expense, index(expense.id)}
    <div transition:fly={{x:200, delay:index*1000 }} animate:flip >
      <Expense {...expense} />
    </div>
    {:else}
      <h2>Currently you have no expenses</h2>
    {/each}
  </ul>
</section>

<style>
  h2 {
    text-transform: capitalize;
  }
</style>
