<script>
    import { onMount } from "svelte";
    import { stores } from "@sapper/app";
    import { fly } from "svelte/transition";
    import { createEventDispatcher } from "svelte";

    import SlideThumb from "@src/comp/SlideThumb.svelte";
    import FaFileAlt from "svelte-icons/fa/FaFileAlt.svelte";
    import FaFilePowerpoint from "svelte-icons/fa/FaFilePowerpoint.svelte";
    import FaBookmark from "svelte-icons/fa/FaBookmark.svelte";
    import IoMdClose from "svelte-icons/io/IoMdClose.svelte";
    import Spinner from "sscl/comp/ui/Spinner.svelte";

    const { session } = stores();
    const dispatch = createEventDispatcher();

    export let slide;
    // TODO: export tags

    const ANIMATION_DURATION = 300;

    // https://learn.javascript.ru/size-and-scroll-window#shirina-vysota-dokumenta
    const init_doc_scrollHeight = Math.max(
        document.body.scrollHeight,
        document.documentElement.scrollHeight,
        document.body.offsetHeight,
        document.documentElement.offsetHeight,
        document.body.clientHeight,
        document.documentElement.clientHeight
    );

    // SlideDetail positioning
    $: triangle_offset_left =
        (slide.dom_el.offsetLeft + slide.dom_el.offsetWidth / 2.2).toFixed(2) +
        "px";
    $: comp_offset_top =
        (slide.dom_el.offsetTop + slide.dom_el.offsetHeight).toFixed(2) + "px";

    let posting = false;

    function update_session_collection() {
        posting = true;

        fetch("/api/search/collection", {
            method: "POST",
            credentials: "include",
            headers: {
                "Content-Type": "application/json",
            },
            body: JSON.stringify(slide),
        })
            .then((res) => res.json())
            .then((data) => {
                posting = false;
                $session.collection = data;
            })
            .catch((e) => {
                posting = false;
                console.warn("Error from post collection:", e);
            });
    }

    function close_widget() {
        // Checking if the widget has expanded the dimensions of document.body
        const scrollY =
            window.innerHeight + window.scrollY - init_doc_scrollHeight;

        if (scrollY > 0) {
            scroll_window(ANIMATION_DURATION, scrollY);
        }

        dispatch("close");
    }

    // Scroll window if widget goes beyond init_doc_scrollHeight
    function scroll_window(duration, scrollY) {
        const start_ts = performance.now();
        let progress_scrollY = 0;

        requestAnimationFrame(function step(current_ts) {
            let progress = (current_ts - start_ts) / duration;

            if (progress >= 1) {
                progress = 1;
            }

            const top = (progress_scrollY - scrollY) * progress;

            window.scrollBy({
                behavior: "smooth",
                top,
            });

            progress_scrollY += -top;

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
    class="slide-details"
    transition:fly|local={{ y: -50, duration: ANIMATION_DURATION }}
    style={`top: ${comp_offset_top}`}
>
    <div class="top-triangle" style={`left: ${triangle_offset_left}`} />

    <div class="details-body">
        <div class="slide">
            <div class="slide-thumb-wrapper">
                <SlideThumb {slide} />
            </div>
        </div>

        <div class="interaction">
            <div class="actions">
                <div
                    class="edit-collection"
                    on:click|preventDefault={update_session_collection}
                >
                    <div class="icon">
                        <FaBookmark />
                    </div>
                    {$session.collection &&
                    $session.collection.findIndex(
                        (sl) => sl.id === slide.id
                    ) !== -1
                        ? "Delete from collection"
                        : "Add to collection"}
                </div>

                <a
                    class="slide-open"
                    target="_blank"
                    href="/api/sm/download/pres-{slide.id}"
                    title="Open original"
                >
                    <div class="icon">
                        <FaFileAlt />
                    </div>
                    Open in GoogleSlides
                </a>

                <a
                    class="slide-download"
                    target="_blank"
                    href="/api/sm/download/pres-{slide.id}"
                    title="Download original"
                >
                    <div class="icon">
                        <FaFilePowerpoint />
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
        on:click|stopPropagation={close_widget}
    >
        <IoMdClose />
    </div>
</div>

{#if posting}
    <div class="fixed-container">
        <Spinner />
    </div>
{/if}

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

    .actions > a,
    .actions > div {
        display: flex;
        justify-content: flex-start;
        align-items: center;
        padding: 0 1rem;
        height: 40px;
        font-size: 0.875rem;
        line-height: 125%;
        color: #fff;
    }

    .actions > a:not(:last-child),
    .actions > div:not(:last-child) {
        margin-bottom: 0.875rem;
    }

    div.edit-collection {
        background-color: #20b2aa;
        text-decoration: underline;
        cursor: pointer;
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

    .fixed-container {
        z-index: 3;
        position: fixed;
        top: 0;
        left: 50%;
        transform: translateX(-50%);
    }
</style>
