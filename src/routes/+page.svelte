<script lang="ts">
	import Introduction from './Introduction.svelte';
	import About from './About.svelte';
	import Experience from './Experience.svelte';
	import PersonalProjects from './PersonalProjects.svelte';
	import Gallery from './Gallery.svelte';
    import { ExternalLinkIcon, X } from 'lucide-svelte';
    import { portfolioContent } from '$lib/portfolioData';
    import { onMount, tick } from 'svelte';

    let gsap: any;

    let sectionsEl: HTMLElement;
    let selectedProject = $state<any>(null);
    let selectedGalleryItem = $state<any>(null);

    // Modal element refs
    let projectModalEl: HTMLElement;
    let projectBackdropEl: HTMLElement;
    let galleryModalEl: HTMLElement;
    let galleryBackdropEl: HTMLElement;

    // Origin card element refs (restored on close)
    let projectCardEl: HTMLElement | null = null;
    let galleryCardEl: HTMLElement | null = null;

    // Scroll stretch
    onMount(async () => {
        gsap = (await import('gsap')).gsap;

        const MAX_STRETCH = 1.1; // ← edit this to control max squish (1.0 = none, 1.2 = heavy)

        let currentScale = 1;
        let targetScale = 1;
        let scrollDir = 'down';
        let lastScrollY = window.scrollY;
        let lastTime = performance.now();
        let rafId: number;
        let lastOrigin = '';

        // Cache children once and promote to compositor layers
        const sectionChildren = Array.from(sectionsEl.children) as HTMLElement[];
        for (const child of sectionChildren) {
            child.style.willChange = 'transform';
        }

        function scrollTick() {
            currentScale += (targetScale - currentScale) * 0.16;
            targetScale += (1 - targetScale) * 0.35;
            const origin = scrollDir === 'down' ? 'bottom center' : 'top center';
            const transform = `scaleY(${currentScale})`;
            const originChanged = origin !== lastOrigin;
            if (originChanged) lastOrigin = origin;
            for (const child of sectionChildren) {
                child.style.transform = transform;
                if (originChanged) child.style.transformOrigin = origin;
            }
            if (Math.abs(currentScale - 1) > 0.0002 || Math.abs(targetScale - 1) > 0.0002) {
                rafId = requestAnimationFrame(scrollTick);
            }
        }

        function onScroll() {
            const now = performance.now();
            const dy = window.scrollY - lastScrollY;
            const dt = Math.max(now - lastTime, 1);
            targetScale = 1 + Math.min(Math.abs(dy / dt) * 0.06, MAX_STRETCH - 1);
            scrollDir = dy >= 0 ? 'down' : 'up';
            document.body.dataset.scrollDir = scrollDir;
            lastScrollY = window.scrollY;
            lastTime = now;
            cancelAnimationFrame(rafId);
            rafId = requestAnimationFrame(scrollTick);
        }

        window.addEventListener('scroll', onScroll, { passive: true });
        return () => {
            window.removeEventListener('scroll', onScroll);
            cancelAnimationFrame(rafId);
            for (const child of sectionChildren) {
                child.style.willChange = '';
            }
        };
    });

    async function openProject(project: any, rect: DOMRect, cardEl: HTMLElement) {
        if (!gsap) return;
        projectCardEl = cardEl;
        selectedProject = project;
        await tick();

        const modalRect = projectModalEl.getBoundingClientRect();
        const dx = (rect.left + rect.width / 2) - (modalRect.left + modalRect.width / 2);
        const dy = (rect.top + rect.height / 2) - (modalRect.top + modalRect.height / 2);

        gsap.to(projectBackdropEl, { opacity: 1, duration: 0.35, ease: 'power2.out' });
        gsap.fromTo(projectModalEl,
            { x: dx, y: dy, scaleX: rect.width / modalRect.width, scaleY: rect.height / modalRect.height, transformOrigin: 'center center' },
            { x: 0, y: 0, scaleX: 1, scaleY: 1, duration: 0.5, ease: 'power3.out', clearProps: 'transform' }
        );
    }

    function closeProject() {
        const cardEl = projectCardEl;
        projectCardEl = null;
        if (!gsap || !cardEl) {
            selectedProject = null;
            return;
        }

        const modalRect = projectModalEl.getBoundingClientRect();
        const cardRect = cardEl.getBoundingClientRect();
        const dx = (cardRect.left + cardRect.width / 2) - (modalRect.left + modalRect.width / 2);
        const dy = (cardRect.top + cardRect.height / 2) - (modalRect.top + modalRect.height / 2);

        gsap.to(projectBackdropEl, { opacity: 0, duration: 0.3, ease: 'power2.in' });
        gsap.to(projectModalEl, {
            x: dx, y: dy,
            scaleX: cardRect.width / modalRect.width,
            scaleY: cardRect.height / modalRect.height,
            duration: 0.38, ease: 'power3.in',
            onComplete: () => {
                selectedProject = null;
            }
        });
    }

    async function openGalleryItem(item: any, rect: DOMRect, cardEl: HTMLElement) {
        if (!gsap) return;
        galleryCardEl = cardEl;
        selectedGalleryItem = item;
        await tick();

        const modalRect = galleryModalEl.getBoundingClientRect();
        const dx = (rect.left + rect.width / 2) - (modalRect.left + modalRect.width / 2);
        const dy = (rect.top + rect.height / 2) - (modalRect.top + modalRect.height / 2);

        gsap.to(galleryBackdropEl, { opacity: 1, duration: 0.35, ease: 'power2.out' });
        gsap.fromTo(galleryModalEl,
            { x: dx, y: dy, scaleX: rect.width / modalRect.width, scaleY: rect.height / modalRect.height, transformOrigin: 'center center' },
            { x: 0, y: 0, scaleX: 1, scaleY: 1, duration: 0.5, ease: 'power3.out', clearProps: 'transform' }
        );
    }

    function closeGalleryItem() {
        const cardEl = galleryCardEl;
        galleryCardEl = null;
        if (!gsap || !cardEl) {
            selectedGalleryItem = null;
            return;
        }

        const modalRect = galleryModalEl.getBoundingClientRect();
        const cardRect = cardEl.getBoundingClientRect();
        const dx = (cardRect.left + cardRect.width / 2) - (modalRect.left + modalRect.width / 2);
        const dy = (cardRect.top + cardRect.height / 2) - (modalRect.top + modalRect.height / 2);

        gsap.to(galleryBackdropEl, { opacity: 0, duration: 0.3, ease: 'power2.in' });
        gsap.to(galleryModalEl, {
            x: dx, y: dy,
            scaleX: cardRect.width / modalRect.width,
            scaleY: cardRect.height / modalRect.height,
            duration: 0.38, ease: 'power3.in',
            onComplete: () => {
                selectedGalleryItem = null;
            }
        });
    }
</script>

<div class="w-full overflow-x-hidden bg-background">
    <div
        class="relative w-full"
        style="background-image: url('/Cloud-nosig.PNG'); background-size: cover; background-position: center; background-repeat: no-repeat;"
    >
        <div class="absolute inset-0 bg-background/20"></div>

        <nav class="fixed inset-x-0 top-0 z-30">
            <div class="mx-auto w-full max-w-[1100px] px-6 sm:px-10 h-14 flex items-center justify-between">
                <a href="#top" class="text-primary font-semibold tracking-tight text-title">S.H.J</a>
                <div class="flex items-center gap-6 text-sm text-secondary">
                    <a class="hover:text-primary" href="#aboutSection">About</a>
                    <a class="hover:text-primary" href="#experienceSection">Experience</a>
                    <a class="hover:text-primary" href="#projectsSection">Projects</a>
                    <a class="hover:text-primary" href="#gallerySection">Art</a>
                    <a class="hover:text-primary" href="#contactSection">Contact</a>
                </div>
            </div>
        </nav>

        <div class="relative z-10 min-h-screen flex items-center px-6 sm:px-10">
            <div class="mx-auto w-full max-w-[1100px]">
                <Introduction data={portfolioContent.introduction} />
            </div>
        </div>

        <div class="pb-12" bind:this={sectionsEl}>
            <section id="aboutSection" class="relative py-12 px-6 sm:px-10">
                <div class="mx-auto max-w-[1100px]">
                    <About data={portfolioContent.about} />
                </div>
            </section>

            <section id="experienceSection" class="relative pt-8 pb-12 px-6 sm:px-10">
                <div class="mx-auto max-w-[1100px]">
                    <h2 class="text-2xl text-title font-semibold mb-6">Work Experience</h2>
                    <div class="rounded-2xl p-4 liquid-glass">
                        <Experience data={portfolioContent.experiences} />
                    </div>
                </div>
            </section>

            <section id="projectsSection" class="relative py-12 px-6 sm:px-10">
                <div class="mx-auto max-w-[1100px]">
                    <h2 class="text-2xl text-title-medium font-semibold mb-6">Projects</h2>
                    <PersonalProjects data={portfolioContent.projects} onSelect={openProject} />
                </div>
            </section>

            <section id="gallerySection" class="relative py-12 px-6 sm:px-10">
                <div class="mx-auto max-w-[1100px]">
                    <h2 class="text-2xl text-title-light font-semibold mb-6">Art</h2>
                    <Gallery data={portfolioContent.gallery} onSelect={openGalleryItem} />
                </div>
            </section>
        </div>
        <img src="/Signature.PNG" alt="Signature" class="absolute bottom-2 right-2 sm:bottom-6 sm:right-10 h-15 sm:h-20 opacity-60 pointer-events-none" />
    </div>

    <footer id="contactSection" class="relative py-12 px-6 sm:px-10 bg-background border-t border-background-tertiary">
        <div class="mx-auto max-w-[1100px] flex justify-between items-center gap-16">
            <div class="space-y-2">
                <div class="text-2xl font-semibold text-primary">S.H.J</div>
            </div>

            <div class="flex gap-16 sm:gap-50">
                <div>
                    <div class="text-xs text-title uppercase tracking-wide">Menu</div>
                    <div class="mt-4 flex flex-col gap-2 text-primary text-sm">
                        <a href="#aboutSection" class="hover:text-secondary">About</a>
                        <a href="#experienceSection" class="hover:text-secondary">Experience</a>
                        <a href="#projectsSection" class="hover:text-secondary">Projects</a>
                        <a href="#gallerySection" class="hover:text-secondary">Art</a>
                        <a href="#contactSection" class="hover:text-secondary">Contact</a>
                    </div>
                </div>

                <div>
                    <div class="text-xs text-title uppercase tracking-wide">Contact</div>
                    <div class="mt-4 flex flex-col gap-2 text-primary text-sm">
                        <a href={portfolioContent.introduction.linkedInSocialLink} class="hover:text-secondary">LinkedIn</a>
                        <a href="https://github.com" class="hover:text-secondary">Github</a>
                        <a href={portfolioContent.introduction.emailAddress} class="hover:text-secondary">Email</a>
                    </div>
                </div>
            </div>
        </div>
        <div class="mt-10 text-center text-xs text-faint">
            &copy; 2026 S.H.J
        </div>
    </footer>
</div>

<!-- Modals rendered outside transformed container -->
{#if selectedProject}
    <div
        role="presentation"
        class="modal-backdrop"
        bind:this={projectBackdropEl}
        onclick={closeProject}
        onkeydown={(e) => { if (e.key === 'Escape') closeProject(); }}
        style="opacity: 0"
    >
        <div
            role="dialog"
            tabindex="-1"
            class="modal-content"
            bind:this={projectModalEl}
            onpointerdown={(e) => e.stopPropagation()}
        >
            <button onclick={closeProject} class="close-button" aria-label="Close modal">
                <X class="w-6 h-6" />
            </button>
            <img src={selectedProject.image} alt={selectedProject.name} class="modal-image" />
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

{#if selectedGalleryItem}
    <div
        role="presentation"
        class="modal-backdrop gallery-modal-backdrop"
        bind:this={galleryBackdropEl}
        onclick={closeGalleryItem}
        onkeydown={(e) => { if (e.key === 'Escape') closeGalleryItem(); }}
        style="opacity: 0"
    >
        <div
            role="dialog"
            tabindex="-1"
            class="modal-content gallery-modal-content"
            bind:this={galleryModalEl}
            onpointerdown={(e) => e.stopPropagation()}
        >
            <button onclick={closeGalleryItem} class="close-button" aria-label="Close modal">
                <X class="w-6 h-6" />
            </button>
            {#if selectedGalleryItem.images}
                <div class="grid grid-cols-1 md:grid-cols-3 gap-4 mb-4">
                    {#each selectedGalleryItem.images as img, i}
                        <img src={img} alt={`${selectedGalleryItem.text} ${i + 1}`} class="modal-image-small" />
                    {/each}
                </div>
            {:else}
                <img src={selectedGalleryItem.image} alt={selectedGalleryItem.text} class="modal-image" />
            {/if}
            <div class="text-secondary text-center mt-4">
                <h3 class="text-lg font-semibold mb-2">{selectedGalleryItem.text}</h3>
                <p class="text-sm leading-relaxed">{selectedGalleryItem.description}</p>
            </div>
        </div>
    </div>
{/if}

<style>
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

    .gallery-modal-backdrop {
        align-items: flex-start;
        overflow-y: auto;
        padding: 1rem 0;
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

    .gallery-modal-content {
        max-width: 50rem;
        margin: auto 1rem;
    }

    .modal-image {
        width: 100%;
        max-height: 60vh;
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
        background: none;
        border: none;
        cursor: pointer;
        transition: color 0.2s;
    }

    .close-button:hover {
        color: var(--color-primary);
    }

    :global(.liquid-glass) {
        background: linear-gradient(135deg, rgba(0, 0, 0, 0.1) 0%, rgba(0, 0, 0, 0.25) 100%);
        backdrop-filter: blur(5px) saturate(120%);
        border: 1px solid rgba(255, 255, 255, 0.22);
        box-shadow:
            inset 0 1px 0 rgba(255,255,255,0.28),
            inset 0 -3px 0 rgba(0,0,0,0.12),
            inset 2px 0 0 rgba(255,255,255,0.08),
            0 6px 15px rgba(0,0,0,0.35);
    }
</style>
