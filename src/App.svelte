<script>
  import { onMount } from "svelte";
  import { afterUpdate } from "svelte";

  let value = '';
  let submittedValue = null;
  let data = {};

  async function fetchData() {
    const response = await fetch(`http://bikol.vm.wmi.amu.edu.pl/dtin/results/${value}`);
    const json = await response.json();
    data = json.tasks;
  }

  onMount(fetchData);
</script>

<main>
  <form on:submit|preventDefault={() => { submittedValue = value; fetchData(); }}>
    <label>
      Search
      <input bind:value />
    </label>

    <button on:click={() => { console.log('button clicked'); fetchData(); }}>GO</button>
  </form>
  <h1>Wyniki zadań</h1>

  <ul>
    {#each Object.entries(data) as [taskName, task]}
      <li>
        <h2>{taskName}</h2>
        <p>Czas: {task.time}</p>
        <p>Wynik: {task.score}</p>
        <p>Informacje: {task.info}</p>
      </li>
    {/each}
  </ul>
</main>
