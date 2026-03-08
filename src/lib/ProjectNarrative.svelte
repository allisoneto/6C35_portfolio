<script>
    import Scrolly from "svelte-scrolly";
    let scrollyProgress = 0

    import projects from "$lib/projects.json";
    let projects_with_year = projects.filter(project => typeof project.year === "number");
    projects_with_year.sort((a, b) => a.year - b.year);
    let projects_without_year = projects.filter(project => typeof project.year !== "number");

    let progressPerProject = 100 / (projects_with_year.length + projects_without_year.length);
    
    let sorted_projects = [...projects_with_year, ...projects_without_year];

    $: activeProjectIdx = Math.min(sorted_projects.length - 1, Math.floor(scrollyProgress / progressPerProject));

</script>

<div class="scrolly-wrapper">
    <Scrolly bind:progress={ scrollyProgress }>
        <!-- Story here -->
         <section class="step">
            {#each sorted_projects as project}
                <div class="step-content">
                    <h3>{project.title}</h3>
                    <!-- <p>{project.description}</p> -->
                     <p>{project.story}</p>
                </div>
            {/each}
         </section>
        <svelte:fragment slot="viz">
            <!-- Visualizations here (these will stay sticky) -->
            <div class="project-detail">
                <h3>{sorted_projects[activeProjectIdx].year}</h3>
                <p>{sorted_projects[activeProjectIdx].description}</p>
                <img src={sorted_projects[activeProjectIdx].image} alt={sorted_projects[activeProjectIdx].title}>
            </div>
        </svelte:fragment>
    </Scrolly>
</div>

<style>
    .scrolly-wrapper {
        width: min(1000ch, 60vw);
        position: relative;
        left: 50%;
        transform: translateX(-50%);
    }

    .step {
        min-height: 80vh;
        padding: 2rem;
    }

    .step-content {
        padding: 1.5rem 1.5rem;
        border-left: 3px solid var(--color-accent-low);
    }

    .project-detail {
        padding: 2rem;
        width: 100%;
    }

</style>