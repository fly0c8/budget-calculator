<script>
  import { onMount, afterUpdate, setContext } from "svelte";
  import Navbar from "./Navbar.svelte";
  import ExpensesList from "./ExpensesList.svelte";
  import Totals from "./Totals.svelte";
  import ExpenseForm from "./ExpenseForm.svelte";
  import Modal from "./Modal.svelte";
  import Github from './Github.svelte';
import GithubAwait from "./GithubAwait.svelte";
  // import expensesData from "./expenses";
  let setName = "";
  let setAmount = null;
  let setId = null;
  let isFormOpen = false;
  // let expenses = [...expensesData];
  let expenses = [];

  $: isEditing = setId ? true : false;
  $: total = expenses.reduce((acc, curr) => {
    return acc + curr.amount;
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
    expenses = [...expenses, expense];    
  }
  function editExpense({ name, amount }) {
    expenses = expenses.map((item) => {
      return item.id === setId ? { ...item, name, amount } : { ...item };
    });
    setId = null;
    setAmount = null;
    setName = "";    
  }

  function setModifiedExpense(id) {
    let expense = expenses.find((item) => item.id === id);
    console.log(expense);
    setId = expense.id;
    setName = expense.name;
    setAmount = expense.amount;
    showForm();
  }

  function setLocalStorage() {
    localStorage.setItem('expenses', JSON.stringify(expenses))
  }

  onMount(()=>{
    expenses = localStorage.getItem('expenses') ? 
      JSON.parse(localStorage.getItem('expenses')) : [];
  });
  afterUpdate(()=>{
    setLocalStorage();
  });

  setContext("remove", removeExpense);
  setContext("add", addExpense);
  setContext("modify", setModifiedExpense);
  setContext("edit", editExpense);
</script>

<Navbar {showForm} />
<main class="content">
  <!-- {#if isFormOpen}
  <Modal>
      <ExpenseForm name={setName} amount={setAmount} {isEditing} {hideForm} />
  </Modal>  
  {/if}

  <Totals title="total expenses" {total} />
  <ExpensesList {expenses} />

  <button
    type="button"
    class="btn btn-primary btn-block"
    on:click={clearExpenses}>clear expenses</button
  > -->

  <!-- <Github /> -->
  <GithubAwait />
</main>
