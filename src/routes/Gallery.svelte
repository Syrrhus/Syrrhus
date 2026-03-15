<script lang="ts">
    import { ChevronLeft, ChevronRight, X } from "lucide-svelte";
    let { data } = $props();

    let scrollContainer: HTMLElement;
    let selectedItem = $state(null);

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

    function openModal(item: any) {
        selectedItem = item;
    }

    function closeModal() {
        selectedItem = null;
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
        {#each data as item}
            <div
                role="button"
                tabindex="0"
                class="gallery-item"
                onclick={() => openModal(item)}
                onkeydown={(event) => {
                    if (event.key === 'Enter' || event.key === ' ') openModal(item);
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

{#if selectedItem}
    <div
        class="modal-backdrop"
        onclick={closeModal}
        onkeydown={(event) => {
            if (event.key === 'Escape') closeModal();
        }}
    >
        <div
            role="dialog"
            tabindex="-1"
            class="modal-content"
            onpointerdown={(e) => e.stopPropagation()}
        >
            <button
                onclick={closeModal}
                class="close-button"
                aria-label="Close modal"
            >
                <X class="w-6 h-6" />
            </button>
            {#if selectedItem.images}
                <div class="grid grid-cols-1 md:grid-cols-3 gap-4 mb-4">
                    {#each selectedItem.images as img, i}
                        <img
                            src={img}
                            alt={`${selectedItem.text} ${i + 1}`}
                            class="modal-image-small"
                        />
                    {/each}
                </div>
            {:else}
                <img
                    src={selectedItem.image}
                    alt={selectedItem.text}
                    class="modal-image"
                />
            {/if}
            <div class="text-secondary text-center mt-4">
                <h3 class="text-lg font-semibold mb-2">{selectedItem.text}</h3>
                <p class="text-sm leading-relaxed">{selectedItem.description}</p>
            </div>
        </div>
    </div>
{/if}

<style>
    /* Styles for gallery items */
    .gallery-item {
        min-width: 260px;
        max-width: 260px;
        flex-shrink: 0;
        border-radius: 1rem;
        background: rgba(0,0,0,0.10);
        backdrop-filter: blur(5px);
            padding: 1rem;
        cursor: pointer;
        transition: all 0.3s;
    }

    .gallery-item:hover {
        background: rgba(255, 255, 255, 0.18);
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

    /* Modal styles */
    .modal-backdrop {
        position: fixed;
        inset: 0;
        z-index: 50;
        display: flex;
        align-items: center;
        justify-content: center;
        background: rgba(0, 0, 0, 0.7);
        backdrop-filter: blur(4px);
    }

    .modal-content {
        position: relative;
        max-width: 50rem;
        width: 100%;
        margin: 0 1rem;
        background: rgba(11, 11, 12, 0.95);
        backdrop-filter: blur(12px);
        border-radius: 1rem;
        padding: 1.5rem;
    }

    .modal-image {
        width: 100%;
        max-height: 70vh;
        border-radius: 0.75rem;
        object-fit: contain;
        margin-bottom: 1rem;
    }

    .modal-image-small {
        width: 100%;
        height: 250px;
        object-fit: cover;
        border-radius: 0.5rem;
    }

    .close-button {
        position: absolute;
        top: 1rem;
        right: 1rem;
        color: var(--color-secondary);
        transition: color 0.2s;
    }

    .close-button:hover {
        color: var(--color-primary);
    }
</style>