<script lang="ts">
    import { base } from '$app/paths';
    let { data } = $props();
</script>

<h2 class="text-2xl text-title font-semibold mb-6">{data.title}</h2>
<div class="bg-[rgba(0,0,0,0.1)] backdrop-blur-sm rounded-2xl p-6" style="border: 1px solid rgba(255, 255, 255, 0.2)">
    <div class="flex flex-col md:flex-row items-center gap-6">
        <div class="flex-shrink-0">
            <img
                src="{base}/portraitface.JPG"
                alt="Profile picture"
                class="w-48 h-48 rounded-full object-cover border-2 border-primary/20"
            />
        </div>
        <div class="flex-1">
            <p class="text-secondary leading-relaxed text-sm sm:text-lg whitespace-pre-line">
                {data.description}
            </p>
        </div>
    </div>
</div>
