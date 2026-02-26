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