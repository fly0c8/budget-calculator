<script>
  import Title from "./Title.svelte";
  import { getContext } from "svelte";
  import { onMount, onDestroy, beforeUpdate, afterUpdate } from 'svelte';

  export let name = "";
  export let amount = null;
  export let isEditing;
  export let hideForm;

  $: isEmpty = !name || !amount;

  const addExpense = getContext("add");
  const editExpense = getContext("edit");

  function handleSubmit() {
    if (isEditing) {
      editExpense({ name, amount });
    } else {
      addExpense({ name, amount });
    }

    name = "";
    amount = null;
    hideForm();
  }

  onMount(() => {
    console.log('onMount..')
  });
  onDestroy(() => {
    console.log('onDestroy..')
  });
  beforeUpdate(() => {
    console.log('beforeUpdate..')
  });
  afterUpdate(() => {
    console.log('afterUpdate..')
  });


</script>

<section class="form" on:submit|preventDefault={handleSubmit}>
  <Title title="add expense" />
  <form class="expense-form">
    <div class="form-control">
      <label for="name">Name</label>
      <input type="text" id="name" bind:value={name} />
    </div>
    <div class="form-control">
      <label for="amount">Amount</label>
      <input type="number" id="amount" bind:value={amount} />
    </div>
    {#if isEmpty}
      <p class="form-empty">please fill out all form fields</p>
    {/if}

    <button
      type="submit"
      class="btn btn-block "
      class:disabled={isEmpty}
      disabled={isEmpty}
    >
      {#if isEditing}
        edit expense
      {:else}
        add expense
      {/if}
    </button>

    <button type="button" class="close-btn" on:click={hideForm}>
      <i class="fas fa-times" />
      Close
    </button>
  </form>
</section>
