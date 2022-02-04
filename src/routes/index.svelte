<script>
	import { onMount } from "svelte";
	import Board from "$lib/WordleBoard.svelte";

	const api = "https://random-words-api.vercel.app/word";
	$: word = "";
	$: definition = "";
	let difficultyLevels = [
		{ level: 0, name: "Hard mode" },
		{ level: 1, name: "Give me a hint at least..." },
		{ level: 2, name: "Cheats enabled" }
	];
	$: difficulty = 0;

	$: guess = "";
	$: guesses = [];
	$: hasWon = guesses.length ? guesses.at(-1).toUpperCase() == word : false;
	$: hasGuessed = false;
	$: easteregg = false;

	const clickGuess = () => {
		if (!hasGuessed) {
			guesses = [];
		}

		if (guess.toLowerCase().startsWith("sudo")) {
			easteregg = true;
		} else {
			easteregg = false;
		}
		guesses = [...guesses, guess.toUpperCase()].filter((x) => x.length > 0);
		guess = "";
		hasGuessed = true;
	};

	const onKeyPress = (e) => {
		if (e.charCode === 13) clickGuess();
	};

	const getWord = async () => {
		const response = await fetch(api);
		const json = await response.json();
		const data = json[0];
		word = data.word.toUpperCase();
		definition = data.definition;
		guesses = [];
		hasGuessed = false;
	};

	onMount(async () => {
		getWord();
	});
</script>

<h1>WORDLE FOR CHEATERS</h1>

<button on:click={getWord}>Get a new word!</button>
<Board {guesses} {word} />

<section>
	<p>Choose a difficulty level:</p>
	{#each difficultyLevels as option}
		<label>
			<input type="radio" bind:group={difficulty} name="difficulty" value={option.level} />
			{option.name}
		</label>
	{/each}

	{#if word}
		{#if difficulty == 0}
			<p>People who don't cheat are really cool. Good luck!</p>
		{:else if difficulty == 1}
			<p>
				The definition of your word is "{definition}". That should make this easy.
			</p>
		{:else}
			<p>
				The word you're looking for is "{word}", which means "{definition}". You really should have
				been able to guess that without cheating...
			</p>
		{/if}
	{/if}

	{#if easteregg}
		<p>You are not in the sudoers file. This incident will be reported.</p>
	{/if}

	{#if hasWon}
		<p>Correct! The word was "{word}", meaning "{definition}".</p>
	{/if}
	<input class="textbox" on:keypress={onKeyPress} bind:value={guess} />
	<button on:click={clickGuess}><span class="flipped">🚀</span> Submit answer 🚀</button>
</section>

<style>
	section {
		display: flex;
		flex-direction: column;
	}
	button {
		max-width: 15rem;
		padding: 0.5rem;
		margin: 2rem auto;
		border-radius: 5%;

		transition: 220ms all ease-in-out;
	}

	button:hover {
		padding: 0.5rem 1rem;
	}

	.textbox {
		width: 100%;
		margin: auto;
	}

	span.flipped {
		display: inline-block;
		transform: scaleX(-1);
	}
</style>
