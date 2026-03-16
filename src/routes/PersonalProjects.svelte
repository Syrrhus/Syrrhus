<script>
	import { ExternalLinkIcon, X } from "lucide-svelte";
    let { data } = $props();

    let selectedProject = $state(null);

    function openModal(project) {
        selectedProject = project;
    }

    function closeModal() {
        selectedProject = null;
    }
</script>

<div class="flex flex-col justify-start w-full mt-6 scroll-mt-24">
    <div class="grid items-start justify-center grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4">
        {#each data as project}
            <div 
                class="project-card"
                role="button"
                tabindex="0"
                onclick={() => openModal(project)}
                onkeydown={(event) => {
                    if (event.key === 'Enter' || event.key === ' ') openModal(project);
                }}
            >
                <img 
                    src={project.image}
                    alt={`${project.name} thumbnail`}
                    class="project-image"
                />   
                <div class="flex flex-col h-full mt-3">
                    <div class="flex justify-between items-start gap-2">
                        <div class="project-name">{project.name}</div>
                        <div class="project-tech">
                            {project.technology}
                        </div>
                    </div>
                    <div class="project-description">
                        {project.description}
                    </div>
                </div>
            </div>
        {/each}
    </div>
</div>

{#if selectedProject}
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
                src={selectedProject.image}
                alt={selectedProject.name}
                class="modal-image"
            />
            <div class="text-secondary text-center mt-4">
                <h3 class="text-lg font-semibold mb-2">{selectedProject.name}</h3>
                <p class="text-xs text-faint mb-2">{selectedProject.technology}</p>
                <p class="text-sm leading-relaxed mb-4 whitespace-pre-line">{selectedProject.modalDescription || selectedProject.description}</p>
                <a 
                    href={selectedProject.link} 
                    target="_blank" 
                    rel="noopener noreferrer"
                    class="inline-flex items-center gap-2 px-4 py-2 bg-primary/20 hover:bg-primary/30 rounded-full text-sm font-medium transition-colors"
                >
                    View Project
                    <ExternalLinkIcon class="h-4 w-4" />
                </a>
            </div>
        </div>
    </div>
{/if}

<style>
    /* Styles for project cards */
    .project-card {
        padding: 0.75rem;
        display: flex;
        flex-direction: column;
        background: rgba(0,0,0,0.10);
        backdrop-filter: blur(5px);
        border-radius: 1rem;
        cursor: pointer;
        transition: all 0.3s;
    }

    .project-card:hover {
        background: rgba(255, 255, 255, 0.18);
    }

    .project-image {
        width: 100%;
        aspect-ratio: 16 / 9;
        border-radius: 0.75rem;
        object-fit: cover;
    }

    .project-name {
        font-size: 1.25rem;
        font-weight: 600;
        color: var(--color-primary);
    }

    .project-tech {
        font-size: 0.75rem;
        color: var(--color-faint);
        font-weight: 500;
        text-align: right;
    }

    .project-description {
        color: var(--color-secondary);
        margin-top: 1rem;
        font-size: 0.875rem;
        line-height: 1.625;
    }

    .project-link {
        margin-top: 1.25rem;
        display: inline-flex;
        align-items: center;
        justify-content: center;
        gap: 0.5rem;
        border-radius: 9999px;
        background: rgba(255, 255, 255, 0.1);
        padding: 0.5rem 1rem;
        font-size: 0.75rem;
        font-weight: 600;
        color: var(--color-primary);
        transition: background-color 0.2s;
    }

    .project-link:hover {
        background: rgba(255, 255, 255, 0.15);
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
        max-width: 40rem;
        width: 100%;
        margin: 0 1rem;
        background: rgba(11, 11, 12, 0.95);
        backdrop-filter: blur(12px);
        border-radius: 1rem;
        padding: 1.5rem;
    }

    .modal-image {
        width: 100%;
        max-height: 60vh;
        border-radius: 0.75rem;
        object-fit: contain;
        margin-bottom: 1rem;
    }

    .close-button {
        position: absolute;
        top: 1rem;
        right: 1rem;
        color: var(--color-secondary);
        background: none;
        border: none;
        cursor: pointer;
    }
</style>