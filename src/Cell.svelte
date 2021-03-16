<script>
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();
	export let rows;
	export let attrs;
	export let value;
	let backgroundColor = '';
	let initialRun = true;
	let timer;
	function onClick(e) {
		dispatch('click', { row: e.target.attributes.row.value, column: e.target.attributes.col.value });
	}
	$: if (!initialRun) {
		backgroundColor = value === 0 ? 'green' : 'yellow';
	} else {
		initialRun = false;
	}
	$: if (backgroundColor) {
		if (timer) {
			clearTimeout(timer);
			timer = null;
		}
		timer = setTimeout(() => {
			backgroundColor = '';
			timer = null;
		}, 1000);
	}
</script>

<span
	class="cell"
	on:click={onClick}
	style="width: calc((100vmin - 40px) / {rows}); background-color: {backgroundColor}"
	{...attrs}
>
	{value !== 0 ? value : ''}
</span>

<style>
	.cell {
		border-left: 1px solid lightgray;
		border-top: 1px solid lightgray;
		font-size: xx-small;
		cursor: pointer;
		text-align: center;
		transition: background-color ease 1s;
	}
	.cell:hover {
		color: red;
		font-weight: bold;
	}
	.cell:last-child {
		border-right: 1px solid lightgray;
	}
</style>
