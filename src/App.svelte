<!-- JS Logic -->
<script>
  import { setContext, onMount, afterUpdate } from "svelte";

  import Navbar from "./Navbar.svelte";
  import ExpenseList from "./ExpenseList.svelte";
  import Totals from "./Totals.svelte";
  import ExpenseForm from "./ExpenseForm.svelte";
  import Modal from "./Modal.svelte";

  // import Github from "./Github.svelte";
  // import GithubAwait from "./GithubAwait.svelte";

  let expenses = [];
  let setName = "";
  let setAmount = null;
  let setId = null;
  let isFormOpen = false;

  $: isEditing = setId ? true : false;
  $: total = expenses.reduce((acc, curr) => {
    return (acc += curr.amount);
  }, 0);

  function showForm() {
    isFormOpen = true;
  }

  function hideForm() {
    isFormOpen = false;
    setName = "";
    setAmount = null;
    setId = null;
  }

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
    showForm();
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

  function setLocalStorage() {
    localStorage.setItem("expenses", JSON.stringify(expenses));
  }

  onMount(() => {
    expenses = localStorage.getItem("expenses")
      ? JSON.parse(localStorage.getItem("expenses"))
      : [];
  });

  afterUpdate(() => {
    setLocalStorage();
  });
</script>

<!-- CSS Logic -->

<!-- Components -->
<Navbar {showForm} />
<!-- <GithubAwait /> -->
<main class="content">
  {#if isFormOpen}
    <Modal>
      <ExpenseForm
        {addExpense}
        name={setName}
        amount={setAmount}
        {isEditing}
        {editExpense}
        {hideForm}
      />
    </Modal>
  {/if}
  <Totals title="Total Expenses" {total} />
  <ExpenseList {expenses} />
  <button class="btn btn-primary btn-block" on:click={clearExpenses}>
    Clear Expenses
  </button>
</main>
