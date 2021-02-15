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
			<Detail item={{ ...itemDetail }} on:close={closeDetailWidget}>
				<div class="slide">
					<div class="slide-thumb-wrapper" />
				</div>
				
				<div class="interaction">
					<div class="actions">
						<a
							class="slide-open"
							target="_blank"
							href="/api/sm/download/pres-{itemDetail.id}"
							title="Open original"
						>
							<div class="icon">
								<svg
									xmlns="http://www.w3.org/2000/svg"
									viewBox="0 0 384 512"
									class="svelte-c8tyih"
									><path
										d="M224 136V0H24C10.7 0 0 10.7 0 24v464c0 13.3 10.7 24 24 24h336c13.3 0 24-10.7 24-24V160H248c-13.2 0-24-10.8-24-24zm64 236c0 6.6-5.4 12-12 12H108c-6.6 0-12-5.4-12-12v-8c0-6.6 5.4-12 12-12h168c6.6 0 12 5.4 12 12v8zm0-64c0 6.6-5.4 12-12 12H108c-6.6 0-12-5.4-12-12v-8c0-6.6 5.4-12 12-12h168c6.6 0 12 5.4 12 12v8zm0-72v8c0 6.6-5.4 12-12 12H108c-6.6 0-12-5.4-12-12v-8c0-6.6 5.4-12 12-12h168c6.6 0 12 5.4 12 12zm96-114.1v6.1H256V0h6.1c6.4 0 12.5 2.5 17 7l97.9 98c4.5 4.5 7 10.6 7 16.9z"
									/></svg
								>
							</div>
							Open in GoogleSlides
						</a>
				
						<a
							class="slide-download"
							target="_blank"
							href="/api/sm/download/pres-{itemDetail.id}"
							title="Download original"
						>
							<div class="icon">
								<svg
									xmlns="http://www.w3.org/2000/svg"
									viewBox="0 0 384 512"
									class="svelte-c8tyih"
									><path
										d="M193.7 271.2c8.8 0 15.5 2.7 20.3 8.1 9.6 10.9 9.8 32.7-.2 44.1-4.9 5.6-11.9 8.5-21.1 8.5h-26.9v-60.7h27.9zM377 105L279 7c-4.5-4.5-10.6-7-17-7h-6v128h128v-6.1c0-6.3-2.5-12.4-7-16.9zm-153 31V0H24C10.7 0 0 10.7 0 24v464c0 13.3 10.7 24 24 24h336c13.3 0 24-10.7 24-24V160H248c-13.2 0-24-10.8-24-24zm53 165.2c0 90.3-88.8 77.6-111.1 77.6V436c0 6.6-5.4 12-12 12h-30.8c-6.6 0-12-5.4-12-12V236.2c0-6.6 5.4-12 12-12h81c44.5 0 72.9 32.8 72.9 77z"
									/></svg
								>
							</div>
							Download in PowePoint
						</a>
					</div>
				
					<div class="tags">
						<p>Related keywords:</p>
				
						<p>...TODO...</p>
					</div>
				</div>
			</Detail>
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

	.slide {
    display: inline-block;
    margin-right: 6%;
    text-align: center;
}

.slide .slide-thumb-wrapper {
    width: 100%;
    height: 15vw;
}

.interaction {
    display: flex;
    flex-direction: column;
}

.actions {
    margin-bottom: 0.875rem;
    display: flex;
    flex-direction: column;
    flex-grow: 1;
    width: 220px;
}

.actions > a {
    display: flex;
    justify-content: flex-start;
    align-items: center;
    padding: 0 1rem;
    height: 40px;
    font-size: 0.875rem;
    line-height: 125%;
    color: #fff;
}

.actions > a:not(:last-child) {
    margin-bottom: 0.875rem;
}

a.slide-open {
    background-color: #32bd7a;
}

a.slide-download {
    background-color: #e95f59;
}

.icon {
    margin-right: 1.2rem;
    display: inline-block;
    width: 0.875rem;
    cursor: pointer;
}

.tags > p {
    font-size: 0.875rem;
    line-height: 125%;
    color: #c6c6c6;
}

.tags > p:first-child {
    margin-bottom: 0.5em;
}

.tags > p:last-child {
    font-weight: 300;
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
