<script lang="ts">
	import Cell from './Cell.svelte';

	const size = 4;
	const counts = [...Array(size).keys()];

	let game_state = $state(<number[]>[]);

	let naughts = $derived(game_state.filter((_, idx) => (idx & 1) == 0));
	let crosses = $derived(game_state.filter((_, idx) => (idx & 1) != 0));

	const ways_to_win = [
		//horz
		[0, 1, 2, 3],
		[4, 5, 6, 7],
		[8, 9, 10, 11],
		[12, 13, 14, 15],
		// vert
		[0, 4, 8, 12],
		[1, 5, 9, 13],
		[2, 6, 10, 14],
		[3, 7, 11, 15],
		// diag
		[0, 5, 10, 11],
		[3, 6, 9, 12],
		// corners
		[0, 1, 4, 5],
		[2, 3, 6, 7],
		[8, 9, 12, 13],
		[10, 11, 14, 15],
		// middle edge
		[1, 2, 5, 6],
		[4, 5, 8, 9],
		[6, 7, 10, 11],
		[9, 10, 13, 14],
		// center
		[5, 6, 9, 10]
	];

	const has_won = (indexes: number[]): boolean => {
		for (let way of ways_to_win) {
			if (way.every((w) => indexes.includes(w))) {
				return true;
			}
		}
		return false;
	};

	let header_text = $derived.by(() => {
		if (has_won(naughts)) {
			return 'O has won!!';
		}

		if (has_won(crosses)) {
			return 'X has won!!';
		}

		if (game_state.length >= size * size) {
			return 'Draw';
		}

		return 'Play!';
	});

	let over = $derived(header_text != 'Play!');
</script>

<h1>{header_text}</h1>

<div class="game-board">
	{#each counts as row (row)}
		<div class="game-row">
			{#each counts as col (col)}
				<Cell id={row * size + col} {over} {game_state} />
			{/each}
		</div>
	{/each}
</div>

<div class="control-row">
	<button onclick={() => game_state.pop()}>Undo</button>

	<button onclick={() => (game_state = [])}>Reset</button>
</div>

<style>
	h1 {
		text-align: center;
	}
	button {
		height: 2em;
		width: 8em;
		border-radius: 0.5em;
		margin: 0 0 1em 0;
		background-color: #ddd;
	}
	.control-row {
		display: flex;
		flex-direction: row;
		justify-content: center; /* center vertically */
		gap: 6em;
	}
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
