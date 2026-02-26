<script>
    export let data = {};
</script>

<article>
    <img src={data.image} alt={data.title}>
    <div>
        <strong>{data.title}</strong>
        <span>{data.author}</span>
    </div>
</article>

<style>
    article {
        display: flex;
        gap: 1em;
        align-items: flex-start;
    }

    img {
        width: 5em;
        min-width: 5em;
        object-fit: cover;
    }

    div {
        display: flex;
        flex-direction: column;
        gap: 0.25em;
    }

    span {
        font-size: 0.9em;
    }
</style>