<script>
  import Results from "./Results.svelte";
  import OptionGrid from "./OptionGrid.svelte";
  import { sampleOptions } from "./sampleOptions";
  import { onMount } from "svelte";
  let optionsInput, options, currentComparison, wins, comparisons;

  let inputDelimiter = "\n";

  onMount(() => {
    const savedOptions = loadOptions();
    if (savedOptions) {
      options = savedOptions;
    } else {
      options = sampleOptions["social"];
    }
    optionsInput = options.join(inputDelimiter);
    restartComparisons();

    function loadOptions() {
      const savedOptions = window.localStorage.getItem("options");
      if (savedOptions) {
        try {
          return JSON.parse(savedOptions);
        } catch (e) {
          console.error("Failed to parse saved options.", savedOptions);
        }
      }
    }
  });

  function getComparisons(optionCount) {
    const output = [];
    for (let i = 0; i < optionCount; i++) {
      for (let j = i + 1; j < optionCount; j++) {
        output.push([i, j]);
      }
    }
    return output;
  }

  function addWin(option) {
    if (typeof wins[option] == "number") {
      wins[option]++;
    } else {
      wins[option] = 1;
    }

    if (currentComparison < comparisons.length - 1) {
      currentComparison++;
    } else {
      console.log("Comparisons Complete!");
    }
  }

  function restartComparisons() {
    currentComparison = 0;
    wins = {};
    options = optionsInput.split("\n");
    comparisons = getComparisons(options.length);
    localStorage.setItem("options", JSON.stringify(options));
  }
</script>

<div class="app-container">
  <!-- {JSON.stringify(comparisons)} -->
  <details open>
    <summary>Contestants</summary>
    <div class="flex flex-col">
      <textarea rows="4" bind:value={optionsInput} />
      <button on:click={restartComparisons}>Submit</button>
    </div>
  </details>

  {#if options}
    {#if currentComparison < comparisons.length - 1}
      <h3>Comparison #{currentComparison + 1} of {comparisons.length}</h3>
      <OptionGrid
        options={[
          options[comparisons[currentComparison]?.[0]],
          options[comparisons[currentComparison]?.[1]],
        ]}
        onChoose={addWin}
      ></OptionGrid>
    {:else}
      <h3>Done! Results Based on {comparisons.length} Comparisons</h3>
    {/if}
  {/if}

  {#if wins}
    <div>
      <details open>
        <summary>Results</summary>
        {#if Object.entries(wins).length}
          <ul class="m-0">
            {#each Object.entries(wins)
              .sort((a, b) => b[1] - a[1])
              .map((x) => `${x[0]}: ${x[1]}`) as entry}
              <li>{entry}</li>
            {/each}
          </ul>
        {:else}
          Results will appear as you choose winners.
        {/if}
      </details>
    </div>
  {/if}

  <!-- <Results></Results> -->
</div>

<style>
  .app-container {
    display: flex;
    flex-flow: column nowrap;
    flex: 1 1 0;
  }
</style>
