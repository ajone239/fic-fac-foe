<script lang="ts">
	import Cell from './Cell.svelte';

	let game_state = $state(<number[]>[]);

	let counts = [...Array(4).keys()];
</script>

<h1>the place the game will go</h1>

<div class="game-board">
	{#each counts as row (row)}
		<div class="game-row">
			{#each counts as col (col)}
				<Cell id={row * 4 + col} {game_state} />
			{/each}
		</div>
	{/each}
</div>

<style>
	.game-row {
		display: flex;
		flex-direction: row;
		gap: 0.5em;
	}
	.game-board {
		display: flex;
		gap: 0.5em;
		flex-direction: column;
		align-items: center; /* center rows horizontally */
		justify-content: center; /* center vertically */
	}
</style>
