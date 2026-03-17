<script lang="ts">
    import { ChevronLeft, ChevronRight } from "lucide-svelte";
    let { data, onSelect } = $props<{ data: any[], onSelect: (item: any, rect: DOMRect, el: HTMLElement) => void }>();

    let scrollContainer: HTMLElement;

    function scrollLeft() {
        if (scrollContainer) {
            scrollContainer.scrollBy({ left: -300, behavior: 'smooth' });
        }
    }

    function scrollRight() {
        if (scrollContainer) {
            scrollContainer.scrollBy({ left: 300, behavior: 'smooth' });
        }
    }

    function jellyClick(node: HTMLElement) {
        function handleClick() {
            node.classList.remove('jelly-click');
            void node.offsetWidth;
            node.classList.add('jelly-click');
        }
        node.addEventListener('click', handleClick);
        return { destroy() { node.removeEventListener('click', handleClick); } };
    }
</script>

<div class="mt-6 relative">
    <button
        onclick={scrollLeft}
        class="absolute left-0 top-1/2 -translate-y-1/2 h-full w-12 z-10 flex items-center justify-center"
        aria-label="Scroll left"
    >
        <div class="rounded-full bg-background/80 backdrop-blur-sm p-2 hover:bg-background/90 transition-colors">
            <ChevronLeft class="w-5 h-5 text-primary" />
        </div>
    </button>
    <div
        bind:this={scrollContainer}
        class="flex gap-4 overflow-x-auto pb-4 hide-scrollbar"
    >
        {#each data as item, i}
            <div
                role="button"
                tabindex="0"
                class="gallery-item"
                data-flip-id={`gallery-${i}`}
                use:jellyClick
                onclick={(e) => { const el = e.currentTarget as HTMLElement; const rect = el.getBoundingClientRect(); setTimeout(() => onSelect(item, rect, el), 180); }}
                onkeydown={(event) => {
                    if (event.key === 'Enter' || event.key === ' ') onSelect(item);
                }}
            >
                {#if item.images}
                    <div class="grid grid-cols-2 gap-1">
                        {#each item.images.slice(0, 4) as img, i}
                            <img
                                src={img}
                                alt={`${item.text} ${i + 1}`}
                                class="gallery-image-small"
                            />
                        {/each}
                    </div>
                {:else}
                    <img
                        src={item.image}
                        alt={item.text}
                        class="gallery-image"
                    />
                {/if}
                <div class="gallery-text">
                    {item.text}
                </div>
            </div>
        {/each}
    </div>
    <button
        onclick={scrollRight}
        class="absolute right-0 top-1/2 -translate-y-1/2 h-full w-12 z-10 flex items-center justify-center"
        aria-label="Scroll right"
    >
        <div class="rounded-full bg-background/80 backdrop-blur-sm p-2 hover:bg-background/90 transition-colors">
            <ChevronRight class="w-5 h-5 text-primary" />
        </div>
    </button>
</div>

<style>
    .gallery-item {
        min-width: 260px;
        max-width: 260px;
        flex-shrink: 0;
        border-radius: 1rem;
        background: rgba(0,0,0,0.10);
        backdrop-filter: blur(5px);
        border: 1px solid rgba(255, 255, 255, 0.2);
        padding: 1rem;
        cursor: pointer;
        transition: background 0.3s;
    }

    .gallery-item:hover {
        background: rgba(255, 255, 255, 0.18);
    }

    .gallery-item:global(.jelly-click) {
        animation: jellyClick 0.45s cubic-bezier(0.34, 1.56, 0.64, 1) forwards;
    }

    @keyframes jellyClick {
        0%   { transform: scale(1); }
        25%  { transform: scale(0.93); }
        50%  { transform: scale(1.04); }
        85%  { transform: scale(0.98); }
        100% { transform: scale(1); }
    }

    .gallery-image {
        width: 100%;
        height: 15rem;
        border-radius: 0.75rem;
        object-fit: cover;
    }

    .gallery-image-small {
        width: 100%;
        height: 120px;
        object-fit: cover;
        border-radius: 0.5rem;
    }

    .gallery-text {
        margin-top: 1rem;
        font-size: 0.875rem;
        color: var(--color-secondary);
    }
</style>
