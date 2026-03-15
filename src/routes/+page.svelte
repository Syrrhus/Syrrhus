<script lang="ts">
	import Introduction from './Introduction.svelte';
	import About from './About.svelte';
	import Experience from './Experience.svelte';
	import PersonalProjects from './PersonalProjects.svelte';
	import Gallery from './Gallery.svelte';
    import { portfolioContent } from '$lib/portfolioData';
</script>

<div class="w-full overflow-x-hidden bg-background">
    <div
        class="relative w-full"
        style="background-image: url('/Cloud-upscaled.png'); background-size: cover; background-position: center; background-repeat: no-repeat;"
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

        <div class="pb-12">
            <section id="aboutSection" class="relative py-12 px-6 sm:px-10">
                <div class="mx-auto max-w-[1100px]">
                    <About data={portfolioContent.about} />
                </div>
            </section>

            <section id="experienceSection" class="relative pt-8 pb-12 px-6 sm:px-10">
                <div class="mx-auto max-w-[1100px]">
                    <h2 class="text-2xl text-title font-semibold mb-6">Work Experience</h2>
                    <div class="bg-[rgba(0,0,0,0.10)] backdrop-blur-sm rounded-2xl p-4">
                        <Experience data={portfolioContent.experiences} />
                    </div>
                </div>
            </section>

            <section id="projectsSection" class="relative py-12 px-6 sm:px-10">
                <div class="mx-auto max-w-[1100px]">
                    <h2 class="text-2xl text-title-medium font-semibold mb-6">Projects</h2>
                    <PersonalProjects data={portfolioContent.projects} />
                </div>
            </section>

            <section id="gallerySection" class="relative py-12 px-6 sm:px-10">
                <div class="mx-auto max-w-[1100px]">
                    <h2 class="text-2xl text-title-light font-semibold mb-6">Art</h2>
                    <Gallery data={portfolioContent.gallery} />
                </div>
            </section>
        </div>
    </div>

    <footer id="contactSection" class="relative py-12 px-6 sm:px-10 bg-background border-t border-background-tertiary">
        <div class="mx-auto max-w-[1100px] flex justify-between items-center gap-16">
            <div class="space-y-2">
                <div class="text-2xl font-semibold text-primary">S.H.J</div>
                <!-- <div class="text-sm text-secondary">Sean Ho Junxian</div> -->
            </div>

            <div class="flex gap-16">
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