<script>
	import Tile from "$lib/WordleTile.svelte";
	export let guess = "";
	export let word = "";
	$: wordChars = word.split("");
	$: tiles = wordChars.map((wordChar, index) => {
		let char = guess.charAt(index) ? guess.charAt(index) : " ";
		let status = "absent";
		if (char == wordChar) {
			status = "correct";
		} else if (wordChars.includes(char)) {
			status = "present";
		}
		return { char: char, status: status };
	});
</script>

<row>
	{#each tiles as tile}
		<Tile char={tile.char} status={tile.status} />
	{/each}
</row>

<style>
	row {
		display: grid;
		grid-gap: 0.25rem;
		grid-template-columns: repeat(5, 50px);
		padding-bottom: 0.25rem;
		margin-bottom: 1rem;
		border-bottom: solid 0.2rem var(--text-color);
	}
</style>
