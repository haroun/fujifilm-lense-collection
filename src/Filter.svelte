<script>
	export let filter;
	
	const label = filter.name;
</script>

<label>
	{label}
	<input type="checkbox" name="{filter.name}" bind:checked={filter.value} />
</label>

<style>
	label {
		flex: 1 1 auto;
		border-left: 1px dotted #dedede;
		border-right: 1px dotted #dedede;
	}
</style>
