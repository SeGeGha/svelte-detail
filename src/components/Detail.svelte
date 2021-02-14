<script>
    import { fly } from "svelte/transition";
    import { createEventDispatcher } from "svelte";

    const dispatch = createEventDispatcher();

    export let item;

    const ANIMATION_DURATION = 300;

    // https://learn.javascript.ru/size-and-scroll-window#shirina-vysota-dokumenta
    const initDocScrollHeight = Math.max(
        document.body.scrollHeight,
        document.documentElement.scrollHeight,
        document.body.offsetHeight,
        document.documentElement.offsetHeight,
        document.body.clientHeight,
        document.documentElement.clientHeight
    );

    // SlideDetail positioning
    $: triangle_offset_left = (item.targetDOM.offsetLeft + item.targetDOM.offsetWidth / 2.2).toFixed(2) + "px";
    $: comp_offset_top = (item.targetDOM.offsetTop + item.targetDOM.offsetHeight).toFixed(2) + "px";

    function closeWidget() {
        // Checking if the widget has expanded the dimensions of document.body
        const scrollY = window.innerHeight + window.scrollY - initDocScrollHeight;

        if (scrollY > 0) {
            scrollWindow(ANIMATION_DURATION, scrollY);
        }

        dispatch("close");
    }

    // Scroll window if widget goes beyond init_doc_scrollHeight
    function scrollWindow(duration, scrollY) {
        const startTs = performance.now();
        let progressScrollY = 0;

        requestAnimationFrame(function step(currentTs) {
            let progress = (currentTs - startTs) / duration;

            if (progress >= 1) {
                progress = 1;
            }

            const top = (progressScrollY - scrollY) * progress;

            window.scrollBy({
                behavior: "smooth",
                top,
            });

            progressScrollY += -top;

            if (progress < 1) {
                requestAnimationFrame(step);
            }
        });
    }

    /*
	let a;
	onMount(() => {
		a.scrollIntoView({
			behavior: "smooth",
			block: "center",
		});
	}); */
</script>

<div
    style={`top: ${comp_offset_top}`}
    class="slide-details"
    transition:fly|local={{ y: -50, duration: ANIMATION_DURATION }}
>
    <div class="top-triangle" style={`left: ${triangle_offset_left}`} />

    <div class="details-body">
        <div class="slide">
            <div class="slide-thumb-wrapper" />
        </div>

        <div class="interaction">
            <div class="actions">
                <a
                    class="slide-open"
                    target="_blank"
                    href="/api/sm/download/pres-{item.id}"
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
                    href="/api/sm/download/pres-{item.id}"
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
    </div>

    <div
        title="Close"
        class="btn-close"
        on:click|stopPropagation={closeWidget}
    >
        <svg
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 512 512"
            class="svelte-c8tyih"
        >
            <path
                d="M405 136.798L375.202 107 256 226.202 136.798 107 107 136.798 226.202 256 107 375.202 136.798 405 256 285.798 375.202 405 405 375.202 285.798 256z"
            />
        </svg>
    </div>
</div>

<style>
    .slide-details {
        position: absolute;
        left: 0;
        z-index: 2;
        padding: 1rem 0;
        width: 100%;
        color: #fff;
        transition: top 0.3s;
    }

    .top-triangle {
        position: absolute;
        top: -0.5rem;
        border: 0.75rem solid transparent;
        border-bottom: 0.75rem solid #242323;
        transition: left 0.3s;
    }

    .details-body {
        display: flex;
        justify-content: center;
        padding: 6.5% 1rem;
        width: 100%;
        background-color: #242323;
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

    .btn-close {
        position: absolute;
        top: 10%;
        right: 2rem;
        width: 25px;
        height: 25px;
        cursor: pointer;
    }
</style>
