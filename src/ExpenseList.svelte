<script>
  import SectionTitle from "./Title.svelte";
  import Expense from "./Expense.svelte";

  export let expenses = [];
  
</script>

<section>
  <SectionTitle title="Expense List" />
  <ul>
    {#each expenses as expense}
      <Expense {...expense} />
    {:else}
      <h2>Currently there are no expenses!</h2>
    {/each}
  </ul>
</section>
