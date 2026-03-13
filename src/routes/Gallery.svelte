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
                <img
                    src={item.image}
                    alt={item.text}
                    class="gallery-image"
                />
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
            <img
                src={selectedItem.image}
                alt={selectedItem.text}
                class="modal-image"
            />
            <div class="text-secondary">
                {selectedItem.text}
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
        background: rgba(255, 255, 255, 0.08);
        backdrop-filter: blur(12px);
            padding: 1rem;
        cursor: pointer;
        transition: all 0.3s;
    }

    .gallery-item:hover {
        transform: scale(1.05);
        background: rgba(255, 255, 255, 0.12);
    }

    .gallery-image {
        width: 100%;
        height: 10rem;
        border-radius: 0.75rem;
        object-fit: cover;
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
        background: rgba(0, 0, 0, 0.5);
        backdrop-filter: blur(4px);
    }

    .modal-content {
        position: relative;
        max-width: 28rem;
        width: 100%;
        margin: 0 1rem;
        background: rgba(11, 11, 12, 0.9);
        backdrop-filter: blur(12px);
        border-radius: 1rem;
        padding: 1.5rem;
    }

    .modal-image {
        width: 100%;
        height: 16rem;
        border-radius: 0.75rem;
        object-fit: cover;
        margin-bottom: 1rem;
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