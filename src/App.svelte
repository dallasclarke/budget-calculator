<!-- JS Logic -->
<script>
  import { setContext } from "svelte";
 
  import Navbar from "./Navbar.svelte";
  import ExpenseList from "./ExpenseList.svelte";

  import expensesData from "./expenses";

  let expenses = [...expensesData];

  function removeExpense(id) {
    expenses = expenses.filter((item) => item.id !== id);
  }

  // Context
  setContext("remove", removeExpense);
</script>

<!-- CSS Logic -->

<!-- Components -->
<Navbar />
<main class="content">
  <ExpenseList {expenses} />
</main>
