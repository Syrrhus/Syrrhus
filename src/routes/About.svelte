<script lang="ts">
    let { data } = $props();
</script>

<section class="w-full pt-16 px-6 sm:px-10">
    <div class="mx-auto w-full max-w-[1100px]">
        <h2 class="text-2xl text-title font-semibold mb-6">{data.title}</h2>
        <div class="bg-[rgba(0,0,0,0.1)] backdrop-blur-sm rounded-2xl p-6">
            <div class="flex flex-col md:flex-row items-center gap-6">
                <div class="flex-shrink-0">
                    <img
                        src="/portraitface.JPG"
                        alt="Profile picture"
                        class="w-60 h-60 rounded-full object-cover border-2 border-primary/20"
                    />
                </div>
                <div class="flex-1">
                    <p class="text-secondary leading-relaxed text-lg whitespace-pre-line">
                        {data.description}
                    </p>
                </div>
            </div>
        </div>
    </div>
</section>