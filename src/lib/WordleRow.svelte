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
		display: flex;
		gap: 0.25rem;
	}
</style>
