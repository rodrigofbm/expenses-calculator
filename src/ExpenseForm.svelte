<script>
  import Title from "./Title.svelte";

  export let name = "";
  export let amount = null;
  export let hideForm;
  export let addExpense;
  export let editExpense;
  export let isEditing;

  $: isEmpty = !name || !amount;

  function handleSubmit() {
    if (isEditing) {
      editExpense({ name, amount });
    } else {
      addExpense({ name, amount });
    }

    name = "";
    amount = null;
  }
</script>

<section class="form">
  <Title title="add expense" />

  <form class="expense-form" on:submit|preventDefault={handleSubmit}>
    <div class="form-control">
      <label for="name">name</label>
      <input type="text" id="name" bind:value={name} />
    </div>

    <div class="form-control">
      <label for="amount">amount</label>
      <input type="text" id="amount" bind:value={amount} />
    </div>

    {#if isEmpty}
      <p class="form-empty">Please fill out all form fields</p>
    {/if}

    <button
      type="submit"
      class="btn btn-block"
      class:disabled={isEmpty}
      disabled={isEmpty}>
      {isEditing ? 'edit expense' : 'add expense'}
    </button>

    <button type="button" class="close-btn" on:click={() => hideForm()}>
      <i class="fas fa-times" />
      close
    </button>
  </form>
</section>
