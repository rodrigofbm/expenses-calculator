<script>
  import { setContext } from 'svelte';
  // components
  import Navbar from './Navbar.svelte';
  import Title from './Title.svelte';
  import ExpensesList from './ExepensesList.svelte';
  // data
  import expensesData from './expenses.js';
  // variables
  let expenses = [...expensesData];
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
  <ExpensesList {expenses} />
  <button 
    type="button" 
    class="btn btn-primary btn-block"
    on:click={clearExpenses}
  >
    clear expenses
  </button>
</main>