<script lang="ts">
    let { data } = $props();
</script>

<div id="experienceSection" class="flex flex-col justify-start w-full mt-6 scroll-mt-24">
    <!-- <div class="text-faint text-xs font-extrabold uppercase">Experience</div> -->
    <div class="flex flex-col items-start justify-center">
        {#each data as experience, index}
            <div class="flex w-full">
                <div class="flex flex-col items-center shrink-0">
                    <div class="w-3 h-3 rounded-full bg-primary shrink-0"></div>
                    {#if index !== data.length - 1}
                        <div class="timeline-line grow"></div>
                    {/if}
                </div>
                <div class="flex ml-3 flex-col grow pb-14">
                    <div class="flex justify-between items-center w-full h-12">
                        <div class="flex flex-col justify-start items-start">
                        <div class="company-name">
                            {experience.company}
                        </div>
                        <div class="role-text">
                            {experience.role}
                        </div>
                        </div>
                        <div class="duration-text flex items-center">
                        {#if experience.duration === "Present"}
                            <div class="pulse-dot w-1.5 h-1.5 rounded-full mr-2"></div>
                        {/if}
                        {experience.duration}
                        </div>
                    </div>
                    <div class="experience-description">
                        {experience.description}
                    </div>
                </div>
            </div>
        {/each}
    </div>
</div>

<style>
    .timeline-line {
        width: 2px;
        background-color: var(--color-background-tertiary);
    }

    .company-name {
        color: var(--color-primary);
        font-size: 1.25rem;
        font-weight: 600;
    }

    .role-text {
        color: var(--color-title);
        font-size: 0.875rem;
    }

    .duration-text {
        color: #706767;
        font-size: 0.875rem;
    }

    .pulse-dot {
        background-color: #16a34a;
        border-color: #15803d;
        animation: pulse 2s cubic-bezier(0.4, 0, 0.6, 1) infinite;
    }

    .experience-description {
        color: var(--color-primary);
        font-size: 0.9rem;
        line-height: 1.5;
        margin-top: 0.75rem;
    }
</style>