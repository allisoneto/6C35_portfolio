<script>
  import projects from "$lib/projects.json";
  import Project from "$lib/Project.svelte";
</script>

<svelte:head>
  <title>Projects</title>
</svelte:head>

<nav>
    <ul>
        <li><a href=".">Home</a></li>
        <li><a class="current" href="projects">Projects</a></li>
        <li><a href="resume">About</a></li>
        <li><a href="contact">Contact</a></li>
        <li><a href="https://github.com/allisoneto" target="_blank">GitHub</a></li>
    </ul>
</nav>

<h1>{projects.length} Projects</h1>

<div class="projects">
    {#each projects as p}
    <Project data={p} />
    {/each}
</div>
        <!-- <a href="./a2_report.html" style="text-decoration: none; color: inherit;"> -->
        <!-- <a href="./a2_report.html" style="text-decoration: none; color: inherit;"> -->
    <!-- <article>
        <h2>Repellat sed asperiores ipsa?</h2>
        <img src="https://vis-society.github.io/labs/2/images/empty.svg" alt="">
        <p>Consequuntur, possimus. Debitis, error mollitia, explicabo molestias labore commodi quas voluptates consequuntur ullam dicta reprehenderit, laboriosam maiores in! Neque, quibusdam. Esse, accusantium quis? Vitae enim, libero rem amet ex sapiente.</p>
    </article>
    <article>
        <h2>Alias inventore ipsa non.</h2>
        <img src="https://vis-society.github.io/labs/2/images/empty.svg" alt="">
        <p>Corrupti minima unde ab iusto, ratione enim debitis alias, aliquid rerum aperiam suscipit, rem impedit ullam voluptate inventore ad nobis nisi facilis expedita quia sint fugit! Eveniet dolor minus fuga.</p>
    </article>
    <article>
        <h2>Praesentium similique reprehenderit placeat?</h2>
        <img src="https://vis-society.github.io/labs/2/images/empty.svg" alt="">
        <p>Doloribus assumenda vitae tenetur pariatur voluptates. Voluptatem nostrum esse officiis odit magni voluptas vel repellat? Consequatur, voluptatum mollitia quaerat perspiciatis assumenda dolore rem reiciendis eaque vero sequi fugiat modi cum.</p>
    </article>
    <article>
        <h2>Quidem excepturi iusto nisi?</h2>
        <img src="https://vis-society.github.io/labs/2/images/empty.svg" alt="">
        <p>Dicta, excepturi sed odit repellendus corrupti recusandae maxime molestiae ipsa facere pariatur cupiditate doloribus voluptate deserunt animi minus quas nemo officia similique illo alias ducimus fugit architecto debitis iusto. Debitis!</p>
    </article>
    <article>
        <h2>Perferendis vero vel voluptatem.</h2>
        <img src="https://vis-society.github.io/labs/2/images/empty.svg" alt="">
        <p>Itaque, cupiditate excepturi, sit aperiam animi recusandae enim quae earum dolorem voluptas ullam similique. Dolore amet facere, accusantium quibusdam deleniti earum, voluptatibus neque vel error numquam cumque, enim quis officiis!</p>
    </article>
    <article>
        <h2>Autem cupiditate quas ad.</h2>
        <img src="https://vis-society.github.io/labs/2/images/empty.svg" alt="">
        <p>Ducimus, repellat accusamus? Repellendus fugiat modi quibusdam esse. Quia expedita vel doloribus? Commodi eum optio, atque earum voluptate iste at? Velit quas explicabo doloremque commodi minima impedit incidunt delectus eos.</p>
    </article>
    <article>
        <h2>Doloremque omnis totam voluptatum.</h2>
        <img src="https://vis-society.github.io/labs/2/images/empty.svg" alt="">
        <p>Rerum iure molestias accusamus illum? Esse similique adipisci ex aspernatur voluptatum ut illo incidunt nobis a quas suscipit praesentium, nihil minima corporis accusantium? Voluptatibus reprehenderit cumque nemo nam iusto minus?</p>
    </article>
    <article>
        <h2>Iusto laboriosam dignissimos nisi!</h2>
        <img src="https://vis-society.github.io/labs/2/images/empty.svg" alt="">
        <p>Quod totam cupiditate aut magni corporis consectetur velit aliquid porro aliquam repellendus cumque odit, itaque accusantium atque excepturi error voluptates tenetur dolorum minus. Nulla cum officia, sint eum excepturi voluptas!</p>
    </article>
    <article>
        <h2>Ducimus fugit laboriosam veritatis?</h2>
        <img src="https://vis-society.github.io/labs/2/images/empty.svg" alt="">
        <p>Quidem soluta minima vitae saepe suscipit animi nihil deleniti, voluptatibus sint distinctio nesciunt possimus voluptas cum aliquid ipsam impedit cumque nulla nostrum nisi ipsa. Odio a aliquam autem ipsum fugit.</p>
    </article>
    <article>
        <h2>Autem aspernatur dignissimos voluptatum.</h2>
        <img src="https://vis-society.github.io/labs/2/images/empty.svg" alt="">
        <p>Fugit expedita quos asperiores laborum distinctio veniam tempore dolor aspernatur optio facere doloribus saepe ex, explicabo hic repellat rerum ducimus culpa ipsam dolores velit et? Odit excepturi fugit illo dolorem?</p>
    </article>
    <article>
        <h2>Corrupti molestias odio sit?</h2>
        <img src="https://vis-society.github.io/labs/2/images/empty.svg" alt="">
        <p>Repellat a fugiat numquam iure et omnis rem deserunt, officiis quia id voluptatibus temporibus accusantium laboriosam magnam veritatis distinctio autem, sed quaerat eos nulla ratione. Cumque rerum aperiam commodi sint.</p>
    </article> -->
    <!-- {/each} -->

