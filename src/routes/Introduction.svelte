<script lang="ts">


    let { data } = $props();

</script>


<!--

    Coastal UI Design Basics:

    Main padding: 4
    Smaller alert / etc padding: 2
    border color: #222222
    card bg color: #0a0a0a
    rounded: lg

-->

<section class="w-full">
    <div class="text-primary text-5xl sm:text-6xl font-semibold tracking-tight">
        {data.name}
    </div>

    <div id="aboutSection" class="mt-10 max-w-[70ch]">
        <div class="text-faint text-xs font-extrabold uppercase">About</div>
        <div class="text-primary mt-3 text-lg leading-relaxed">{data.description}</div>
    </div>
</section>