<script>
  import { setContext, onMount, afterUpdate } from "svelte";

  const state = {
    removeExpense,
    setModifiedExpense
  };

  // components
  import Navbar from "./Navbar.svelte";
  import ExpensesList from "./ExpensesList.svelte";
  import TotalExpenses from "./TotalExpenses.svelte";
  import ExpenseForm from "./ExpenseForm.svelte";
  import Modal from "./Modal.svelte";

  // data
  // import expensesData from "./expenses";

  // variables
  let expenses = [];

  // set editing variables
  let setName = "";
  let setAmount = null;
  let setId = null;

  // toggle form variables
  let isFormOpen = false;

  // reactives
  $: isEditing = setId ? true : false;
  $: totalExpenses = expenses.reduce((acc, curr) => {
    return (acc += curr.amount);
  }, 0);

  // functions
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
    expenses = expenses.filter(expense => expense.id !== id);
  }

  function clearExpenses() {
    expenses = [];
  }

  function addExpense({ name, amount }) {
    let expense = { id: Math.random() * Date.now(), name, amount };
    expenses = [expense, ...expenses];
  }

  function setModifiedExpense(id) {
    let expense = expenses.find(expense => expense.id === id);

    setId = expense.id;
    setName = expense.name;
    setAmount = expense.amount;

    showForm();
  }

  function editExpense({ name, amount }) {
    expenses = expenses.map(expense => {
      return expense.id === setId ? { ...expense, name, amount } : expense;
    });
    setId = null;
    setAmount = null;
    setName = "";
  }

  // context
  setContext("state", state);

  // local storage
  function setLocalStorage() {
    localStorage.setItem("expenses", JSON.stringify(expenses));
  }

  onMount(() => {
    expenses = localStorage.getItem("expenses")
      ? JSON.parse(localStorage.getItem("expenses"))
      : [];
  });

  afterUpdate(params => {
    setLocalStorage();
  });
</script>

<Navbar {showForm} />
<main class="content">
  {#if isFormOpen}
    <Modal>
      <ExpenseForm
        {addExpense}
        name={setName}
        amount={setAmount}
        {isEditing}
        {editExpense}
        {hideForm} />
    </Modal>
  {/if}
  <TotalExpenses title="total expenses" {totalExpenses} />
  <ExpensesList {expenses} />
  <button
    type="button"
    on:click={clearExpenses}
    class="btn btn-primary btn-block">
    Clear expenses
  </button>
</main>
