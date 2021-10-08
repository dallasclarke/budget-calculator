<script>
  import { fly } from "svelte/transition";

  import SectionTitle from "./Title.svelte";
  import Expense from "./Expense.svelte";

  export let expenses = [];
</script>

<section>
  <SectionTitle title="Expense List" />
  <ul>
    {#each expenses as expense, index}
      <div transition:fly={{ x: 200, delay: (index + 1) * 700 }}>
        <Expense {...expense} />
      </div>
    {:else}
      <h2>Currently there are no expenses!</h2>
    {/each}
  </ul>
</section>
