<script>
  import App from "$lib/App.svelte";

</script>
<main>
    <div class="wrapper">
      <h1 class="m-0">This Or That</h1>
      <p>Which of these do you like better?</p>
    <App />
  </div>
</main>

<style>
  main {
    height: 100%;
    display: flex;
    flex-flow: column nowrap;
  }

  .wrapper {
    padding: 20px;
    flex: 1 1 0;
    display: flex;
    flex-flow: column nowrap;
  }
</style>
