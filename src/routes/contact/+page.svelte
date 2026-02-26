<svelte:head>
  <title>Contact me</title>
</svelte:head>


<nav>
    <ul>
        <li><a href=".">Home</a></li>
        <li><a href="projects">Projects</a></li>
        <li><a href="resume">About</a></li>
        <li><a class="current" href="contact">Contact</a></li>
        <li><a href="https://github.com/allisoneto" target="_blank">GitHub</a></li>
    </ul>
</nav>

<h1>Contact Form</h1>

<form action="mailto:allisons-backpack@mit.edu" method="GET">
    <label>
        Your Email:
        <input type="email" name="email" value="example@gmail.com">
    </label>

    <label>
        Subject:
        <input name="subject" value="Your subject here">
    </label>

    <label>
        Message:
        <textarea name="body" value="Your message here"></textarea>
    </label>

    <button>Send</button>
</form>
