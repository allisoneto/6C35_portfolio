<script>
    export let data = {};
</script>

<article>
    <!-- <a href="./a2_report.html" style="text-decoration: none; color: inherit;"> -->
    <h2>{data.title}</h2>
    <img src={data.image} alt={data.title}>
    <p>{data.description}</p>
    <!-- </a> -->
</article>

<style>
    h2 {
    font-size: 125%;
    }

    article {
    display: grid;
    grid-template-rows: subgrid;
    grid-row: span 3;
    margin: 0;
    }
</style>