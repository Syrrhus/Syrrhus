<script lang="ts">
	let { data, onSelect } = $props<{ data: any[], onSelect: (p: any, rect: DOMRect, el: HTMLElement) => void }>();

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

<div class="flex flex-col justify-start w-full mt-6 scroll-mt-24">
    <div class="grid items-start justify-center grid-cols-2 lg:grid-cols-3 gap-2 sm:gap-4">
        {#each data as project, i}
            <div
                class="project-card"
                role="button"
                tabindex="0"
                data-flip-id={`project-${i}`}
                use:jellyClick
                onclick={(e) => { const el = e.currentTarget as HTMLElement; const rect = el.getBoundingClientRect(); setTimeout(() => onSelect(project, rect, el), 180); }}
                onkeydown={(event) => {
                    if ((event as KeyboardEvent).key === 'Enter' || (event as KeyboardEvent).key === ' ') onSelect(project);
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

<style>
    .project-card {
        padding: 0.75rem;
        display: flex;
        flex-direction: column;
        background: linear-gradient(135deg, rgba(0, 0, 0, 0.21) 0%, rgba(255,255,255,0.04) 100%);
        backdrop-filter: blur(5px) saturate(150%);
        border-radius: 1rem;
        border: 1px solid rgba(255, 255, 255, 0.22);
        box-shadow:
            inset 0 1px 0 rgba(255,255,255,0.28),
            inset 0 -3px 0 rgba(0,0,0,0.12),
            inset 2px 0 0 rgba(255,255,255,0.08),
            0 6px 15px rgba(0,0,0,0.35);
        cursor: pointer;
        transition: background 0.2s, box-shadow 0.2s, transform 0.2s, filter 0.2s;
    }

    .project-card:hover {
        background: linear-gradient(135deg, rgba(20, 20, 20, 0.35) 0%, rgba(255,255,255,0.08) 100%);
        box-shadow:
            inset 0 1px 0 rgba(255,255,255,0.38),
            inset 0 -3px 0 rgba(0,0,0,0.1),
            inset 2px 0 0 rgba(255,255,255,0.12),
            0 12px 28px rgba(0,0,0,0.5);
        transform: translateY(-3px) scale(1.015);
        filter: brightness(1.08);
    }

    .project-card:active {
        transform: translateY(0) scale(0.97);
        filter: brightness(0.93);
        transition: transform 0.08s, filter 0.08s;
    }

    .project-card:global(.jelly-click) {
        animation: jellyClick 0.45s cubic-bezier(0.34, 1.56, 0.64, 1) forwards;
    }

    @keyframes jellyClick {
        0%   { transform: scale(1); }
        25%  { transform: scale(0.93); }
        50%  { transform: scale(1.04); }
        85%  { transform: scale(0.98); }
        100% { transform: scale(1); }
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

    @media (max-width: 639px) {
        .project-card {
            padding: 0.5rem;
        }

        .project-name {
            font-size: 0.875rem;
        }

        .project-tech {
            font-size: 0.625rem;
        }

        .project-description {
            font-size: 0.75rem;
            margin-top: 0.5rem;
            line-height: 1.4;
        }
    }
</style>
