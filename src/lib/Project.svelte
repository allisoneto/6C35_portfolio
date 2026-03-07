<script>
    import { base } from '$app/paths';
    export let data = {};
</script>

<article>
    {#if data.url}
        <a href={data.url.startsWith("http") ? data.url : base + data.url}
           style="text-decoration: none; color: inherit;"
           target={data.url.startsWith("http") ? "_blank" : null}>
            <h2>{data.title}</h2>
            <img src={data.image} alt={data.title}>
            <p>{data.description}</p>
        </a>
    {:else}
        <h2>{data.title}</h2>
        <img src={data.image} alt={data.title}>
        <p>{data.description}</p>
    {/if}
</article>

<style>
    h2 {
    font-size: 125%;
    }

    /* Use flex column so each card sizes to its own content instead of subgrid,
       which was forcing equal row heights and creating white space above images */
    article {
    display: flex;
    flex-direction: column;
    margin: 0;
    }
</style>