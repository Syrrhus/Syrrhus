<script>
	import { ExternalLinkIcon } from "lucide-svelte";
    let { data } = $props();
</script>

<div class="flex flex-col justify-start w-full mt-6 scroll-mt-24">
    <div class="grid items-start justify-center grid-cols-1 gap-6 sm:grid-cols-2">
        {#each data as project}
            <div class="project-card">
                <img 
                    src={project.image}
                    alt={`${project.name} thumbnail`}
                    class="project-image"
                />   
                <div class="flex flex-col h-full mt-5">
                    <div class="flex justify-between items-start gap-4">
                        <div class="project-name">{project.name}</div>
                        <div class="project-tech">
                            {project.technology}
                        </div>
                    </div>
                    <div class="project-description">
                        {project.description}
                    </div>
                    <a 
                        href={project.link} 
                        target="_blank" 
                        rel="noopener noreferrer"
                        class="project-link"
                    >
                        View project
                        <ExternalLinkIcon class="h-4 w-4" strokeWidth={1.5}/>
                    </a>
                </div>
            </div>
        {/each}
    </div>
</div>

<style>
    /* Styles for project cards */
    .project-card {
        padding: 1.5rem;
        display: flex;
        flex-direction: column;
        background: rgba(255, 255, 255, 0.08);
        backdrop-filter: blur(12px);
        border-radius: 1rem;
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
</style>