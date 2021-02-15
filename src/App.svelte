<script>
  import Lense from "./Lense.svelte";
  import Filter from "./Filter.svelte";

  // const pipe = (...functions) => (initial) => functions.reduce((accumulator, current) => current(accumulator), initial)
    const pipe = (...functions) => {
        console.log(functions)
        return (initial) => {
            console.log(initial)
            return functions.reduce(
                (accumulator, current) => {
                    console.log(current)
                    return current(accumulator)
                },
                initial
            )
        }
    }

  export let name;

  let inventory;
  let store;

  let filterWeatherResistance = {
    name: 'weather-resistance',
    value: false,
    filter: (lense => lense.name.includes('WR'))
  };
  let filterMacro = {
    name: 'macro',
    value: false,
    filter: (lense =>
      lense.name.includes('Macro')
      || lense.focusRange.min <= 300
      || lense.maxMagnification > 0.3
    )
  };
  let filterLowLight = {
    name: 'low-light',
    value: false,
    filter: (lense => lense.aperture.max < 2)
  };
  let filterPortrait = {
    name: 'portrait',
    value: false,
    filter: (lense =>
      (lense.focal < 50 && lense.aperture.max < 2)
      || (lense.focal >= 50 && lense.aperture.max < 2.4)
    )
  };
  let filterLandscape = {
    name: 'landscape',
    value: false,
    filter: (lense => lense.focal <= 23)
  };
  let filterSharpness = {
    name: 'sharpness',
    value: false,
    filter: (lense =>
      lense.construction.elements > 10
      && (
        lense.construction.details['extra low dispersion'] >= 3
        || lense.construction.details['super extra low dispersion'] > 0
        )
    )
  };
	let filters = [
    filterWeatherResistance,
    filterMacro,
    filterLowLight,
    filterPortrait,
    filterLandscape,
    filterSharpness,
  ];

  $: storeFilters = pipe(
    ...filters
      .filter(filter => filter.value)
      .map(filter => filter.filter)
      .concat((lense => lense))
  )

  $: fetch('/store.json')
    .then(r => r.json())
    .then(data => {
      inventory = data;
      window.scrollTo(0, 0);
    });

  $: if (inventory) {
    store = JSON.parse(JSON.stringify(inventory.primes.filter(storeFilters)))
  }
</script>

<main>
  <h1>{name}</h1>
  {#if store}
    {#each store as lense}
      <Lense {lense} />
    {/each}
    <div class="filters">
      {#each filters as filter (filter.name)}
        <Filter {filter} value="{filter.value}" />
      {/each}
    </div>
    <pre>
      {filterSharpness.value}
    </pre>
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
    flex-basis: 100%;
    opacity: 0;
    animation: 0.4s 0.8s forwards fade-in;
  }

  @keyframes fade-in {
    from { opacity: 0; }
    to { opacity: 1; }
  }
</style>
