<script>
  import { onMount, afterUpdate, setContext } from "svelte";
  // components
  import Navbar from "./Navbar.svelte";
  import Title from "./Title.svelte";
  import ExpensesList from "./ExepensesList.svelte";
  import Totals from "./Totals.svelte";
  import ExpenseForm from "./ExpenseForm.svelte";
  import Modal from "./Modal.svelte";
  // data
  //import expensesData from "./expenses.js";
  // variables
  let expenses = [];
  let setName = "";
  let setAmount = null;
  let setId = null;
  // toggle form variables
  let isFormOpen = false;
  // reactive
  $: total = expenses.reduce((acc, curr) => (acc += curr.amount), 0);
  $: isEditing = !!setId;
  // functions
  function showForm() {
    isFormOpen = true;
  }

  function hideForm() {
    isFormOpen = false;

    setId = null;
    setAmount = null;
    setName = "";
  }

  function addExpense({ name, amount }) {
    amount = parseFloat(amount);
    const expense = {
      id: Math.random() * Date.now(),
      name,
      amount
    };
    expenses = [expense, ...expenses];
  }

  function setModifiedExpense(expenseId) {
    const expense = expenses.find(item => item.id === expenseId);

    setId = expense.id;
    setName = expense.name;
    setAmount = expense.amount;

    showForm();
  }

  function editExpense({ name, amount }) {
    amount = parseFloat(amount);
    expenses = expenses.map(exp =>
      exp.id === setId ? { ...exp, name, amount } : { ...exp }
    );

    setId = null;
    setAmount = null;
    setName = "";
  }

  function removeExpense(id) {
    expenses = expenses.filter(item => item.id !== id);
  }

  function clearExpenses() {
    expenses = [];
  }
  // context
  setContext("remove", removeExpense);
  setContext("modify", setModifiedExpense);
  // local storage
  function setLocalStorage() {
    localStorage.setItem("expense", JSON.stringify(expenses));
  }

  onMount(() => {
    let inMemoryExpenses = localStorage.getItem("expense");

    expenses = inMemoryExpenses ? JSON.parse(inMemoryExpenses) : [];
  });

  afterUpdate(() => {
    setLocalStorage();
  });
</script>

<Navbar {showForm} />
<main class="content">
  {#if isFormOpen}
    <Modal>
      <ExpenseForm
        {addExpense}
        {editExpense}
        {isEditing}
        {hideForm}
        name={setName}
        amount={setAmount} />
    </Modal>
  {/if}
  <Totals title="total expenses" {total} />
  <ExpensesList {expenses} />
  <button
    type="button"
    class="btn btn-primary btn-block"
    on:click={clearExpenses}>
    clear expenses
  </button>
</main>
