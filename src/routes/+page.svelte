<script>
    import projects from "$lib/projects.json";
    import Project from "$lib/Project.svelte";
    import reading from "$lib/reading.json";
    import ReadingItem from "$lib/ReadingItem.svelte";
  </script>

<nav>
    <ul>
        <li><a href="." class="current">Home</a></li>
        <li><a href="projects">Projects</a></li>
        <li><a href="resume">About</a></li>
        <li><a href="contact">Contact</a></li>
        <li><a href="https://github.com/allisoneto" target="_blank">GitHub</a></li>
    </ul>
</nav>

<h1>Allison Eto's 6.C35 Portfolio</h1>

<div class="intro-and-reading">
    <div class="intro">
        <p>This is the beginning of Allison Eto's personal portfolio for MIT's 6.C35 course - Interactive Data Visualization and Society.
            Completed assignments and visualizations will be posted here, as well as some information about Allison.
        </p>

        <p>It is currently under development.</p>

        <img src="images\allison_w_snowman.jpeg" alt="Allison kneeling next to a small snowman" width="500">
    </div>

    <aside class="reading-sidebar">
        <h2>What I'm Reading</h2>
        <div class="reading">
            {#each reading.slice(0, 3) as r}
            <ReadingItem data={r} />
            {/each}
        </div>
    </aside>
</div>

<h2>Recent Projects</h2>
<div class="projects highlights">
    {#each projects.slice(0, 3) as p}
    <Project data={p} />
    {/each}
</div>
