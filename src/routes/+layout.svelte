
<script>
    import "../style.css";
    import { page } from "$app/stores";
    import { base } from "$app/paths";
    let pages = [
        {url: "/", title: "Home"},
        {url: "/projects", title: "Projects"},
        {url: "/resume", title: "About"},
        {url: "/contact", title: "Contact"},
        {url: "https://github.com/allisoneto", title: "GitHub"},
    ]
</script>


<nav>
    <ul>
        {#each pages as p}
        <li>
            <a href={base + p.url}
                class:current={p.url === "/" // is this link the home page?
                ? $page.url.pathname === (base + "/") // if yes - set current = true if the path name matches. Else, set current = true if the path name starts correctly
                : $page.url.pathname.startsWith(base + p.url)}
                target={p.url.startsWith("http") ? "_blank": null}
            >
                {p.title}
            </a>
        </li>
        {/each}
        <!-- <li><a href="/">Home</a></li>
        <li><a href="/projects">Projects</a></li>
        <li><a href="/resume">About</a></li>
        <li><a href="/contact">Contact</a></li>
        <li><a href="https://github.com/allisoneto" target="_blank">GitHub</a></li> -->
    </ul>
</nav>

<slot />

<style>

    nav {
        display: flex;
        border-bottom-width: 3px;
        border-bottom-style: solid;
        border-bottom-color: var(--color-grey);
        }

    ul,
    li {
    display: contents;
    }

    a {
    flex: 1;
    text-decoration: none;
    color: inherit;
    text-align: center;
    padding: 0.5em;
    margin-bottom: 0.5em;
    }

    a.current {
    border-bottom-style: solid;
    border-bottom-color: var(--color-grey);
    border-bottom-width: 0.4em;
    padding-bottom: 0.1em;
    margin-bottom: -2px;
    }

    a:hover {
    background-color: var(--color-accent-light);
    border-bottom-style: solid;
    border-bottom-color: var(--color-accent-dark);
    border-bottom-width: 0.4em;
    padding-bottom: 0.1em;
    margin-bottom: -2px;
    }

</style>