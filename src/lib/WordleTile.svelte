<script>
	export let char = '';
	export let status = 'absent';
</script>

<tile class={status}>
	{char}
</tile>

<style>
	tile {
		width: 50px;
		height: 50px;
		color: #d7dadc;
		font-size: 2rem;
		font-weight: bold;
		display: inline-flex;
		justify-content: center;
		align-items: center;
		vertical-align: middle;
		text-transform: uppercase;
	}

	.correct {
		background-color: #538d4e;
	}

	.present {
		background-color: #b59f3b;
	}

	.absent {
		background-color: #3a3a3c;
	}
</style>
