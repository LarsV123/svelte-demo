<script>
	import Row from "$lib/WordleRow.svelte";
	export let guesses;
	export let word;
</script>

<board>
	{#each guesses as guess}
		<Row {guess} {word} />
	{:else}
		<Row guess="" {word} />
	{/each}
</board>

<style>
	board {
		display: flex;
		flex-direction: column;
		gap: 0.25rem;
		margin: 2rem auto;
	}
</style>
