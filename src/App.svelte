<script>
	import { flip } from "svelte/animate";

	import Detail from "./components/Detail.svelte";

	let data = [
		{ id: 1, color: 'red', },
		{ id: 2, color: 'yellow', },
		{ id: 3, color: 'green', },
		{ id: 4, color: 'purple', },
		{ id: 5, color: 'grey', },
		{ id: 6, color: 'gold', },
		{ id: 7, color: 'aqua' },
		{ id: 8, color: 'chocolate', },
		{ id: 9, color: 'wheat' },
		{ id: 10, color: 'white', },
		{ id: 11, color: 'steelblue', },
		{ id: 12, color: 'pink', },
		{ id: 13, color: 'red', },
		{ id: 14, color: 'yellow', },
		{ id: 15, color: 'green', },
		{ id: 16, color: 'purple', },
		{ id: 17, color: 'grey', },
		{ id: 18, color: 'gold', },
		{ id: 19, color: 'aqua' },
		{ id: 20, color: 'chocolate', },
		{ id: 21, color: 'wheat' },
		{ id: 22, color: 'white', },
		{ id: 23, color: 'steelblue', },
		{ id: 24, color: 'pink', },
	];

	let itemDetail = {};
	
	function toggleDetailItem(targetDOM, itemData) {
		itemDetail = {
			...itemData,
			targetDOM,
		};
	}

	function closeDetailWidget() {
		itemDetail = {};
	}
</script>

<main>
	<div class="data-grid">
		{#each data as item (item.id)}
			<div
				class='data-grid_item'
				style={`background-color: ${item.color}`}
				on:click={({ currentTarget }) => toggleDetailItem(currentTarget, item)}
				animate:flip={{ duration: 350 }}
			/>
		{/each}

		{#if itemDetail.id >= 0}
			<Detail
				item={{ ...itemDetail }}
				on:close={closeDetailWidget}
			/>
		{/if}
	</div>
</main>

<style>
	.data-grid {
		position: relative;
		margin: 2rem auto;
		display: flex;
		flex-wrap: wrap;
		gap: 1.375rem;
		width: 80%;
	}

	.data-grid_item {
		position: relative;
		width: calc((100% - 3 * 1.375rem) / 4);
		height: 11vw;
		cursor: pointer;
		transition: box-shadow 0.2s;
	}

	.data-grid_item:hover {
		box-shadow: 0 14px 28px rgba(0,0,0,0.25), 0 10px 10px rgba(0,0,0,0.22);
	}

	@media screen and (max-width: 1023px) {
		.data-grid_item {
			width: calc((100% - 2 * 1.375rem) / 3);
			height: 14vw;
		}
	}

	@media screen and (max-width: 991px) {
		.data-grid_item {
			width: calc((100% - 1.375rem) / 2);
			height: 21vw;
		}
	}

	@media screen and (max-width: 767px) {
		.data-grid {
			justify-content: center;
		}

		.data-grid_item {
			width: 280px;
			height: 160px;
		}
	}
</style>
