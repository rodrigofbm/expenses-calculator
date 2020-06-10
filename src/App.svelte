<script>
  import { setContext } from 'svelte';
  // components
  import Navbar from './Navbar.svelte';
  import Title from './Title.svelte';
  import ExpensesList from './ExepensesList.svelte';
  import Totals from './Totals.svelte';
  import ExpenseForm from './ExpenseForm.svelte';
  // data
  import expensesData from './expenses.js';
  // variables
  let expenses = [...expensesData];
  // reactive
  $: total = expenses.reduce((acc, curr) => acc += curr.amount, 0);
  // functions
  function removeExpense(id) {
    expenses = expenses.filter(item => item.id !== id);
  }
  function clearExpenses() {
    expenses = [];
  }
  // context
  setContext("remove", removeExpense);
</script>

<Navbar />
<main class="content">
  <ExpenseForm />
  <Totals title="total expenses" {total} />
  <ExpensesList {expenses} />
  <button 
    type="button" 
    class="btn btn-primary btn-block"
    on:click={clearExpenses}
  >
    clear expenses
  </button>
</main>