<!-- JS Logic -->
<script>
  import { setContext } from "svelte";

  import Navbar from "./Navbar.svelte";
  import ExpenseList from "./ExpenseList.svelte";
  import Totals from "./Totals.svelte";
  import ExpenseForm from "./ExpenseForm.svelte";

  import expensesData from "./expenses";

  let expenses = [...expensesData];

  $: total = expenses.reduce((acc, curr) => {
    return acc += curr.amount;
  }, 0)

  function removeExpense(id) {
    expenses = expenses.filter((item) => item.id !== id);
  }

  function clearExpenses() {
    expenses = [];
  }

  // Context
  setContext("remove", removeExpense);
</script>

<!-- CSS Logic -->

<!-- Components -->
<Navbar />
<main class="content">
  <ExpenseForm />
  <Totals title="Total Expenses" {total} />
  <ExpenseList {expenses} />
  <button class="btn btn-primary btn-block" on:click={clearExpenses}>
    Clear Expenses
  </button>
</main>
