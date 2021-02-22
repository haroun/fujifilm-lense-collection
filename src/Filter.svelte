<script>
  import {createEventDispatcher} from 'svelte';

  export let filter;

  const dispatch = createEventDispatcher();

  function toggleFilter() {
    dispatch('toggleFilter', {name: filter.name, value: filter.value})
  }
</script>

<label>
  {filter.name}
  <input type="checkbox" name="{filter.name}" bind:checked={filter.value} on:change={toggleFilter} />
</label>

<style>
  label {
    flex: 1 1 auto;
    border-left: 1px dotted #dedede;
    border-right: 1px dotted #dedede;
  }
</style>
