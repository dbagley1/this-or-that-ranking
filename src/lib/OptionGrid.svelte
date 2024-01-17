<script>
  export let options;
  export let onChoose;
</script>

<div class="option-grid grid grid-cols-1 md:grid-cols-2">
  {#each options as option}
    <div class="option-card" on:click={() => onChoose(option)}>{option}</div>
  {/each}
</div>

<style>
  .option-grid {
    gap: 20px;
    /* flex: 1 1 0; */
  }
  .option-card {
    display: flex;
    font-size: 1.5rem;
    padding: 10px;
    font-weight: bold;
    justify-content: center;
    align-items: center;
    border: solid 1px #e5e7eb;
  }
</style>
