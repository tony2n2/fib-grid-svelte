<script>
	import * as FibGrid from './FibGrid';
	import Cell from './Cell.svelte';
	const ROWS = 50;
	const COLUMNS = 50;
	let fg = FibGrid.create(ROWS, COLUMNS);
	function clickHandler(e) {
		fg = FibGrid.incrementRowCol(fg, e.detail.row, e.detail.column);
		const fibs = FibGrid.matchFibOnRowCol(fg, e.detail.row, e.detail.column);
		// reset matched cells to empty after 1s
		setTimeout(() => {
			fibs.forEach((seq) => {
				fg = FibGrid.resetRange(fg, seq);
			});
		}, 1000);
	}
</script>

<main>
	<div class="grid">
		{#each fg as row, rowId}
			<div
				class="row"
				key={rowId}
				style="flex-basis: calc((100vmin - 40px) / {ROWS});
					line-height: calc((100vmin - 40px) / {ROWS} - 1px)"
			>
				{#each row as cell, colId}
					<Cell rows={ROWS} attrs={{ row: rowId, col: colId }} value={cell} on:click={clickHandler} />
				{/each}
			</div>
		{/each}
	</div>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	.grid {
		display: flex;
		flex-direction: column;
	}
	.row {
		display: flex;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>
