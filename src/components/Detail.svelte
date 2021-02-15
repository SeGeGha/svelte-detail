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
        <slot></slot>
    </div>

    <div title="Close" class="btn-close" on:click|stopPropagation={closeWidget}>
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512">
            <path d="M405 136.798L375.202 107 256 226.202 136.798 107 107 136.798 226.202 256 107 375.202 136.798 405 256 285.798 375.202 405 405 375.202 285.798 256z" />
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

    .btn-close {
        position: absolute;
        top: 10%;
        right: 2rem;
        width: 25px;
        height: 25px;
        cursor: pointer;
    }

    .btn-close > svg {
        fill: #fff;
    }
</style>
