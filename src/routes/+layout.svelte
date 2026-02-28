
<script>
    import "../style.css";
    import { page } from "$app/stores";
    import { base } from "$app/paths";
    let root = globalThis.document?.documentElement;
    let localStorage = globalThis.localStorage ?? {};
    let colorScheme = localStorage.colorScheme ?? "light dark";
    $: root?.style.setProperty("color-scheme", colorScheme);
    $: localStorage.colorScheme = colorScheme;

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

<label class="color-scheme-switch">
    Theme:
    <select bind:value={ colorScheme }>
        <option value="light dark">Automatic</option>
        <option value="light">Light</option>
        <option value="dark">Dark</option>
    </select>
</label>

<slot />

<style>

    /* :root {
        color-scheme: dark;
    } */

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
        margin-bottom: 5px;
    }

    a.current {
        border-bottom-style: solid;
        border-bottom-color: var(--color-grey);
        border-bottom-width: 0.4em;
        padding-bottom: 0.1em;
        margin-bottom: -3px;
    }

    a:hover {
        background-color: var(--color-accent-low);
        border-bottom-style: solid;
        border-bottom-color: var(--color-accent-med);
        border-bottom-width: 0.4em;
        padding-bottom: 0.1em;
        margin-bottom: -3px;
    }

    .color-scheme-switch {
        position: absolute;
        top: 1rem;
        right: 1rem;
        gap: 4px;
        font-size: 80%;
        font-family: inherit;
    }

</style>