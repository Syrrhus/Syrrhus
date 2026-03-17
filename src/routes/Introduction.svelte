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

<section id="top" class="w-full">
    <div class="hero-name">
        {data.displayName}
    </div>

    <div class="mt-4 text-secondary text-base sm:text-lg">
        {data.subtitle}
    </div>
</section>

<style>
    .hero-name {
        color: var(--color-primary);
        /* font-family: var(--font-serif); */
        font-weight: 100;
        text-transform: uppercase;
        letter-spacing: 0.0em;
        white-space: pre-line;
        font-size: clamp(4rem, 7vw, 6.5rem);
        line-height: 1.0;
    }
</style>