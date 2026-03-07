<script>
    import projects from "$lib/projects.json";
    import Project from "$lib/Project.svelte";
    import reading from "$lib/reading.json";
    import ReadingItem from "$lib/ReadingItem.svelte";

    import { onMount } from "svelte";

    let githubData = null; // This will eventually hold our Github stats
    let loading = true; // This will be true *until* the fetch's promise resolves to a value
    let error = null; // If the API call resulted in an error, it will go into this variable

    // function retrieveGithubData(){
    // console.log("Page has been mounted!")
    // }
    onMount(async () => {
            try { // First, try running this block of code
                console.log("Page has been mounted!")
                let response = await fetch("https://api.github.com/users/allisoneto");
                // let response = await {
                //                         ok: true,
                //                         json: async () => ({
                //                             followers: 100,
                //                             following: 100,
                //                             public_repos: 100,
                //                             public_gists: 100,
                //                         }),
                //                         };

                console.log(response);
                githubData = await response.json();
                console.log(githubData);
            } catch (err) { // if the "try" block runs into an error, cancel excecution and run this code instead
                error = err;
            }
            loading = false; // don't forget to add this line!
        }
    );


    // onMount(retrieveGithubData);

    // async function retrieveGithubData(){
    //     try { // First, try running this block of code
    //         console.log("Page has been mounted!")
    //         let response = await fetch("https://api.github.com/users/allisoneto");
    //         console.log(response);
    //         githubData = await response.json();
    //         console.log(githubData);
    //     } catch (err) { // if the "try" block runs into an error, cancel excecution and run this code instead
    //         error = err;
    //     }
    //     loading = false; // don't forget to add this line!
    // }

  </script>

<h1>Allison Eto's 6.C35 Portfolio</h1>

<div class="intro-and-reading">
    <div class="intro">
        <p>This is the beginning of Allison Eto's personal portfolio for MIT's 6.C35 course - Interactive Data Visualization and Society.
            Completed assignments and visualizations will be posted here, as well as some information about Allison.
        </p>

        <p>It is currently under development.</p>

        <img src="images/allison_w_snowman.jpeg" alt="Allison kneeling next to a small snowman" width="500">
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


{#if loading}
    <p>Loading...</p>
{:else if error}
    <p>Something went wrong: {error.message}</p>
{:else}
    <section class="github-stats">
        <h2>My GitHub</h2>
        <dl>
            <dt>FOLLOWERS</dt>
            <dd>{githubData.followers}</dd>
            <dt>FOLLOWING</dt>
            <dd>{githubData.following}</dd>
            <dt>PUBLIC REPOSITORIES</dt>
            <dd>{githubData.public_repos}</dd>
        </dl>
    </section>
{/if}


<h2>Recent Projects</h2>
<div class="projects highlights">
    {#each projects.slice(0, 3) as p}
    <Project data={p} />
    {/each}
</div>
