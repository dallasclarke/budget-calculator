<!-- JS Logic -->
<script>
  import { setContext } from "svelte";

  import Navbar from "./Navbar.svelte";
  import ExpenseList from "./ExpenseList.svelte";
  import Totals from "./Totals.svelte";
  import ExpenseForm from "./ExpenseForm.svelte";

  import expensesData from "./expenses";

  let expenses = [...expensesData];
  let setName = "";
  let setAmount = null;
  let setId = null;

  $: isEditing = setId ? true : false;
  $: total = expenses.reduce((acc, curr) => {
    return (acc += curr.amount);
  }, 0);

  function removeExpense(id) {
    expenses = expenses.filter((item) => item.id !== id);
  }

  function clearExpenses() {
    expenses = [];
  }

  function addExpense({ name, amount }) {
    let expense = {
      id: Math.random() * Date.now(),
      name,
      amount,
    };

    expenses = [expense, ...expenses];
  }

  function setMotifiedExpense(id) {
    let expense = expenses.find((item) => item.id === id);

    setId = expense.id;
    setName = expense.name;
    setAmount = expense.amount;
  }

  function editExpense({ name, amount }) {
    expenses = expenses.map((item) => {
      return item.id === setId ? { ...item, name, amount } : { ...item };
    });

    setId = null;
    setAmount = null;
    setName = "";
  }

  // Context
  setContext("remove", removeExpense);
  setContext("modify", setMotifiedExpense);
</script>

<!-- CSS Logic -->

<!-- Components -->
<Navbar />
<main class="content">
  <ExpenseForm
    {addExpense}
    name={setName}
    amount={setAmount}
    {isEditing}
    {editExpense}
  />
  <Totals title="Total Expenses" {total} />
  <ExpenseList {expenses} />
  <button class="btn btn-primary btn-block" on:click={clearExpenses}>
    Clear Expenses
  </button>
</main>
