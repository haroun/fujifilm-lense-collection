<script>
  import Lense from "./Lense.svelte";
  import Filter from "./Filter.svelte";

  export let name;
  
  let store;

  $: fetch('/store.json')
    .then(r => r.json())
    .then(data => {
      store = data;
      window.scrollTo(0, 0);
    });

	const filters = [
    { name: 'weather-resistance', value: false},
    { name: 'macro', value: false},
    { name: 'low-light', value: false},
    { name: 'portrait', value: false},
    { name: 'landscape', value: false},
    { name: 'sharpness', value: false},
  ];

	function toggle(critera) {
		if (filters[criteria]) {
			filters[criteria] = !filters[criteria]
		}
	}

  function handleKeydown(event) {
    switch (event.key) {
    case '1':
      toggle('isWeatherResistance');
      break;
    case '2':
      toggle('isMacro');
      break;
    case '3':
      toggle('isLowLight');
      break;
    case '4':
      toggle('isPortrait');
      break;
    case '5':
      toggle('isLandscape');
      break;
    case '6':
      toggle('isSharpness');
      break;
    }
  }
</script>

<svelte:window on:keydown={handleKeydown}/>

<main>
  <h1>{name}</h1>
  {#if store}
    {#each store.primes as lense}
      <Lense {lense} />
    {/each}
    <div class="filters">
      {#each filters as filter (filter.name)}
        <Filter {filter} />
      {/each}
    </div>
  {:else}
    <p class="loading">loading...</p>
  {/if}
</main>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
    display: flex;
    flex-flow: row wrap;
    color: #4e4e4e;
  }

  h1 {
    color: #ed1a3a;
    text-transform: uppercase;
    font-size: 3em;
    font-weight: 100;
    flex-basis: 100%;
  }

  .filters {
    flex-basis: 100%;
    display: flex;
    flex-flow: row wrap;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }

  .loading {
    opacity: 0;
    animation: 0.4s 0.8s forwards fade-in;
  }

  @keyframes fade-in {
    from { opacity: 0; }
    to { opacity: 1; }
  }
</style>
