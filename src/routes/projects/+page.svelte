<script>
  import projects from "$lib/projects.json";
  import Project from "$lib/Project.svelte";
  import ProjectNarrative from "$lib/ProjectNarrative.svelte";
  let years = projects.map(proj => proj.year)
  let validYears = years.filter(year => typeof year === "number");
  let range = validYears.length > 0 ? Math.max(...validYears) - Math.min(...validYears) + 1 : 0;
  
</script>

<svelte:head>
  <title>Projects</title>
</svelte:head>

<h1>{projects.length} Projects over {range} Years</h1>


<p>Scroll down to see my a timeline of my projects and how they've contributed to my professional and personal life</p>

<ProjectNarrative />

<p class="outro">Thanks for scrolling through my project story! Feel free to explore all of the projects at your leisure below.</p>


<h1>{projects.length} Projects</h1>

<div class="projects">
    {#each projects as p}
    <Project data={p} />
    {/each}
</div>