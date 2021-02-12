<script>
	import { flip } from "svelte/animate";

	import Detail from "./components/Detail.svelte";
	let data = [
		{ id: 1 },
		{ id: 2 },
		{ id: 3 },
		{ id: 4 },
		{ id: 5 },
		{ id: 6 },
		{ id: 7 },
		{ id: 8 },
		{ id: 9 },
		{ id: 10 },
		{ id: 11 },
		{ id: 12 },
	];

	let slide_detail = {};
	function toggle_detail_slide(dom_el, slide_info) {
		slide_detail = {
			...slide_info,
			dom_el,
		};
	}

	function close_detail_widget() {
		slide_detail = {};
	}
</script>

<main>
	<div class="slides-grid">
		{#each data as slide (slide.id)}
			<div
				class="slide-thumb"
				on:click={({ currentTarget }) =>
					toggle_detail_slide(currentTarget, slide)}
				animate:flip={{ duration: 350 }}
			/>
		{/each}

		{#if slide_detail.id}
			<Detail
				slide={{ ...slide_detail }}
				on:close={close_detail_widget}
			/>
		{/if}
	</div>
</main>

<style>
	.slides-grid {
		position: relative;
		margin-bottom: 2%;
		display: flex;
		flex-wrap: wrap;
		gap: 1.375rem;
		width: 80%;
	}

	.slide-thumb {
		position: relative;
		width: calc((100% - 3 * 1.375rem) / 4);
		height: 11vw;
		background: red;
	}

	@media screen and (max-width: 1023px) {
		.slide-thumb {
			width: calc((100% - 2 * 1.375rem) / 3);
			height: 14vw;
		}
	}

	@media screen and (max-width: 991px) {
		.slide-thumb {
			width: calc((100% - 1.375rem) / 2);
			height: 21vw;
		}
	}

	@media screen and (max-width: 767px) {
		.slides-grid {
			justify-content: center;
		}

		.slide-thumb {
			width: 280px;
			height: 160px;
		}
	}
</style>
