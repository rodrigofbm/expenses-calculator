<script>
  import SectionTitle from './Title.svelte';
  import ExpenseItem from './ExpenseItem.svelte';

  export let expenses = [];
</script>

<style>
  h2{
    text-transform: capitalize;
  }
</style>

<section>
  <SectionTitle title="exenses list" />

  <ul>
    {#each expenses as expense}
      <div >
        <ExpenseItem {...expense} />
      </div>
    {:else}
      <h2>no expenses added to the list</h2>
    {/each}
  </ul>
</section>